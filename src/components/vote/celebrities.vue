<template>
  <div>
    <h4>Celebrities</h4>
    <div class="row mx-3">
      <single
        v-for="celebrity in celebrities"
        :key="celebrity.id"
        :celebrity="celebrity"
        bg-variant="dark"
        border-variant="light"
        text-variant="warning"
        class="col-4 my-5"
        :voteAllowance="true"
        :title="'Vote for ' + celebrity.name"
      />
    </div>
  </div>
</template>

<script>
import single from "./single.vue";
import eventBus from "../../eventBus";
export default {
  components: { single },
  data() {
    return {
      celebrities: [
        {
          id: 1,
          name: "Joaquin Phoenix",
          image: "joaquin-phoenix.jpeg",
          vote: 182,
          time: ""
        },
        {
          id: 2,
          name: "Jennifer Lawrence",
          image: "jennifer-lawrence.jpeg",
          vote: 151,
          time: ""
        },
        {
          id: 3,
          name: "Keanu Reeves",
          image: "keanu-reeves.jpeg",
          vote: 145,
          time: ""
        },
        {
          id: 4,
          name: "Gal Gadot",
          image: "gal-gadot.jpeg",
          vote: 111,
          time: ""
        },
        {
          id: 5,
          name: "Tom Hiddleston",
          image: "tom-hiddleston.jpeg",
          vote: 150,
          time: ""
        },
        {
          id: 6,
          name: "Rowan Atkinson",
          image: "rowan-atkinson.jpeg",
          vote: 221,
          time: ""
        }
      ]
    };
  },
  mounted() {
    eventBus.$emit("topThree", this.topThree);
    eventBus.$on("voted", bool => {
      if (bool.id != null) {
        eventBus.$emit("topThree", this.topThree);
      }
    });
  },
  computed: {
    topThree() {
      const array = this.celebrities;
      array.sort((a, b) => {
        return b.vote - a.vote;
      });
      const topThree = [];
      for (let i = 0; i < 3; i++) {
        topThree.push(array[i]);
      }
      return topThree;
    }
  }
};
</script>

<style scoped>
</style>