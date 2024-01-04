<template>
    <div>
        <select v-model="typeOfTask" placeholder="Кратое описание задачи">
            <option>учил JS</option>
            <option>учил TS</option>
            <option>страдал хернёй</option>
        </select>
      <input v-model="message" placeholder="Кратое описание задачи">
      <button @click="submitForm">Отправить</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        typeOfTask: null,
        message: '',
      };
    },
    methods: {
      submitForm() {
        console.log(this.typeOfTask, this.message);
        this.$emit('formSubmitted')
        /*
        Передать данные: this.$emit('formSubmitted', {
          typeOfTask: null,
          message: '',
        })
        */
      },
    },
  };
  </script>