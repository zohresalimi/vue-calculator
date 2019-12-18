
<template>
  <div id="app">
    <main class="calculator">
      <div class="display">{{ display}}</div>
      <div class="buttons">
        <div class="button-row" v-for="(row, index) in buttonRows" :key="index">
          <div
            class="button"
            :class="{ mainOperator: button.type == 'mainOperator'}"
            :style="button.style"
            @click="buttonClick(button)"
            v-for="(button, index) in row"
            :key="index"
          >
            <span v-html="button.text"></span>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data(vm) {
    return {
      display: "0",
      previousValue: null,
      currentOperator: "",
      buttonClicks:{
        number(button){
          if (vm.previousValue === null) {
            vm.previousValue = Number(vm.display);
            vm.display = "";
          }
          if (button.text == "." && vm.display.includes(".")) return;
          vm.display += button.text;
          if(vm.display[0] == "0"){
            vm.display = vm.display.slice(1);
          }
        },
        operator(button){
          if(vm.currentOperator){
            vm.performOperation();
          }
          vm.previousValue = null;
          vm.currentOperator = button.text;
        },
        special(button){
          if (button.text == "AC") {
            vm.display = "0";
          } else if (button.text == "+/-") {
            vm.display *= -1;
          } else if (button.text == "=") {
            vm.performOperation();
            vm.currentOperator = "";
          }
        }
      },
      operations: {
        "&#0247": (a, b) => a / b,
        x: (a, b) => a * b,
        "+": (a, b) => a + b,
        "-": (a, b) => a - b
      },
      buttonRows: [
        [
          { text: "AC", type: "special" },
          { text: "+/-", type: "special" },
          { text: "%", type: "operator" },
          { text: "&#0247", type: "operator" }
        ],
        [
          { text: "7", type: "number" },
          { text: "8", type: "number" },
          { text: "9", type: "number" },
          { text: "x", type: "operator" }
        ],
        [
          { text: "4", type: "number" },
          { text: "5", type: "number" },
          { text: "6", type: "number" },
          { text: "-", type: "operator" }
        ],
        [
          { text: "1", type: "number" },
          { text: "2", type: "number" },
          { text: "3", type: "number" },
          { text: "+", type: "operator" }
        ],
        [
          { text: "0", type: "number" },
          { text: ".", type: "number", style: "flex-basis: calc(100%/2.32)" },
          {
            text: "=",
            type: "special",
            style: "flex-basis: calc(100%/2.2)"
          }
        ]
      ]
    };
  },
  methods: {
    buttonClick(button) {
      const buttonFn = this.buttonClicks[button.type];
      buttonFn(button);
    },
    performOperation(){
        this.display = this.operations[this.currentOperator](+this.previousValue,+this.display)
    }
  }
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
}
.calculator {
  width: 300px;
  font-family: sans-serif;
  background-color: #f0f0f0;
  border: 1px solid #f0f0f0;
}
.display {
  height: 50px;
  text-align: right;
  padding: 15px;
  font-size: 18px;
  font-weight: 700;
}
.button-row {
  display: flex;
  justify-content: space-around;
  text-align: center;
}
.button {
  display: inline-block;
  background-color: #fff;
  width: 100%;
  padding: 0.4em;
  margin: 0.1em;
  cursor: pointer;
}
.mainOperator {
  background-color: #bfd9f2;
}

.button:active {
  background-color: rgb(208, 208, 208);
}
</style>
