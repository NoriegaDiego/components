<script setup>
import {ref, computed, onMounted} from "vue"

import ButtonCounter from './components/ButtonCounter.vue';
import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(false);

const favorito = ref("");

const cambiarFavorito = (title) =>{
  favorito.value = title
}

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
};

onMounted(async() => {
  
  loading.value=true;
  try{
    const res = await fetch("https://jsonplaceholder.typicode.com/posts")
    posts.value = await res.json();
  }
  catch (error){
    console.log(error)
  } finally {
    setTimeout(() =>{
        loading.value=false;
      }, 2000);
  }
  });
  
  
  /*fetch("https://jsonplaceholder.typicode.com/posts")
    .then(res => res.json())
    .then((data) => posts.value=data)

    .catch((e) => console.log(e))
    .finally(() => {
      setTimeout(() =>{
        loading.value=false;
      }, 2000);
  }); */


const prev = () => {
  inicio.value += -postXpage;
  fin.value += -postXpage;

}

const maxlength = computed(() => posts.value.length);



</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
<h1>App</h1>
<h2>Mis post favoritos: {{ favorito }}</h2>

<PaginatePost 
@next="next" 
@prev="prev" 
:inicio="inicio" 
:fin="fin"
:maxlength="maxlength"
class="mb-2"/>

  <BlogPost 
  v-for="post in posts.slice(inicio, fin)"
  :key="post.id"
  :title="post.title" 
  :id="post.id" 
  :body="post.body"
  :cambiarFavorito="cambiarFavorito"
  class="mb-2"
  >
</BlogPost>
  
  
  </div>
  
</template>