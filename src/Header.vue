<template>
  <div class="sc-header" :style="{background: colors.header.bg, color: colors.header.text}">
    <img class="sc-header--img" :src="imageUrl" alt v-if="imageUrl">
    <div :class="{'sc-header--title':true, 'ellipsis':true, 'has-user-list':hasUserList}" @click="toggleUserList">{{title}}<br/><small>이 채팅방은 실시간으로 반영되지 않습니다. <span @click="loadNewMessages" v-if="!loading">새로고침</span><i v-else aria-hidden="true" class="v-icon material-icons theme--light">refresh</i></small></div>
    <div class="sc-header--close-button" @click="onClose">
      <img src="./assets/close-icon.png" alt>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    imageUrl: {
      type: String,
      required: true
    },
    title: {
      type: String
    },
    onClose: {
      type: Function,
      required: true
    },
    colors: {
      type: Object,
      required: true
    },
    hasUserList: {
      type: Boolean,
      default: true
    },
    loadNewMessages:{
      type:Function,
      default: () => []
    },
    loading:{
      type: Boolean,
      default: false
    }
  },
  methods: {
    toggleUserList() {
      if (this.hasUserList) {
        this.inUserList = !this.inUserList
        this.$emit('userList', this.inUserList)
      }
    }
  },
  data() {
    return {
      inUserList: false
    }
  }
}
</script>
<style>
.sc-header {
  min-height: 60px;
  border-top-left-radius: 9px;
  border-top-right-radius: 9px;
  padding: 10px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.2);
  position: relative;
  box-sizing: border-box;
  display: flex;
}

.sc-header--img {
  border-radius: 50%;
  align-self: center;
  padding: 10px;
}

.sc-header--title {
  align-self: center;
  padding: 10px;
  flex: 1;
  user-select: none;
  border-radius: 5px;
}

.sc-header--title.has-user-list {
  cursor: pointer;
}

.sc-header--title.has-user-list:hover {
  box-shadow: 0px 2px 5px rgba(0.2, 0.2, 0.5, 0.1);
}

.sc-header--close-button {
  width: 40px;
  align-self: center;
  height: 40px;
  box-sizing: border-box;
  cursor: pointer;
  border-radius: 50%;
  margin-left: auto;
}

.sc-header--close-button:hover {
  box-shadow: 0px 2px 5px rgba(0.2, 0.2, 0.5, 0.1);
}

.sc-header--close-button img {
  width: 100%;
  height: 100%;
  padding: 13px;
  box-sizing: border-box;
}

@media (max-width: 600px) {
  .sc-header {
    border-radius: 0px;
  }
}
</style>
