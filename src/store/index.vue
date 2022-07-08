<script>
    import { reactive } from "@vue/reactivity";
    import Swal from "sweetalert2";

    const state = reactive({
        tareas: [],
    });

    const store = {
        state,
        order( e = 1 ){
            if(e){
                return store.state.tareas.sort((a,z) => z.id - a.id)
            }else{
                return store.state.tareas
            }
        },
        complete(){
            store.state.tareas.filter( (e)=> {
                if(e.done){
                    e.timer<=1?store.delete(e.id, true):e.timer -= 1 
                }
            } )
            // console.log(store.state.tareas)
            setTimeout(() => {store.complete()}, 1000)
        },
        add(e) {
            if (e.text.length > 0) {
                store.state.tareas.push(e);
            }
        },
        delete(tarea, timer = false) {
            if(timer){
                store.state.tareas = store.state.tareas.filter((e) => e.id !== tarea);
            }else{
                Swal.fire({
                    title: "Está usted seguro?",
                    text: "La tarea será eliminada!",
                    icon: "warning",
                    showCancelButton: true,
                    confirmButtonColor: "#3085d6",
                    cancelButtonColor: "#d33",
                    confirmButtonText: "Sí, eliminarlo!",
                }).then((result) => {
                    if (result.isConfirmed) {
                        store.state.tareas = store.state.tareas.filter((e) => e.id !== tarea);
                    }
                });
            }
        },
        edit(id, text) {
            if (text.length > 0) {
                store.state.tareas.filter((e) => {
                    if (e.id == id && e.text !== text) {
                        Swal.fire({
                            title: "Está usted seguro?",
                            text: "La tarea será modificada!",
                            icon: "warning",
                            showCancelButton: true,
                            confirmButtonColor: "#3085d6",
                            cancelButtonColor: "#d33",
                            confirmButtonText: "Sí, modificarlo!"
                        }).then((result) => {
                            if( result.isConfirmed ) {
                                e.text = text;
                            }
                        })
                    }
                });
            }
        },
    };

    store.complete()

    export default store;
</script>
