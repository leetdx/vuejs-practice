<template>
  <div id="app">
    <!-- <Form /> -->
    <!-- <Todo /> -->
    <!-- <celebrity-vote /> -->
    <button class="btn btn-outline-primary" @click="openClick(1)">Form</button>
    <button class="btn btn-outline-primary mx-2" @click="openClick(2)">
      Todo
    </button>
    <button class="btn btn-outline-primary" @click="openClick(3)">Vote</button>
    <d-modal v-show="show" :size="size" :theme="theme" @closeEvent="closeEvent">
      <template slot="d-modal-header">{{ comp }}</template>
      <template slot="title"></template>
      <template slot="content">
        <component :is="comp" />
      </template>
      <template slot="d-modal-footer"></template>
    </d-modal>
  </div>
</template>

<script>
import Vote from "./components/vote/vote-place.vue";
import dModal from "./components/d-modal.vue";
import firstForm from "./components/fundamentals/form.vue";
import todoApp from "./components/fundamentals/todo.vue";
// import topThree from "./components/vote/top-three.vue";
export default {
  name: "App",
  components: {
    "d-modal": dModal,
    "first-form": firstForm,
    "to-do": todoApp,
    "celebrity-vote": Vote
  },
  data() {
    return {
      show: false,
      comp: "",
      size: "d-modal-lg",
      theme: ""
    };
  },
  methods: {
    openClick(val) {
      if (val === 1) {
        this.comp = "first-form";
        this.size = "d-modal-lg";
        this.theme = "d-white";
      } else if (val === 2) {
        this.comp = "to-do";
        this.size = "d-modal-sm";
        this.theme = "d-white";
      } else if (val === 3) {
        this.comp = "celebrity-vote";
        this.size = "d-modal-lg";
        this.theme = "d-dark";
      } else {
        this.comp = null;
        alert("No components");
      }
      if (this.comp) {
        this.show = true;
      }
    },
    closeEvent(val) {
      if (val) {
        this.show = false;
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
