<template>
  <div class="singleCard" v-b-tooltip.hover :title="title">
    <b-card
      overlay
      img-top
      :bg-variant="bgVariant"
      :border-variant="borderVariant"
      :text-variant="textVariant"
      :img-src="require(`../../assets/images/${celebrity.image}`)"
      :img-alt="celebrity.image"
      @click="handleClick()"
    >
      <template #header>
        <div class="display-3">{{ celebrity.vote }}</div>
      </template>
      <template #footer>
        <div>{{ celebrity.name }}</div>
      </template>
    </b-card>
  </div>
</template>

<script>
import eventBus from "../../eventBus";
export default {
  name: "single",
  props: [
    "celebrity",
    "bgVariant",
    "borderVariant",
    "textVariant",
    "voteAllowance",
    "title"
  ],
  data() {
    return {};
  },
  methods: {
    handleClick() {
      if (this.voteAllowance) {
        this.celebrity.vote++;
        eventBus.$emit("voted", this.celebrity);
      }
    }
  }
};
</script>

<style scoped>
.singleCard {
  cursor: pointer;
}
</style>