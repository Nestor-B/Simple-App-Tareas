<template>
    <TareasVue />
    <br />
</template>

<script>
    import TareasVue from './components/Tareas.vue'
    import store from "./store"
    export default {
        name: "App",
        components: {
            TareasVue,
        },
        provide(){
            return {
                store
            }
        }
    };
</script>