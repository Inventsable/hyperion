<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-bar>
        <q-icon :name="getActiveIcon" />
        <div class="toolbar-title">{{ selected }}</div>
        <q-space />
      </q-bar>
    </q-header>

    <q-footer>
      <q-bar class="q-pl-sm">
        <q-btn dense flat @click="newFolder" :icon="`mdi-folder-plus`" />
        <q-btn dense flat @click="newFile" :icon="`mdi-content-save`" />
        <!-- <tooltipper msg="Add new" /> -->
        <q-space></q-space>
      </q-bar>
    </q-footer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import getExtVersion from "src/utils/main/getExtVersion";

// Importing standalone functions here:
import { openDialog, saveDialog } from "cluecumber";

export default {
  name: "MyLayout",
  components: {
    drawer: require("components/panel/Drawer.vue").default,
    toolbar: require("components/panel/Toolbar.vue").default,
    tooltipper: require("components/panel/tooltipper.vue").default
  },
  data: () => ({}),
  mounted() {
    console.log("Hello?");
    console.log(this.$route);
  },
  computed: {
    ...mapGetters("settings", ["settings"]),
    app() {
      return this.$root.$children[0];
    },
    getActiveIcon() {
      return this.app.selected
        ? /\.[a-zA-Z]*$/.test(this.app.selected)
          ? "mdi-file"
          : "mdi-folder"
        : "home";
    },
    selected() {
      return this.app.selected;
    },
    extVersion() {
      return getExtVersion();
    },
    size: {
      get() {
        return this.settings.size;
      },
      set(value) {
        this.setSize(value);
      }
    }
  },
  methods: {
    ...mapActions("settings", ["setSize"]),

    isActiveStyle(state) {
      return `
        color: var(--color-${state ? "selection" : "default"});
      `;
    },
    newFolder() {
      let path = openDialog(`Select folder to watch`);
      console.log(path);
    },
    newFile() {
      let path = saveDialog(`Save new file`, ["ai"]);
      console.log(path);
    }
  }
};
</script>

<style>
.q-layout__section--marginal {
  user-select: none;
  cursor: default;
  background-color: var(--color-header);
}

@media screen and (max-width: 260px) {
  .toolbar-title {
    display: none;
  }
}
</style>
