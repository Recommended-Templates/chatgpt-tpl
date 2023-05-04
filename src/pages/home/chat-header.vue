<template>
  <div>
    <div class="bg-white">
      <div class="chat-wrap pt-3 pb-3 pos-r">
        <div class="al-c">
          <img :src="logo" class="mr-3 avatar" />
          <div class="mr-auto shrink-1">
            <h2 class="fz-16">{{ info.title || "ChatGPT Demo" }}</h2>
            <p class="gray fz-13 mt-1 line-3" v-if="info.bio">
              {{ info.bio }}
            </p>
          </div>
          <div class="ml-2" v-if="info.btnLink">
            <a :href="info.btnLink" target="_blank" class="fz-15">
              {{ info.btnName || "Link" }}
            </a>
          </div>
          <div class="ml-2" v-if="!info.apiKey">
            <img
              src="img/key.svg"
              width="22"
              class="d-b hover-1"
              @click="showSetting = true"
            />
          </div>
        </div>
      </div>
    </div>

    <div
      class="pos-mask trans-200"
      :class="{
        'op-0 ev-n': !showSetting,
      }"
    >
      <div class="pos-mask bg-black-3" @click="showSetting = false"></div>
      <div
        class="pos-a top-0 w100p z-100 bg-white trans-200"
        :class="{
          'up-close': !showSetting,
        }"
      >
        <div class="chat-wrap">
          <div class="fz-14 mb-2 gray">
            OpenAI API Key (Stored in localStorage)
          </div>
          <div class="d-flex">
            <textarea
              v-model="newApiKey"
              @keyup.enter="onNewApiKey"
              type="text"
              rows="3"
              class="chat-input flex-1"
              placeholder="sk-******"
            />
            <button class="chat-btn ml-2" @click="onNewApiKey">OK</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    info: Object,
    logo: String,
    apiKey: String,
  },
  data() {
    return {
      showSetting: false,
      newApiKey: this.apiKey,
    };
  },
  methods: {
    onNewApiKey() {
      if (!/^sk-\w{48}$/.test(this.newApiKey)) {
        window.alert("malformed api key");
        return;
      }
      this.showSetting = false;
      this.$emit("new-key", this.newApiKey);
    },
  },
};
</script>