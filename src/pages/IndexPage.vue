<template>
  <div class="q-pa-md">
    <q-input
          v-model="text"
          outlined
          label="Response"
          autogrow
          readonly
          class="text-field"
        />

    <div class="q-gutter-md">
      <q-input
        v-model="inputValue"
        outlined
        dense
        placeholder="Send a message"
        hint="Write here your message to send to llama chat"
      >
        <template v-slot:after>
          <q-btn flat color="primary" icon="send" @click="handleButtonClick" />
        </template>
      </q-input>
    </div>
  </div>
</template>

<style>
.text-field {
  flex: 1;
  height: 100%;
  margin-bottom: 50px;
}
</style>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',

  data () {
    return {
      inputValue: '',
      text: ''
    }
  },

  methods: {
    handleButtonClick () {
      // Add code here to handle the button click event
      console.log('Input Value:', this.inputValue)
      // Effectuez la requête POST
      axios.post('http://localhost:8000', '{"request": "' + this.inputValue + '"}')
        .then((response) => {
          this.response = response.data
          console.log('response:', this.response)
          this.text = this.response.response
        })
        .catch((error) => {
          console.error(error)
        })
    }
  }

})

</script>
