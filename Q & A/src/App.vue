<template>
  <div>
    <Header :correctCount="correctCount" :totalCount="totalCount" />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Content
            :cQuestion="questionlist[index]"
            v-if="questionlist.length"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Content from "./components/Content.vue";

export default {
  name: "App",
  components: {
    Header,
    Content,
  },

  data() {
    return {
      questionlist: [],
      index: 0,
      correctCount: 0,
      totalCount: 0,
    };
  },

  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
      }
    },
    increment(is_correct) {
      if (is_correct) {
        this.correctCount++;
      }
      this.totalCount++;
    },
  },

  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=28&type=multiple", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((result) => {
        this.questionlist = result.results;
      });
  },
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
