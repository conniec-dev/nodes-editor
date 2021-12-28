<template>
  <div class="everything">
    <div id="drawflow"></div>
  </div>
</template>

<script>
import { h, getCurrentInstance, render } from "vue";
/*eslint-disable */
import NodeClick from "./components/NodeClick.vue";
import AddingNode from "./components/AddingNode.vue";
import Drawflow from "drawflow";
import styleDrawflow from "drawflow/dist/drawflow.min.css"; // eslint-disable-line no-use-before-define
/*eslint-enable */

export default {
  name: "App",
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
    this.editor.addNode(
      "Name",
      0,
      1,
      150,
      150,
      "Class",
      data,
      "NodeClick",
      "vue"
    );
    this.editor.addNode(
      "Name2",
      0,
      1,
      500,
      150,
      "Class",
      data,
      "NodeClick",
      "vue"
    );

    const props2 = { name: "Add" };
    this.editor.registerNode("AddingNode", AddingNode, props2, options);

    this.editor.addNode(
      "Name3",
      2,
      1,
      850,
      150,
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
.everything {
  height: 100%;
  width: 100%;
  border: 5px solid red;
}
.menu_container {
  display: flex;
  flex-direction: column;
}

#drawflow {
  width: 100%;
  height: 500px;
}
</style>
