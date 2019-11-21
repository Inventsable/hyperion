<template>
  <div class="" style="width: 100%">
    <q-tree :nodes="real" node-key="path" :selected.sync="selected" />
  </div>
</template>

<script>
let fs = require("fs");
let path = require("path");
import spy from "cep-spy";

export default {
  data: () => ({
    selected: ``,
    ignore: /^\.|node_modules/,
    // simple: [
    //   {
    //     label: "root",
    //     icon: "mdi-folder",
    //     path: `./`,
    //     children: [
    //       {
    //         label: "Folder 1",
    //         icon: "mdi-folder",
    //         path: `./Folder-1`,
    //         children: [
    //           {
    //             label: "file.txt",
    //             icon: "mdi-file",
    //             path: `./Folder-1/file.txt`
    //           },
    //           {
    //             label: "file2.txt",
    //             icon: "mdi-file",
    //             path: `./Folder-1/file.ai`
    //           }
    //         ]
    //       },
    //       {
    //         label: "Folder 2",
    //         icon: "mdi-folder",
    //         path: `./Folder-2`,
    //         children: [
    //           { label: "File", icon: "mdi-file", path: `./Folder-2/file.json` },
    //           { label: "File", icon: "mdi-file", path: `./Folder-2/file.md` }
    //         ]
    //       }
    //     ]
    //   }
    // ],
    real: []
  }),
  watch: {
    selected(val) {
      this.app.selected = val;
      console.log(val);
    }
  },
  computed: {
    app() {
      return this.$root.$children[0];
    }
  },
  async created() {
    this.real = [await this.collectPath(spy.path.root)];
  },
  methods: {
    async collectPath(root) {
      let realpath = path.resolve(root);
      let folders = await this.readDirForDirs(realpath);
      let files = await this.readDirForFiles(realpath);
      let name = root.split(/\/|\\/);
      name = name[name.length - 1];
      let children = [];
      let tree = {
        label: name,
        icon: "mdi-folder",
        path: realpath
      };
      if (files.length || folders.length) {
        if (folders.length) {
          for (const folder of folders)
            children.push(await this.collectPath(`${root}/${folder}`));
        }
        if (files.length) {
          children = children.length
            ? [].concat(children, this.constructFile(root, files))
            : this.constructFile(root, files);
        }
        tree["children"] = children;
      }
      return tree;
    },
    async readDirForDirs(thispath) {
      let parent = await this.readDir(thispath);
      let children = parent.filter(child => {
        return fs
          .lstatSync(`${thispath.replace(/\/&/, "")}/${child}`)
          .isDirectory();
      });
      return children.filter(child => {
        return !this.ignore.test(child);
      });
    },
    async readDirForFiles(thispath, files = false) {
      let parent = await this.readDir(thispath);
      let children = parent.filter(child => {
        return !fs
          .lstatSync(`${thispath.replace(/\/&/, "")}/${child}`)
          .isDirectory();
      });
      return children.filter(child => {
        return !this.ignore.test(child);
      });
    },
    constructFile(root, children) {
      return children.map(child => {
        return {
          icon: "mdi-file",
          label: child,
          path: path.resolve(`${root}/${child}`)
        };
      });
    },
    async readDir(thispath) {
      return new Promise((resolve, reject) => {
        fs.readdir(
          path.resolve(thispath),
          { encoding: "utf-8" },
          (err, files) => {
            if (err) reject(err);
            resolve(
              files.filter(file => {
                return !this.ignore.test(file);
              })
            );
          }
        );
      });
    }
  }
};
</script>

<style>
.q-tree__node-header-content {
  color: var(--color-default);
}

.q-tree__node--selected .q-tree__node-header-content {
  color: var(--color-selection);
}

.q-tree__children {
  padding-left: 20px;
}

.q-tree__node:after {
  border-left: 0px solid currentColor;
}

.q-tree__node-header:before {
  border-left: 0px solid currentColor;
  border-bottom: 0px solid currentColor;
}
</style>
