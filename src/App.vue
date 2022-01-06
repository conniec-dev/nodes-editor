<template>
  <div class="everything">
    <div class="menu-container">
      <Menu :menuItems="menuItems" @itemClick="handleItemClick" />
    </div>
    <div id="drawflow"></div>
  </div>
</template>

<script>
import { h, getCurrentInstance, render } from "vue";
/*eslint-disable */
import Menu from "./components/Menu.vue";
import VariableNode from "./components/VariableNode.vue";
import NumberNode from "./components/NumberNode.vue";
import AddingNode from "./components/AddingNode.vue";
import CodeBlockNode from "./components/CodeBlockNode.vue";
import SubtractNode from "./components/SubtractNode.vue";
import MultiplicationNode from "./components/MultiplicationNode.vue";
import DivisionNode from "./components/DivisionNode.vue";
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
      menuItems: [
        {
          id: "code_block",
          itemName: "Code Block",
          icon: "../icons/block.svg",
        },
        {
          id: "variable",
          itemName: "Variable",
          icon: "../icons/box.svg",
        },
        {
          id: "number",
          itemName: "Number",
          icon: "../icons/arithmetic_operations.svg",
        },
        {
          id: "add",
          itemName: "Add",
          icon: "../icons/arithmetic_operations.svg",
        },
        {
          id: "subtract",
          itemName: "Subtract",
          icon: "../icons/arithmetic_operations.svg",
        },
        {
          id: "multiplication",
          itemName: "Multiplication",
          icon: "../icons/arithmetic_operations.svg",
        },
        {
          id: "division",
          itemName: "Division",
          icon: "../icons/arithmetic_operations.svg",
        },
        {
          id: "conditional",
          itemName: "Conditional",
          icon: ".",
        },
        {
          id: "for_loop",
          itemName: "For Loop",
          icon: ".",
        },
      ],
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

    // Node registering
    const codeBlockNodeDefaultProps = {};
    const variableNodeDefaultProps = {};
    const numberNodeDefaultProps = {};
    const addingNodeDefaultProps = {};
    const subtractNodeDefaultProps = {};
    const multiplicationNodeDefaultProps = {};
    const divisionNodeDefaultProps = {};

    const options = {};
    this.editor.registerNode(
      "CodeBlockNode",
      CodeBlockNode,
      codeBlockNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "VariableNode",
      VariableNode,
      variableNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "NumberNode",
      NumberNode,
      numberNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "AddingNode",
      AddingNode,
      addingNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "SubtractNode",
      SubtractNode,
      subtractNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "MultiplicationNode",
      MultiplicationNode,
      multiplicationNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "MultiplicationNode",
      MultiplicationNode,
      multiplicationNodeDefaultProps,
      options
    );
    this.editor.registerNode(
      "DivisionNode",
      DivisionNode,
      divisionNodeDefaultProps,
      options
    );
    // Test node creation
    // this.editor.addNode("Name3", 2, 1, 1, 1, "Class", {}, "AddingNode", "vue");
  },
  methods: {
    handleItemClick({ itemId }) {
      // Create a node
      const numberOfInputs = 0;
      const numberOfInputsForOperations = 2;
      const numberOfOutputs = 1;
      const props = {};
      const coords = {
        x: 30,
        y: 30,
      };

      if (itemId === "code_block") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputs,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "CodeBlockNode",
          "vue"
        );
      } else if (itemId === "variable") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputs,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "VariableNode",
          "vue"
        );
      } else if (itemId === "number") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputs,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "NumberNode",
          "vue"
        );
      } else if (itemId === "add") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputsForOperations,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "AddingNode",
          "vue"
        );
      } else if (itemId === "subtract") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputsForOperations,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "SubtractNode",
          "vue"
        );
      } else if (itemId === "multiplication") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputsForOperations,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "MultiplicationNode",
          "vue"
        );
      } else if (itemId === "division") {
        this.editor.addNode(
          `${itemId}_unique_id_test`,
          numberOfInputsForOperations,
          numberOfOutputs,
          coords.x,
          coords.y,
          "Class",
          props,
          "DivisionNode",
          "vue"
        );
      }
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
