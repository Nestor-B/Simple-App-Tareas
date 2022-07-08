<template>
    <header class="my-3 text-center">
        <h1 class="display-3 text-dark">App Tareas</h1>
        <p>Crear, editar y eliminar tareas.</p>
    </header>

    <section class="my-1 px-3 p-2">
        <div class="row">
            <div class="col-md-3"></div>
            <div class="col-md-6">
                <div class="d-none my-2 mx-2 d-flex justify-content-end"> 
                    Total:<b class="mx-1">
                        {{store.state.tareas.length}}
                    </b>tareas
                </div>
                <form action="">
                    <input type="text" ref="tagInput" v-model="input" class="form-control" placeholder="Nueva tarea" />
                    <input ref="tagButtonSubmit" type="submit" @click.prevent="()=>{
                        store.add({
                            id: Date.now(), 
                            text: input, 
                            editing: false, 
                            done: false,
                            timer: 5
                        }); clearInput()
                    }" class="btn d-block btn-primary my-2" value="Agregar Tarea" />
                </form>
            </div>
            <div class="col-md-3"></div>
        </div>
    </section>

    <section class="px-3">
        <div class="row">
            <div class="col-md-3"></div>
            <div class="col-md-6">
                <h2 class="text-center">Lista de tareas</h2>
                <div class="position-relative d-flex my-1 p-1 px-4 justify-content-between align-items-center border" v-for="tarea, index in store.order()" :key="tarea" 
                    :class=" [tarea.timer == 1?'quit':''] "
                >
                    <div> 
                        <span class="text-secondary">{{ store.order().length - index }}</span> 
                        <i class="bi bi-dash"></i> {{tarea.text}}
                    </div>
                    <div class="d-flex align-items-center">
                        <button class="btn">
                            <b v-if="tarea.done" class="text-success">  
                                <div class="timer" title="Eliminando ...">{{tarea.timer}}</div>
                                Realizado 
                            </b>
                        </button>
                        <button :disabled="tarea.done" class="btn" @click="()=>{tarea.editing = !tarea.editing; tarea.editing?edit(tarea.text):clearInput('')}" title="Guardar Cambios">
                            <span v-if="tarea.editing" 
                                class="text-danger"
                                @click="store.edit(tarea.id, input)">
                                Guardar
                            </span>
                            <span v-else title="Editar"><i class="bi bi-pencil-square"></i></span> 
                        </button>
                        <button :disabled="tarea.done" class="btn" @click="store.delete(tarea.id)" title="Eliminar">
                            <i class="bi bi-trash3"></i>
                        </button>
                        <button class="btn text-danger" title="Finalizar tarea"> 
                            <input type="checkbox" @change="() => {tarea.done = !tarea.done;tarea.timer = 5 }" />
                        </button>
                    </div>
                </div>
                <p class="text-center">
                    {{store.order().length?'':'No hay tareas.'}}
                </p>
            </div>
            <div class="col-md-3"></div>
        </div>
    </section>
</template>

<script>
    import { ref } from '@vue/reactivity'

    export default {
        name: 'TareasVue',
        inject: ['store'],
        setup(){
            const input = ref('')
            const tagInput = ref(null)
            const tagButtonSubmit = ref(null)
            
            const clearInput = () => {
                input.value = ''
                tagInput.value.focus()
                tagButtonSubmit.value.disabled = false
            }
            const edit = (e) => {
                input.value = e
                tagButtonSubmit.value.disabled = true
            }

            return {input, clearInput, tagInput, tagButtonSubmit, edit}
        }
    }
</script>

<style>
    .timer {
        width: 50px;
        height: 50px;
        border-radius: 50%;
        border: 5px double transparent;
        position: absolute;
        line-height: 40px;
        color: red;
        right: -22px;
        top: -22px;
        transform: scale(1);
        animation: scala 444ms infinite linear;
        filter: drop-shadow(1px 1px 15em rgba(222, 0, 0, .2));
        box-shadow: inset 0 0 0 1px rgba(0,0,0,.3), 0 0 0 1px rgba(222, 0, 0, .2);
    }
    @keyframes scala {
        20% {transform: scale(1.04);border: 5px double transparent;border-top: 5px solid rgb(222, 0, 0);}
        40% {transform: scale(1.04);border: 5px double transparent;border-right: 5px solid rgb(222, 0, 0);}
        60% {transform: scale(1.04);border: 5px double transparent;border-bottom: 5px solid rgb(222, 0, 0);}
        80% {transform: scale(1.04);border: 5px double transparent;border-left: 5px solid rgb(222, 0, 0);}
    }
</style>