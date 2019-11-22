<template>
  <q-pull-to-refresh style="height: 100%;" @refresh="refresh">
    <q-page>
      <div v-if="paths.length" class="tree-wrapper q-pa-md q-gutter-sm">
        <file-tree v-for="(path, i) in paths" :key="i" :root="path" />
        <!-- <q-btn dense flat @click="" :icon="`mdi-close`"> </q-btn> -->
      </div>
      <div v-else class="fixed-center text-center">
        <p>
          <sadface />
        </p>
        <p class="text-faded">Sorry, nothing here...<strong>(404)</strong></p>
        <q-btn color="secondary" style="width:200px;" to="/" label="Go back" />
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
    "file-tree": require("src/components/Filetree.vue").default,
    sadface: require("src/assets/sadLogo.vue").default
  },
  data: () => ({
    // paths: [spy.path.root]
  }),
  computed: {
    ...mapGetters("settings", ["settings"]),
    app() {
      return this.$root.$children[0];
    },
    size() {
      return this.settings.size;
    },
    paths() {
      return this.app.paths;
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
