<template>
  <div class="gin-http-client">
    <div class="field">
      <gin-label label="URL" />
      <gin-input v-model="host" label="Host" />
      <gin-input v-model="route" label="Route" />
    </div>
    <div class="field">
      <div class="headers-tree">
        <gin-tree label="Headers" v-model="headers" :edit="true" />
      </div>
    </div>
    <div class="field">
      <div class="payload-tree">
        <gin-tree label="Payload" v-model="payload" :edit="true" />
      </div>
    </div>
    <div class="field">
      <gin-label label="Method" />
      <gin-select v-model="method" :options="methods" />
    </div>
    <div>
    <gin-button @click="send">
      Send
    </gin-button>
    </div>

    <gin-tree label="Data" class="data" :value="data" v-if="data" />
  </div>
</template>

<script>
import { GinInput, GinButton, GinTree, GinLabel, GinSelect } from '@aubrgin/gin-components';
import axios from 'axios';

export default {
  name: 'GinAppHttpClient',
   components: {
     GinInput,
     GinButton,
     GinTree,
     GinLabel,
     GinSelect,
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
       methods: [
         'get',
         'post',
         'patch',
         'put',
         'delete',
       ],
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
       this.data = response;

       this.loading = false;
     },
   },
};
</script>

<style lang="scss">
 .gin-http-client {
   .field {
     margin: 16px;

     .headers-tree {
       width: 60%;
       margin: 0px;
     }
   }

   .data {
     font-family: monospace;
   }
 }
</style>
