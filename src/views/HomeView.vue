<script setup lang="ts">
import axios from 'axios';
import { ref } from 'vue';
let response = ref();

axios.get("/attacks?limit=30").then(resp => {
  response.value = resp.data;
});

</script>
<template>
  <main>
   <div class="attack" v-for="elem in response">
    <b>From: {{ elem.sourceDetails.ip }} / {{ elem.sourceDetails.reversedns }}</b>
    <div class="events">
      <div class="event" v-for="event in elem.logEvents">
        
        <div v-if="event.eventtype === 'password_auth'">
          <p>Username: {{  event.event.user }}    Password: {{  event.event.password }}</p>
        </div>
        <div v-else-if="event.eventtype === 'exec'">
           {{  event.event.command }}
        </div>
        <div v-else>
          {{ event.eventtype }}
        </div>
      </div>
    </div>
   </div>

  </main>
</template>

<style>
  .attack {
    padding: 15px;
    background-color: #eee;
    margin-bottom: 20px;
  }


  .event p {
    font-weight: bold;
  }
</style>

