<template>
  <div class="everything">
    <div class="menu-container">
      <Menu />
    </div>
    <div id="drawflow"></div>
  </div>
</template>

<script>
import { h, getCurrentInstance, render } from "vue";
/*eslint-disable */
import Menu from "./components/Menu.vue";
import NodeClick from "./components/NodeClick.vue";
import AddingNode from "./components/AddingNode.vue";
import Drawflow from "drawflow";
import styleDrawflow from "drawflow/dist/drawflow.min.css"; // eslint-disable-line no-use-before-define
/*eslint-enable */

export default {
  name: "App",
  components: {
    Menu,
  },
  data() {
    return {
      editor: null,
    };
  },

  mounted() {
    const id = document.getElementById("drawflow");

    const Vue = { version: 3, h, render };

    this.editor = new Drawflow(id, Vue);
    const internalInstance = getCurrentInstance();
    this.editor.value = new Drawflow(
      id,
      Vue,
      internalInstance.appContext.app._context
    );

    this.editor.start();
    const props1 = { name: "Number" };
    const options = {};
    this.editor.registerNode("NodeClick", NodeClick, props1, options);
    const data = { name: "" };
    this.editor.addNode("Name", 0, 1, 1, 1, "Class", data, "NodeClick", "vue");
    this.editor.addNode("Name2", 0, 1, 1, 1, "Class", data, "NodeClick", "vue");

    const props2 = { name: "Add" };
    this.editor.registerNode("AddingNode", AddingNode, props2, options);

    this.editor.addNode(
      "Name3",
      2,
      1,
      1,
      1,
      "Class",
      data,
      "AddingNode",
      "vue"
    );
  },

  methods: {
    exportData() {
      alert(JSON.stringify(this.editor.export()));
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

.everything {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100%;
  border: 1px solid red;
}
.menu-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  align-content: flex-start;
  justify-content: space-evenly;
  border: 1px solid blue;
  width: 20%;
}

.item-title {
  font-size: 25px;
  font-weight: bold;
}

.item {
  order: 5;
  font-size: 15px;
  border: 1px solid orange;
  height: 100%;
  width: 100%;
  padding: 10px;
}

#drawflow {
  display: flex;
  height: 100%;
  width: 80%;
  border: 1px solid olivedrab;
}

.icon {
  margin-top: 2px;
  height: 20px;
  width: 20px;
}
</style>
