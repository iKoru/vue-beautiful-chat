<template>
  <div class="sc-message-list" ref="scrollList" :style="{backgroundColor: colors.messageList.bg}" @scroll="scrollMessageList">
    <Message v-for="(message, idx) in messages" :message="message" :chatImageUrl="chatImageUrl(message.author)" :authorName="authorName(message.author)" :key="idx" :colors="colors" :messageStyling="messageStyling"/>
    <Message v-show="showTypingIndicator !== ''" :message="{author: showTypingIndicator, type: 'typing'}" :chatImageUrl="chatImageUrl(showTypingIndicator)" :colors="colors" :messageStyling="messageStyling"/>
  </div>
</template>
<script>
import Message from './Message.vue'
import chatIcon from './assets/chat-icon.svg'

export default {
  components: {
    Message
  },
  props: {
    participants: {
      type: Array,
      required: true
    },
    messages: {
      type: Array,
      required: true
    },
    showTypingIndicator: {
      type: String,
      required: true
    },
    colors: {
      type: Object,
      required: true
    },
    alwaysScrollToBottom: {
      type: Boolean,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    },
    loadPreviousMessages:{
      type: Function,
      default: () => []
    }
  },
  data(){
    return {
      scrolls:null
    }
  },
  methods: {
    _scrollDown() {
      this.$refs.scrollList.scrollTop = this.$refs.scrollList.scrollHeight
    },
    shouldScrollToBottom() {
      return (
        this.alwaysScrollToBottom ||
        this.$refs.scrollList.scrollTop >
          this.$refs.scrollList.scrollHeight - 600
      )
    },
    scrollMessageList(){
      if(this.scrolls !== null){
        clearTimeout(this.scrolls);
      }
      if(this.$refs.scrollList.scrollTop === 0){
        this.scrolls = setTimeout(() => {
          this.loadPreviousMessages();
        }, 500)
      }
    },
    profile(author) {
      const profile = this.participants.find(profile => profile.id === author)

      // A profile may not be found for system messages or messages by 'me'
      return profile || { imageUrl: '', name: '' }
    },
    chatImageUrl(author) {
      return this.profile(author).imageUrl
    },
    authorName(author) {
      return this.profile(author).name
    }
  },
  computed: {
    defaultChatIcon() {
      return chatIcon
    }
  },
  mounted() {
    this._scrollDown()
  },
  updated() {
    if (this.shouldScrollToBottom()) {
      this.$nextTick(this._scrollDown())
    }
  }
}
</script>

<style scoped>
.sc-message-list {
  height: calc(100% - 110px);
  overflow-y: auto;
  background-size: 100%;
  padding-top: 10px;
  padding-left: 15px;
  padding-right: 10px;
}
</style>
