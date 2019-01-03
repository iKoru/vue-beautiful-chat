<template>
  <div class="sc-message--text" :style="messageColors">
    <p class="sc-message-data-text" v-html="messageText"></p>
    <p v-if="data.meta" class="sc-message--meta" :style="{color: messageColors.color}">{{data.meta}}</p>
  </div>
</template>

<script>
import escapeGoat from 'escape-goat'
import Autolinker from 'autolinker'
const fmt = require('msgdown')

export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    }
  },
  computed: {
    messageText() {
      const escaped = escapeGoat.escape(this.data.text)

      return Autolinker.link(this.messageStyling ? fmt(escaped) : escaped, {
        className: 'chatLink',
        truncate: { length: 50, location: 'smart' }
      })
    }
  }
}
</script>

<style scoped>
.sc-message-data-text {
  margin-bottom: 5px;
}
a.chatLink {
  color: inherit !important;
}
</style>
