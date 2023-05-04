<style lang="scss">
.chat-session {
  top: 130px;
  left: 50px;
  width: 240px;
  z-index: 100;
  .session-item {
    padding: 15px;
    display: flex;
    align-items: center;
    cursor: pointer;
    &:hover {
      background: #f6f6f6;
    }
    img {
      margin-right: 10px;
    }
  }
  .bdb-1 {
    border-bottom: 1px solid #eee;
  }
}
</style>


<template>
  <div class="pos-a chat-session bg-white">
    <div class="session-item bdb-1">
      <img src="img/chat.svg" width="20" />
      <span>Session 1</span>
    </div>
    <div class="session-item">
      <img src="img/add.svg" width="20" />
      <b>New Session</b>
    </div>
  </div>
</template>