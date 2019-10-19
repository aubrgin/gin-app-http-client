<template>
  <div class="gin-http-client">
    <div class="url">
      <gin-input v-model="url" label="url" />
    </div>
    <div class="headers">
      Headers
      <div class="headers-tree">
        <gin-tree v-model="headers" />
      </div>
    </div>
    <div class="payload">
      Payload
      <div class="payload-tree">
        <gin-tree v-model="payload" />
      </div>
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

    <gin-tree class="data" :value="data" v-if="data" />
  </div>
</template>

<script>
import { GinInput, GinButton, GinTree } from '@aubrgin/gin-components';
import axios from 'axios';

export default {
  name: 'GinAppHttpClient',
   components: {
     GinInput,
     GinButton,
     GinTree,
   },
   data() {
     return {
       url: '',
       method: 'get',
       payload: {},
       headers: {},
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

<style lang="scss">
 .gin-http-client {
   .url {
     margin: 16px;
   }

   .headers {
     margin: 16px;

     .headers-tree {
       width: 60%;
       margin: 0px;
     }
   }

   .payload {
     margin: 16px;

     .payload-tree {
       width: 60%;
       margin: 0px;
     }
   }

   .data {
     font-family: monospace;
   }
 }
</style>
