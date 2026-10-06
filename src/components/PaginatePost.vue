<script setup>

defineProps(["inicio", "fin", "maxlength"]);

const emit = defineEmits(['next', 'prev'])

const activarSiguiente = () => {
    console.log("me dise a siguiente");
    emit("next");
}
</script>

<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button @click="emit('prev')" 
        type="button" 
        class="btn btn-outline-primary" 
        :disabled="inicio === 0">Previous{{ inicio }}</button>

        <button @click="activarSiguiente" 
        type="button" 
        class="btn btn-outline-primary"
        :disabled="fin >=maxlength">Next {{ fin }}</button>

    </div>
</template>