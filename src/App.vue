<script setup>
  import { ref, reactive, watch, onMounted } from "vue";
  import { uid } from "uid";
  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Paciente from './components/Paciente.vue'

const paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: ''
});

  let pacientes = ref([]);

  watch(pacientes, () => {
    guardarLocalStorage();
  }, {
    deep: true
  });
  onMounted(() => {
    const storagePacientes = localStorage.getItem('pacientes');
    if(storagePacientes){
      pacientes.value = JSON.parse(storagePacientes);
    }
  });
  const guardarPacientes = () => {
    if(paciente.id){      
      const { id } = paciente;
      const index = pacientes.value.findIndex((auxPaciente) => auxPaciente.id === id);
      pacientes.value[index] = {...paciente}
    } else {
      pacientes.value.push({
        ...paciente,
        id: uid()
      });

    }
    Object.assign(paciente, {
      nombre: '',
      propietario: '',
      email: '',
      alta: '', 
      sintomas: '',
      id: null
    });
  }

  const actualizarPaciente = (id) => {
    const pacienteEditar = pacientes.value.filter((paciente) => paciente.id === id)[0];
    Object.assign(paciente, pacienteEditar);
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter((eliminarPaciente) => eliminarPaciente.id !== id);
    console.log("eliminar");
  }
  const guardarLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
  }
</script>

<template>
  <div class="container mx-auto mt-20 ">
    <Header/>

    <div class="mt-12 md:flex">
      <Formulario
        :id="paciente.id"
        v-model:nombre="paciente.nombre" 
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email" 
        v-model:alta="paciente.alta" 
        v-model:sintomas="paciente.sintomas"
        @guardar-pacientes="guardarPacientes" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administrar tus Pacientes
        </h3>

        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Paciente 
            v-for="paciente in pacientes" 
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">
          No hay pacientes
        </p>

      </div>
    </div>
  </div>
</template>


