<template>
  <div>
    <top />
    <hr class="my-4" />
    <div class="row">
      <div class="col-10">
        <celebrities />
      </div>
      <div class="col-2">
        <history />
      </div>
    </div>
  </div>
</template>

<script>
import celebrities from "./celebrities.vue";
import history from "./vote-history.vue";
import top from "./top-three.vue";
export default {
  name: "vote",
  components: { celebrities, history, top },
  data() {
    return {};
  }
};
</script>

<style>
</style>