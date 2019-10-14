<template>
  <div class="app">
    <div>
      <gin-input v-model="url" label="url" />
    </div>
    <div>
      <gin-input v-model="payload" label="payload" />
    </div>
    <select v-model="method">
      <option value="get"> GET </option>
      <option value="post"> POST </option>
      <option value="patch"> PATCH </option>
      <option value="delete"> DELETE </option>
    </select>
    <div>
    <gin-button @click="send">
      Send
    </gin-button>
    </div>

    <div v-text="data" v-if="data" />
  </div>
</template>

<script>
import { GinInput, GinButton } from '@aubrgin/gin-components';
import axios from 'axios';

export default {
  name: 'GinAppHttpClient',
   components: {
     GinInput,
     GinButton,
   },
   data() {
     return {
       url: '',
       method: '',
       payload: '',
       data: undefined,
       loading: false,
     }
   },
   methods: {
     async send() {
       this.loading = true;
       const response = await axios[this.method](this.url, { params: this.payload});
       this.data = response.data;
       this.loading = false;
     },
   },
};
</script>

<style>
</style>
