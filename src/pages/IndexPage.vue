<template>
  <div class="q-pa-md">
    <q-input
      v-model="text"
      outlined
      autogrow
      readonly
      class="text-field"
    >
      <template v-slot:append>
        <q-btn dense flat color="negative" icon="clear" @click="handleClearButtonClick" />
        <q-btn class="icon" dense flat color="primary" icon="img:src/assets/copy.svg" @click="copyToClipboard" />
      </template>
    </q-input>

    <div class="q-gutter-md">
      <q-input
        v-model="inputValue"
        outlined
        dense
        placeholder="Send a message"
        hint="Write here your message to send to llama chat"
        @keyup.enter="handleButtonClick"
      >
        <template v-slot:append>
          <q-btn dense flat color="primary" icon="send" @click="handleButtonClick" />
        </template>
      </q-input>
    </div>
  </div>
</template>

<style>
.text-field {
  margin-bottom: 50px;
}
.icon {
  color: #1976D2;
}
</style>

<script>
import axios from 'axios'
import { copyToClipboard } from 'quasar'
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'IndexPage',

  data () {
    return {
      inputValue: '',
      text: ref(''),
      current_text: ''
    }
  },

  methods: {
    handleButtonClick () {
      // Add code here to handle the button click event
      console.log('Input Value:', this.inputValue)
      this.current_text = this.text
      this.text = this.current_text + ' - ' + this.inputValue
      // Effectuez la requête POST
      axios.post('http://localhost:8000', '{"request": "' + this.inputValue + '"}')
        .then((response) => {
          this.response = response.data
          console.log('response:', this.response)
          this.current_text = this.text
          this.text = this.current_text + '\n' + '  > ' + this.response.response + '\n\n'
        })
        .catch((error) => {
          console.error(error)
        })
      this.inputValue = ''
    },
    handleClearButtonClick () {
      this.text = ''
    },
    copyToClipboard () {
      copyToClipboard(this.text).then(() => {
        // success!
      }).catch(() => {
        // fail
      })
    }
  }

})

</script>
