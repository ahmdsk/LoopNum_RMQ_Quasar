<template>
  <q-page class="flex column flex-center">
    <div class="text-h6 text-center">Monitor Consume Count String 1-20 From RabbitMQ</div>
    <div class="q-pa-md container-textarea">
      <q-input v-model="text" filled type="textarea" readonly />
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import mqtt from 'mqtt'
import { mqttConfig, urlMQTT } from '../lib/mqtt'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      text: '',
      client: mqtt.connect(urlMQTT, mqttConfig)
    }
  },
  created() {
    this.client.on("connect", () => {
      this.text += 'Berhasil Menghubungkan ke RabbitMQ ... \n\n';
    });

    this.client.subscribe("routing_edp_ahmad", (err) => {
      if (!err) {

      }
    });

    this.client.on("message", (topic, message) => {
      this.text += `${message.toString()} \n`;
    });
  }
})
</script>

<style>
.container-textarea {
  width: 600px;
  overflow-y: scroll;
}

textarea {
  height: 300px;
}

@media screen and (max-width: 500px) {
  .container-textarea {
    width: 100%;
  }
}
</style>