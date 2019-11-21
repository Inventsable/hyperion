<template>
  <q-pull-to-refresh style="height: 100%;" @refresh="refresh">
    <q-page>
      <div class="tree-wrapper q-pa-md q-gutter-sm">
        <file-tree :root="path" />
        <!-- <q-btn dense flat @click="" :icon="`mdi-close`"> </q-btn> -->
      </div>
    </q-page>
  </q-pull-to-refresh>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import spy from "cep-spy";

export default {
  components: {
    "quasar-logo": require("src/assets/quasarLogo.vue").default,
    "file-tree": require("src/components/Filetree.vue").default
  },
  data: () => ({
    path: spy.path.root
  }),
  computed: {
    ...mapGetters("settings", ["settings"]),
    app() {
      return this.$root.$children[0];
    },
    size() {
      return this.settings.size;
    }
  },
  async mounted() {
    // await this.app.runCS("init()");
  },
  methods: {
    ...mapActions("settings", ["setSize"]),
    refresh(done) {
      console.log("Refreshing action");
      setTimeout(() => {
        done();
      }, 2000);
    }
  }
};
</script>

<style>
.main-page {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-wrap: wrap;
}

.tree-wrapper {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-wrap: nowrap;
}
</style>
