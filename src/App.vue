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
          <div @click="openSaveModal" class="options-buttons">
            <img :src="require('./icons/save.svg')" class="options-icons" />
            &nbsp; Save
          </div>
          <div
            @click="listToggleModal"
            role="button"
            id="list-modal-button"
            class="options-buttons"
          >
            <img :src="require('./icons/list.svg')" class="options-icons" />
            &nbsp; All Programs
          </div>
          <div @click="editor.clearModuleSelected()" class="options-buttons">
            <img :src="require('./icons/trash.svg')" class="options-icons" />
            &nbsp; Clear
          </div>
        </div>
      </OptionsModal>
    </div>
    <SavingModal @close="exportData" :saveModalActive="saveModalActive">
      <div class="save-modal-container">
        <p>Save as:</p>
        <input
          v-model="programTitle"
          type="text"
          placeholder="Write a title for your program"
          class="program-title-input"
        />
        <div class="save-modal-buttons">
          <div
            role="button"
            class="save-modal-button"
            @click="closeSaveModal"
            :saveModalActive="saveModalActive"
          >
            Cancel
          </div>
          <div role="button" class="save-modal-button" @click="exportData">
            Save
          </div>
        </div>
      </div>
    </SavingModal>
    <Modal @close="listToggleModal" :listModalActive="listModalActive">
      <div class="list-modal-content">
        <h2 class="programs-header">Programs</h2>
        <div class="container">
          <div class="row">
            <div class="col-xs-12">
              <div class="table-responsive" data-pattern="priority-columns">
                <table
                  summary="This table shows a list of programs created with Nodes Editor"
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
                      @click="importData(program.name)"
                    >
                      <td>
                        {{ program.name }}
                      </td>
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
import SavingModal from "./components/SavingModal.vue";
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
    SavingModal,
    OptionsModal,
    Modal,
  },
  precanvas: null,
  data() {
    return {
      editor: null,
      programTitle: "",
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
      saveModalActive: false,
      modalActive: false,
      listModalActive: false,
      closeOnEscape: true,
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
    openSaveModal() {
      this.saveModalActive = !this.saveModalActive;
    },
    exportData() {
      this.saveModalActive = !this.saveModalActive;
      const exportedContent = this.editor.export();
      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");

      const raw = JSON.stringify({
        name: this.programTitle,
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

    closeSaveModal() {
      this.saveModalActive = !this.saveModalActive;
    },

    toggleModal() {
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
        this.listOfPrograms = Object.values(this.programsFirebaseResult);
        for (let i = 0; i < this.listOfPrograms.length; i++) {
          const date = new Date(this.listOfPrograms[i].date);
          const formatted_date = date.toLocaleDateString();
          this.listOfPrograms[i].date = formatted_date;
        }
      } catch (error) {
        console.log("error", error);
      }
    },

    async importData(programName) {
      const requestOptions = {
        method: "GET",
        redirect: "follow",
      };
      let programIdToGet = "";

      try {
        for (var element in this.listOfPrograms) {
          if (programName === this.listOfPrograms[element].name) {
            programIdToGet = this.listOfPrograms[element].id;
          }
        }
        const response = await fetch(
          `http://localhost:8000/programs/${programIdToGet}`,
          requestOptions
        );
        const result = await response.json();
        this.editor.import(JSON.parse(result.drawflow_output));
        this.listModalActive = !this.listModalActive;
      } catch (error) {
        console.log("error", error);
      }
    },
    clearModuleSelected() {
      this.precanvas.innerHTML = "";
      this.drawflow.drawflow[this.module] = { data: {} };
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
  background: var(--background-color);
  background-size: 25px 25px;
  background-image: linear-gradient(to right, #f1f1f1 1px, transparent 1px),
    linear-gradient(to bottom, #f1f1f1 1px, transparent 1px);
}

.drawflow > .drawflow-delete {
  border: 2px solid #43b993;
  background: white;
  color: #43b993;
  -webkit-box-shadow: 0 2px 20px 2px #43b993;
  box-shadow: 0 2px 20px 2px #43b993;
}

.drawflow-delete {
  border: 2px solid rgb(0, 0, 0);
  background: white;
  color: rgb(0, 0, 0);
  -webkit-box-shadow: 0 2px 20px 2px #ddd;
  box-shadow: 0 2px 20px 2px #ddd;
}

.drawflow .connection .main-path {
  stroke: rgb(65, 64, 64);
  stroke-width: 3px;
}

.drawflow .connection .point {
  stroke: var(--border-color);
  stroke-width: 2;
  fill: white;
  transform: translate(-9999px, -9999px);
} 

.drawflow .connection .point.selected, .drawflow .connection .point:hover {
  fill: #ddd;
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
  font-family: "Sora", sans-serif;
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

.options-buttons {
  cursor: pointer;
  font-weight: 300;
}

/* .options {
  font-size: 15px;
} */

#optionsIcon {
  height: 35px;
  width: 35px;
}

#save-modal-container {
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

.save-modal {
  border: 1px solid blue;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-content: space-around;
  justify-content: space-around;
  width: 50%;
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
  font-family: "Sora", sans-serif;
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
  font-weight: 500;
}

.table {
  width: 100%;
  table-layout: auto;
  border: 1px solid #ddd;
  border-collapse: collapse;
}

.save-modal-container {
  height: 80%;
  font-family: "Sora", sans-serif;
  font-weight: 100;
  font-size: 15px;
}

.save-modal-buttons {
  height: 50%;
  display: flex;
  justify-content: flex-end;
}

.save-modal-button {
  border: 1px solid #ddd;
  align-self: end;
  padding: 5px;
  margin-right: 10px;
}

.program-title-input {
  height: 25px;
  width: 100%;
  font-family: "Sora", sans-serif;
  border: 1px solid #ddd;
}

/* .table-bordered {
  border: 1px solid #ddd;
} */

th {
  border: 1px solid #ddd;
  text-align: center;
  font-weight: 500;
}

td {
  border: 1px solid #ddd;
  text-align: center;
  font-weight: 300;
}
</style>
