<template>
  <div>
    <div class="row align-items-center justify-content-center">
      <div class="col-3 display-1">TOP <strong>#3</strong></div>
      <div class="col-9">
        <div class="row mx-3 align-items-center justify-content-center">
          <single
            v-for="(celebrity, index) in topThree"
            :key="celebrity.id"
            :celebrity="celebrity"
            bg-variant="dark"
            border-variant="light"
            text-variant="info"
            :voteAllowance="false"
            :class="index === 1 ? 'col-4' : 'col-3'"
            :title="celebrity.name"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import single from "./single.vue";
import eventBus from "../../eventBus";
export default {
  name: "top",
  components: { single },
  data() {
    return {
      topThree: []
    };
  },
  mounted() {
    eventBus.$on("topThree", topThree => {
      this.topThree = topThree;
      const index = [1, 0, 2];
      this.topThree = index.map(i => this.topThree[i]);
    });
  }
};
</script>

<style>
</style>