<template>
  <div>
    <div id="progress">
      <div v-bind:style="{ width: this.progress + '%' }"></div>
    </div>
    <sidebar></sidebar>
    <main>
      <router-view></router-view>
      <shell v-if="isExecEnabled && isLogged && user.perm.execute" />
    </main>
    <prompts></prompts>
  </div>
</template>

<script>
import { mapState, mapGetters } from "vuex";
import Prompts from "@/components/prompts/Prompts";
import Shell from "@/components/Shell";
import { enableExec } from "@/utils/constants";

export default {
  name: "layout",
  components: {
    Prompts,
    Shell,
  },
  computed: {
    ...mapGetters(["isLogged", "progress"]),
    ...mapState(["user"]),
    isExecEnabled: () => enableExec,
  },
  watch: {
    $route: function () {
      this.$store.commit("resetSelected");
      this.$store.commit("multiple", false);
      if (this.$store.state.show !== "success")
        this.$store.commit("closeHovers");
    },
  },
};
</script>
