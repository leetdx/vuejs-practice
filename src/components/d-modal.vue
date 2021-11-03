<template>
  <div>
    <div :class="classes">
      <div
        class="
          d-modal-header
          row
          px-2
          justify-content-between
          align-items-center
        "
      >
        <h2 class="d-header-text col-8">
          <slot name="d-modal-header">Custom modal</slot>
        </h2>
        <button
          type="button"
          class="btn-close d-close-button col-1 p-2"
          aria-label="Close"
          @click="closeClick"
        ></button>
      </div>
      <div class="d-modal-body">
        <hr />
        <h3>
          <slot name="title">Body title</slot>
        </h3>
        <slot name="content">Some content</slot>
        <hr />
      </div>
      <div class="d-modal-footer">
        <h3>
          <slot name="d-modal-footer">Footer</slot>
        </h3>
      </div>
    </div>
    <div class="d-modal-filter" @click="outsideClick"></div>
  </div>
</template>

<script>
export default {
  name: "custom-modal",
  props: ["size", "theme"],
  data() {
    return {
      show: false
    };
  },
  computed: {
    // showModal() {
    //   return this.show;
    // },
    classes() {
      return ["d-modal", this.size, this.theme];
    }
  },
  watch: {
    show() {
      console.log("watch");
    }
  },
  methods: {
    // closeClick() {
    //   this.$emit("closeEvent", true);
    // },
    // outsideClick() {
    //   this.$emit("closeEvent", true);
    // }
    closeClick() {
      this.$emit("closeEvent", true);
    },
    outsideClick() {
      this.$emit("closeEvent", true);
    }
  }
};
</script>

<style scoped>
.d-modal {
  position: fixed;
  z-index: 1055;
  height: auto;
  width: 900px;
  left: 450px;
  top: 100px;
  padding: 15px;
  border-radius: 5px;
  overflow: scroll;
  background-color: rgb(255, 255, 255);
}

.d-modal-filter {
  position: fixed;
  z-index: 1054;
  height: 100%;
  width: 100%;
  left: 0;
  top: 0;
  /* overflow: hidden; */
  background: rgba(22, 29, 32, 0.3);
}

.d-white {
  background-color: rgb(255, 255, 255);
  color: rgb(22, 29, 32);
  opacity: 0.9.5;
}

.d-dark {
  color: rgb(235, 235, 235);
  background-color: rgb(22, 29, 32);
  opacity: 0.9.5;
}

.d-modal-sm {
  max-height: 600px;
  width: 600px;
  left: 560px;
  top: 100px;
}

.d-modal-md {
  max-height: 600px;
  width: 900px;
  left: 450px;
  top: 100px;
}

.d-modal-lg {
  max-height: 800px;
  width: 1500px;
  left: 150px;
  top: 100px;
}

.d-modal-header {
  /* background: rgba(71, 150, 84, 0.3); */
}

.d-modal-body {
  /* background: rgba(138, 114, 160, 0.3); */
}
.d-modal-footer {
  /* background: rgba(62, 119, 143, 0.3); */
}
</style>