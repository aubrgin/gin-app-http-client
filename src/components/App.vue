<template>
  <div class="gin-http-client">
    <div class="url">
      <gin-label label="URL" />
      <gin-input v-model="host" label="Host" />
      <gin-input v-model="route" label="Route" />
    </div>
    <div class="headers">
      <div class="headers-tree">
        <gin-tree label="Headers" v-model="headers" :edit="true" />
      </div>
    </div>
    <div class="payload">
      <div class="payload-tree">
        <gin-tree label="Payload" v-model="payload" :edit="true" />
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

    <gin-tree label="Data" class="data" :value="data" v-if="data" />
  </div>
</template>

<script>
import { GinInput, GinButton, GinTree, GinLabel } from '@aubrgin/gin-components';
import axios from 'axios';

export default {
  name: 'GinAppHttpClient',
   components: {
     GinInput,
     GinButton,
     GinTree,
     GinLabel,
   },
   computed: {
     url() {
       return `${this.host}/${this.route}`;
     },
   },
   data() {
     return {
       route: '',
       host: '',
       method: 'get',
       payload: {},
       headers: {},
       data: {},
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

     .gin-label {
       margin-bottom: 16px;
     }

     .gin-input-container {
       margin-bottom: 32px;
     }
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
