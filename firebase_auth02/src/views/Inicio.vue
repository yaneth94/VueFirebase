<template>
    <div class="container">
    <h1>Lista de tareas</h1>
    <h3>Bienvenido: {{usuario.email}}</h3>
    <router-link :to="{name: 'Agregar'}">
      <button class="btn btn-success btn-block">Agregar</button>
    </router-link>
    <div v-if="carga" class="text-center mt-5">
      <h3>Cargando contenido ...</h3>
      <pulse-loader :loading="carga" ></pulse-loader>
    </div>
    <form @submit.prevent="buscador(busqueda)">
      <input type="text" placeholder="Buscar Tarea" class="form-control mt-5" v-model="busqueda" v-on:keyup="buscador(busqueda)">
    </form>
    <ul class="list-group mt-5" v-if="!carga"> 
      <li class="list-group-item" 
      v-for="item of arrayFiltrado" :key="item.id">
        {{item.id}} - {{item.nombre}}
        <div class="float-right">
          <router-link class="btn btn-warning btn-sm mr-2" 
          :to="{name: 'Editar', params:{ id: item.id}}">
            Editar
          </router-link>
          <button @click="eliminarTarea(item.id)" class="btn btn-danger btn-sm">Eliminar</button>
        </div>  
      </li>
    </ul>
  </div>

</template>

<script>
import { mapState,mapActions,mapGetters } from 'vuex'
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'

export default {
    name: 'Inicio',
    data(){
      return{
        busqueda: ''
      }
    },
    components: {
      PulseLoader
    },
    computed: {
        ...mapState(['usuario','tareas','carga']),
        ...mapGetters(['arrayFiltrado'])
    },
    methods: {
        ...mapActions(['getTareas','eliminarTarea','buscador'])
    },
    created(){
        this.getTareas();
    }
}
</script>