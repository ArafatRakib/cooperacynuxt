<template>
  <div>
    <nuxt-link
      to="/useredit"
      class="ad"
    >Edit your information</nuxt-link><br>
    <nuxt-link
      to="/project/form"
      class="ac"
    >Add a new Project</nuxt-link><br>
    <nuxt-link
      to="/project/search"
      class="af"
    >Project search</nuxt-link><br>
    <h5 class="mt-2 d-flex justify-content-center">YOUR PROJECTS</h5>
    <projectlist />
  </div>
</template>

<script>
import projectlist from "@/components/projectlist";
export default {
  middleware: ["auth"],
  components: { projectlist: projectlist },
  mounted() {
    var cc = ["D", "U", "F", "I", "C", "T", "E"];
    var sum = 0;
    var sum2 = 0;
    var sum3 = 0;
    var max = 0;
    var res = {};
    var res2 = {};
    var res3 = {};
    for (let j = 0; j < cc.length; j++) {
      res[cc[j]] = this.$auth.user[cc[j]];
    }
    for (let j = 0; j < cc.length; j++) {
      sum += res[cc[j]];
    }
    if (sum == 0) {
      for (let j = 0; j < cc.length; j++) {
        res[cc[j]] = 4;
      }
    } else {
      for (let j = 0; j < cc.length; j++) {
        res2[cc[j]] = (res[cc[j]] * 21) / sum;
      }
      for (let j = 0; j < cc.length; j++) {
        if (max < res2[cc[j]]) {
          max = res2[cc[j]];
        }
      }
      for (let j = 0; j < cc.length; j++) {
        res2[cc[j]] = res2[cc[j]] * (1 - Math.abs((max - 6) / max));
      }
      for (let j = 0; j < cc.length; j++) {
        sum2 += res2[cc[j]];
      }
      for (let j = 0; j < cc.length; j++) {
        res3[cc[j]] = Math.abs(6 - res2[cc[j]]) / 7;
      }
      for (let j = 0; j < cc.length; j++) {
        sum3 += res3[cc[j]];
      }
      for (let j = 0; j < cc.length; j++) {
        res[cc[j]] = Math.round(
          (res3[cc[j]] / sum3) * (21 - sum2) + res2[cc[j]] + 1
        );
      }
    }
    this.$store.commit("setUserBar", res);
  },
  async fetch({ store, params }) {
    await store.dispatch("getUserProjectAction", {
      userid: store.state.auth.user.id
    });
  }
};
</script>
