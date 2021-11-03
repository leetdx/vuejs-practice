<template>
  <div>
    <h4>Votes history</h4>
    <ul class="list-group-flush my-5 overflow-auto">
      <li
        v-for="celebrity in getTenRecords"
        :key="celebrity.index"
        class="list-group-item"
      >
        <div>
          <b-avatar
            variant="info"
            :src="require(`../../assets/images/${celebrity.image}`)"
          ></b-avatar>
        </div>
        <strong>
          {{ celebrity.name }}
        </strong>
        <br />
        <small>
          {{ celebrity.time }}
        </small>
      </li>
    </ul>
  </div>
</template>

<script>
import eventBus from "../../eventBus";
export default {
  name: "history",
  data() {
    return {
      votesHistory: [],
      celebrity: {}
    };
  },
  mounted() {
    eventBus.$on("voted", celebrity => {
      celebrity.time = this.getTime();
      this.celebrity = celebrity;
    });
  },
  computed: {
    getTenRecords() {
      return this.votesHistory.slice(0, 10);
    }
  },
  watch: {
    celebrity() {
      this.votesHistory.unshift(this.celebrity);
    }
  },
  methods: {
    getTime() {
      const d = new Date();
      return d.toLocaleTimeString();
    }
  }
};
</script>

<style>
</style>