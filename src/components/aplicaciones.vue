<template>
  <div class="container">
    <div class="form">
        <h4 class="form__title">Ingresar información de la aplicación</h4>
        <form class="box box__form" action="" method="post">
            <label for="name"></label>
            <input v-model="values.name" class="box box__input" type="text" id="name" name="name" placeholder="Nombre de la aplicación"><br>

            <label for="port"></label>
            <input v-model="values.port" class="box box__input" type="text" id="port" name="port" placeholder="Puerto de la aplicación"><br>

            <label for="state"></label>
            <input v-model="values.state" class="box box__input" type="text" id="state" name="state" placeholder="Estado de la aplicación"><br>

            <label for="type"></label>
            <input v-model="values.typeApp" class="box box__input" type="text" id="typeApp" name="typeApp" placeholder="Tipo de aplicación"><br>

            <label for="language"></label>
            <input v-model="values.lang" class="box box__input" type="text" id="lang" name="lang" placeholder="Lenguaje de la aplicación"><br>

            <input @click="sendInfo" class="box button" type="button" value="Enviar">
        </form>
    </div>

    <div v-for="value in values" :key="value.id">
        <div class="box box__device">
            <h5 class="container__text">Nombre del dispositivo: {{value.name}} </h5>
            <h5 class="container__text">Puerto: {{value.port}} </h5>
            <h5 class="container__text">Estado: {{value.state}} </h5>
            <h5 class="container__text">Tipo de aplicación: {{value.typeApp}} </h5>
            <button @click="deleteInfo(value._id)" class="box button container__button">Borrar</button>
        </div>
    </div>
  </div>
</template>

<script>
import PostService from '@/PostService.js';
import "@/assets/styles/global.css";

export default {
  name: 'Dispositivos',
  data() {
    return {
      values: {
        name: '',
        port: '',
        state: '',
        typeApp: '',
        lang: '',
      },
      error: '',
      text: ''
    }
  },
  async created() {
      try {
        this.values = await PostService.getPosts();
      }
      catch(err) {
        this.error = err.message;
      }
  },
  methods: {
      async sendInfo() {
        console.log('llego');
        console.log(
          this.values
        );
        await PostService.insertPost(this.values);
        this.values = await PostService.getPosts();
        window.location.reload();
      },
      async deleteInfo(id) {
        await PostService.deletePost(id);
        this.values = await PostService.getPosts();
        window.location.reload();
      },
  },
}
</script>
