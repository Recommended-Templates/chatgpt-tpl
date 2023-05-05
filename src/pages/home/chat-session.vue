<style lang="scss">
.chat-session {
  top: 130px;
  left: -260px;
  width: 240px;
  z-index: 100;
  .session-item {
    padding: 15px;
    display: flex;
    align-items: center;
    cursor: pointer;
    &:hover {
      background: #f9f9f9;
    }
    &.active {
      background: #f0f0f0;
      font-weight: bold;
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
    <div
      class="session-item bdb-1"
      :class="{
        active: curId == it.id,
      }"
      @click="onItem(it)"
      v-for="it in list"
      :key="it.id"
    >
      <img src="img/chat.svg" width="20" />
      <span class="flex-1 line-1">Chat {{ it.id + 1 }}</span>
    </div>
    <div class="session-item" @click="onAdd">
      <img src="img/add.svg" width="20" />
      <b>New Session</b>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: {
    ...mapState({
      noticeMsg: (s) => s.noticeMsg,
      streamingId: (s) => s.streamingId,
    }),
  },
  data() {
    return {
      list: JSON.parse(localStorage.sessionList || "[]"),
      curId: localStorage.sessionId || 0,
      showMob: false,
    };
  },
  created() {
    if (!this.list.length) {
      this.onAdd();
    }
  },
  watch: {
    noticeMsg({ name }) {
      if (name == "clearMsg") {
        this.onClear();
      }
    },
  },
  methods: {
    async onItem(it) {
      if (this.streamingId && this.streamingId != it.id) {
        const val = confirm("Are you sure to abort this chat session?");
        if (!val) return;
        this.$setMsg("abort-chat");
        await this.$sleep(300);
      }
      this.$setMsg("change-session");
      this.curId = it.id;
      localStorage.sessionId = it.id;
      this.$emit("close");
    },
    onAdd() {
      if (this.list.length >= 5) {
        return alert("Limit 5 to maximum");
      }
      const ids = this.list.map((it) => it.id);
      const maxId = Math.max(-1, ...ids);
      const id = maxId + 1;
      const item = {
        id,
      };
      this.list.push(item);
      this.storList();
      this.onItem(item);
    },
    storList() {
      localStorage.sessionList = JSON.stringify(this.list);
    },
    onClear() {
      if (this.list.length == 1) return;
      let idx = 0;
      this.list.forEach((it, i) => {
        if (it.id == this.curId) idx = i;
      });
      this.list.splice(idx, 1);
      localStorage.removeItem("msgList" + this.curId);
      this.storList();
      idx = idx == 0 ? 0 : idx - 1;
      this.onItem(this.list[idx]);
    },
  },
};
</script>