<template>
  <button
    :url="url"
    :description="description"
    :isBlank="isBlank"
    class="share-button share-button--hatena"
    type="button"
    @click="openShareWindow"
  >
    <img v-if="customIcon" v-bind:src="customIcon" alt="" />
    <icon :iconName="ariaLabel" class="share-button__icon" v-if="hasIcon === true">
      <path
        d="M20.47 0C22.42 0 24 1.58 24 3.53v16.94c0 1.95-1.58 3.53-3.53 3.53H3.53C1.58 24 0 22.42 0 20.47V3.53C0 1.58 1.58 0 3.53 0h16.94zm-3.705 14.47c-.78 0-1.41.63-1.41 1.41s.63 1.414 1.41 1.414 1.41-.645 1.41-1.425-.63-1.41-1.41-1.41zM8.61 17.247c1.2 0 2.056-.042 2.58-.12.526-.084.976-.222 1.32-.412.45-.232.78-.564 1.02-.99s.36-.915.36-1.48c0-.78-.21-1.403-.63-1.87-.42-.48-.99-.734-1.74-.794.66-.18 1.156-.45 1.456-.81.315-.344.465-.824.465-1.424 0-.48-.103-.885-.3-1.26-.21-.36-.493-.645-.883-.87-.345-.195-.735-.315-1.215-.405-.464-.074-1.29-.12-2.474-.12H5.654v10.486H8.61zm.736-4.185c.705 0 1.185.088 1.44.262.27.18.39.495.39.93 0 .405-.135.69-.42.855-.27.18-.765.254-1.44.254H8.31v-2.297h1.05zm8.656.706v-7.06h-2.46v7.06H18zM8.925 9.08c.71 0 1.185.08 1.432.24.245.16.367.435.367.83 0 .38-.13.646-.39.804-.265.154-.747.232-1.452.232h-.57V9.08h.615z"
      />
    </icon>
    <span v-if="btnText" class="share-button__text">{{ btnText }}</span>
  </button>
</template>

<script>
import Icon from "./icon/Icon.vue";
import {
  createWindow,
  eventEmit,
  getDocumentHref,
  getDocumentTitle,
} from "../helpers";

export default {
  name: "HatenaShareButton",
  components: { Icon },
  props: {
    url: { type: String, default: getDocumentHref },
    description: { type: String, default: getDocumentTitle },
    btnText: { type: String, default: "Hatena" },
    modalWidth: { type: Number, default: 500 },
    modalHeight: { type: Number, default: 500 },
    hasIcon: { type: Boolean, default: true },
    isBlank: { type: Boolean, default: true },
    customIcon: { type: String, default: "" },
    ariaLabel: { type: String, default: "Hatena" },
  },
  methods: {
    openShareWindow() {
      eventEmit(this, "onShare", { name: "Hatena" });
      const configWindow = createWindow(
        this.$props.modalWidth,
        this.$props.modalHeight
      );
      const url = `http://b.hatena.ne.jp/bookmarklet?url=${encodeURIComponent(
        this.$props.url
      )}&btitle=${encodeURIComponent(this.$props.description)}`;

      return this.$props.isBlank
        ? window.open(url, "_blank")
        : window.open(url, "Share this", configWindow);
    },
  },
};
</script>

<style lang="scss" scoped>
$main-color: hsla(196, 100%, 44%, 1);
$focus-color: hsla(196, 94%, 69%, 0.4);
$hover-color: hsla(196, 100%, 44%, 0.9);
$painted-color: hsla(195, 77%, 34%, 1);

@import "../css/styles";
</style>
