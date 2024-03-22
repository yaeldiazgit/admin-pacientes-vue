<script setup>
    import { reactive, computed } from "vue";
    import Alerta from "./Alerta.vue";


    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        },
    });
    
    const alerta = reactive({
        mensaje: '',
        tipo: ''
    })

    const validar = () => {
        if(Object.values(props).includes('')){
            alerta.mensaje = 'Todos los campos son obligatorios';
            alerta.tipo = 'error';
            return
        }
        emit("guardar-pacientes");
        alerta.mensaje = 'Guardado correctamente...';
        alerta.tipo = 'ok';

        setTimeout(() => {
            Object.assign(alerta, {
                mensaje: '',
                tipo: ''
            })
        }, 1500);
    }
    const botonRegistrar = computed(() => {
        return props.id ? 'Actualizar Paciente' : 'Registrar Paciente';
    });

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-pacientes']);

</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center ">Seguimiento Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y <span class="text-indigo-600 font-bold">Administralos</span>
        </p>
        <Alerta
            v-if="alerta.mensaje"
            :alerta="alerta"
        />
        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar">
            <div class="mb-5">
                <label 
                    for="mascota"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Mascota
                </label>
                <input 
                    type="text" 
                    id="mascota"
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 placeholder-gray-400 rounded-md"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                />


                <label 
                    for="propietario"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre del Propietario
                </label>
                <input 
                    type="text" 
                    id="propietario"
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 placeholder-gray-400 rounded-md"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                />


                <label 
                    for="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                    type="email" 
                    id="email"
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 placeholder-gray-400 rounded-md"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                />


                <label 
                    for="alta"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Alta
                </label>
                <input 
                    type="date" 
                    id="alta"
                    class="border-2 w-full p-2 placeholder-gray-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                />


                <label 
                    for="sintomas"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                <textarea 
                    id="sintomas"
                    placeholder="Describe los sintomas"
                    class="border-2 w-full p-2 placeholder-gray-400 rounded-md h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                />

                <input 
                    type="submit"
                    class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 
                    cursor-pointer transition-colors " 
                    :value="botonRegistrar"/>
            </div>

        </form>

    </div>
</template>
