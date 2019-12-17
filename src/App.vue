
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
  data() {
    return {
      display: "0",
      previousValue: null,
      currentOperator: "",
      operations: {
        "&#0247": (a, b) => a / b,
        x: (a, b) => a * b,
        "+": (a, b) => a + b,
        "-": (a, b) => a - b
      },
      buttonRows: [
        [
          { text: "AC", type: "operator" },
          { text: "+/-", type: "operator" },
          { text: "%", type: "operator" },
          { text: "&#0247", type: "mainOperator" }
        ],
        [
          { text: "7", type: "number" },
          { text: "8", type: "number" },
          { text: "9", type: "number" },
          { text: "x", type: "mainOperator" }
        ],
        [
          { text: "4", type: "number" },
          { text: "5", type: "number" },
          { text: "6", type: "number" },
          { text: "-", type: "mainOperator" }
        ],
        [
          { text: "1", type: "number" },
          { text: "2", type: "number" },
          { text: "3", type: "number" },
          { text: "+", type: "mainOperator" }
        ],
        [
          { text: "0", type: "number" },
          { text: ".", type: "operator", style: "flex-basis: calc(100%/2.32)" },
          {
            text: "=",
            type: "mainOperator",
            style: "flex-basis: calc(100%/2.2)"
          }
        ]
      ]
    };
  },
  methods: {
    buttonClick(button) {
      if (button.type == "number") {
        if (this.display == "0") {
          this.display = "";
        }
        if (this.previousValue === null) {
          this.previousValue = Number(this.display);
          this.display = "";
        }
        if (button.text == "." && this.display.includes(".")) {
        }
        this.display += button.text;
      } else if (button.text == "AC") {
        this.display = "0";
      } else if (button.text == "+/-") {
        this.display *= -1;
      } else if (button.text == "=") {
        this.display = this.operations[this.currentOperator](
          +this.previousValue,
          +this.display
        );
      } else if (button.type == "mainOperator") {
        this.previousValue = null;
        this.currentOperator = button.text;
      }
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
}
.mainOperator {
  background-color: #bfd9f2;
}

.button:active {
  background-color: rgb(208, 208, 208);
}
</style>
