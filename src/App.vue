<template>
  <div class="everything">
    <div class="menu-container">
      <Menu :menuItems="menuItems" @itemClick="handleItemClick" />
    </div>
    <div id="drawflow"></div>
    <div id="options-modal-container">
      <div role="button" @click="toggleModal" id="options-modal-button">
        <img :src="require('./icons/options.svg')" id="optionsIcon" />
        Options
      </div>
      <OptionsModal @close="toggleModal" :modalActive="modalActive">
        <div class="modal-content">
          &nbsp;
          <div @click="exportData">
            <img :src="require('./icons/save.svg')" class="options-icons" />
            &nbsp; Save
          </div>
          <div @click="listToggleModal" role="button" id="list-modal-button">
            <img :src="require('./icons/list.svg')" class="options-icons" />
            &nbsp; All Programs
          </div>
          <div>
            <img :src="require('./icons/trash.svg')" class="options-icons" />
            &nbsp; Delete
          </div>
        </div>
      </OptionsModal>
    </div>
    <Modal @close="listToggleModal" :listModalActive="listModalActive">
      <div class="list-modal-content">
        <h2 class="programs-header">Programs</h2>
        <div class="container">
          <div class="row">
            <div class="col-xs-12">
              <div class="table-responsive" data-pattern="priority-columns">
                <table
                  summary="This tanle shows a list of programs created with Nodes Editor"
                  class="table table-bordered"
                >
                  <thead>
                    <tr>
                      <th data-priority="1" class="list-of-programs-header">
                        Title
                      </th>
                      <th data-priority="2" class="list-of-programs-header">
                        Creation Date
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      class="list-of-programs"
                      v-for="program in listOfPrograms"
                      :key="program.id"
                    >
                      <td>{{ program.name }}</td>
                      <td>{{ program.date }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Modal>
  </div>

  <div id="modal-container"></div>
</template>

<script>
import { h, getCurrentInstance, render } from "vue";
/*eslint-disable */
import OptionsModal from "./components/OptionsModal.vue";
import Modal from "./components/Modal.vue";
import Menu from "./components/Menu.vue";
import VariableNode from "./components/VariableNode.vue";
import NumberNode from "./components/NumberNode.vue";
import AddingNode from "./components/AddingNode.vue";
import CodeBlockNode from "./components/CodeBlockNode.vue";
import SubtractNode from "./components/SubtractNode.vue";
import MultiplicationNode from "./components/MultiplicationNode.vue";
import DivisionNode from "./components/DivisionNode.vue";
import codeBlockIcon from "./icons/block.svg";
import boxIcon from "./icons/box.svg";
import numberIcon from "./icons/number.svg";
import addIcon from "./icons/add.svg";
import subtractIcon from "./icons/subtract.svg";
import multiplicationIcon from "./icons/multiplication.svg";
import divisionIcon from "./icons/division.svg";
import conditionalIcon from "./icons/conditionals.svg";
import forLoopIcon from "./icons/for_loop.svg";
import optionsIcon from "./icons/options.svg";

import Drawflow from "drawflow";

import styleDrawflow from "drawflow/dist/drawflow.min.css"; // eslint-disable-line no-use-before-define
/*eslint-enable */

export default {
  name: "App",
  components: {
    Menu,
    OptionsModal,
    Modal,
  },
  data() {
    return {
      editor: null,
      listPrograms: [],
      listOfPrograms: [],
      menuItems: [
        {
          id: "code_block",
          itemName: "Code Block",
          icon: codeBlockIcon,
        },
        {
          id: "variable",
          itemName: "Variable",
          icon: boxIcon,
        },
        {
          id: "number",
          itemName: "Number",
          icon: numberIcon,
        },
        {
          id: "add",
          itemName: "Add",
          icon: addIcon,
        },
        {
          id: "subtract",
          itemName: "Subtract",
          icon: subtractIcon,
        },
        {
          id: "multiplication",
          itemName: "Multiplication",
          icon: multiplicationIcon,
        },
        {
          id: "division",
          itemName: "Division",
          icon: divisionIcon,
        },
        {
          id: "conditional",
          itemName: "Conditional",
          icon: conditionalIcon,
        },
        {
          id: "for_loop",
          itemName: "For Loop",
          icon: forLoopIcon,
        },
      ],
      modalActive: false,
      listModalActive: false,
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
    exportData() {
      const exportedContent = this.editor.export();
      console.log(JSON.stringify(exportedContent));
      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");

      const raw = JSON.stringify({
        name: "test2",
        drawflow_output: JSON.stringify(exportedContent),
      });

      const requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
      };

      fetch("http://localhost:8000/item", requestOptions)
        .then((response) => response.text())
        .then((result) => console.log(result))
        .catch((error) => console.log("error", error));
    },
    toggleModal () {
      this.modalActive = !this.modalActive;
    },
    async listToggleModal() {
      this.listModalActive = !this.listModalActive;
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };
      try {
        const response = await fetch(
          "http://localhost:8000/programs",
          requestOptions
        );
        const result = await response.json();
        this.programsFirebaseResult = result;
        console.log(`ListPrograms:`, this.programsFirebaseResult);
        this.listOfPrograms = Object.values(this.programsFirebaseResult);
      } catch (error) {
        console.log("error", error);
      }
    }
  }
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
  align-items: stretch;
  justify-content: space-evenly;
  width: 20%;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
}

.item-title {
  font-size: 35px;
  font-family: "Sora", sans-serif;
  font-weight: 700;
  margin-left: 10px;
}

.item {
  order: 5;
  font-family: "Sora", sans-serif;
  font-size: 25px;
  font-weight: 200;
  height: 100%;
  width: 100%;
  padding: 15px;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
  cursor: pointer;
}

#drawflow {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-end;
  height: 100%;
  width: 80%;
  border: 1px solid red;
}

#options-modal-container {
  position: absolute;
  border: 1px solid blue;
  right: 0;
  top: 0;
  display: inline-flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: stretch;
  width: 10%;
}

#options-modal-button {
  cursor: pointer;
}

.modal-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.icons {
  height: 25px;
  width: 25px;
  margin-top: 15px;
  margin-right: 1px;
}

.options-icons {
  height: 25px;
  width: 25px;
  margin-top: 2px;
}

/* .options {
  font-size: 15px;
} */

#optionsIcon {
  height: 35px;
  width: 35px;
}

#list-modal-container {
  position: absolute;
  top: 0;
  right: 0;
  background-color: rgba(2, 11, 14, 0.276);
  display: flex;
  justify-content: center;
  border: 5px solid green;
  height: 100%;
  width: 100%;
}

.list-modal {
  border: 1px solid blue;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-content: space-around;
  justify-content: space-around;
  width: 50%;
}

.list-modal-content {
  border: 1px solid red;
  display: flex;
  flex-direction: column;
  height: 90%;
}

.programs-header {
  text-align: initial;
  padding: 20px 10;
}

.table {
  width: 100%;
  table-layout: auto;
  border: 1px solid #ddd;
  border-collapse: collapse;
}
/* .table-bordered {
  border: 1px solid #ddd;
} */

th,
td {
  border: 1px solid #ddd;
  text-align: center;
}
</style>
