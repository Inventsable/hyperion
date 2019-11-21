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
      <q-bar>
        <q-btn dense flat @click="" :icon="`mdi-home`">
          <tooltipper msg="Test" />
        </q-btn>
        <q-space></q-space>
        <q-btn-dropdown
          auto-close
          dense
          flat
          color="primary"
          :icon="'mdi-format-vertical-align-top'"
          dropdown-icon
        >
          <q-btn
            dense
            flat
            @click=""
            icon="mdi-format-vertical-align-top"
            :style="isActiveStyle(true)"
          />
          <q-btn
            dense
            flat
            @click=""
            icon="mdi-format-vertical-align-center"
            :style="isActiveStyle(false)"
          />
          <q-btn
            dense
            flat
            @click=""
            icon="mdi-format-vertical-align-bottom"
            :style="isActiveStyle(false)"
          />
        </q-btn-dropdown>
        <q-btn dense flat @click="" :icon="`mdi-eye`">
          <tooltipper msg="Test" />
        </q-btn>
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

export default {
  name: "MyLayout",
  components: {
    drawer: require("components/panel/Drawer.vue").default,
    toolbar: require("components/panel/Toolbar.vue").default,
    tooltipper: require("components/panel/tooltipper.vue").default
  },
  data: () => ({}),
  mounted() {
    console.log("Panel");
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
    }
  }
};
</script>

<style>
.q-layout__section--marginal {
  user-select: none;
  cursor: default;
  background-color: var(--color-header-border);
}

@media screen and (max-width: 260px) {
  .toolbar-title {
    display: none;
  }
}
</style>
