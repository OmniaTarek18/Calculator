<template>
  <div class="calculator">
    <div class="display history">{{ operation }}</div>
    <div class="display">{{ displayNum }}</div>
    <div class="buttons">
      <button value="%">%</button>
      <button>CE</button>
      <button>C</button>
      <button>&#x232B;</button>
      <button><sup>1</sup>/<sub>X</sub></button>
      <button><sub>X</sub><sup>2</sup></button>
      <button>&radic;x</button>
      <button @click="setOper('/')">&divide;</button>
      <button @click="displayNum = '7'">7</button>
      <button @click="displayNum = '8'">8</button>
      <button @click="displayNum = '9'">9</button>
      <button @click="setOper('*')">&times;</button>
      <button @click="displayNum = '4'">4</button>
      <button @click="displayNum = '5'">5</button>
      <button @click="displayNum = '6'">6</button>
      <button @click="setOper('-')">-</button>
      <button @click="displayNum = '1'">1</button>
      <button @click="displayNum = '2'">2</button>
      <button @click="displayNum = '3'">3</button>
      <button @click="setOper('+')">+</button>
      <button><sup>+</sup>/<sub>-</sub></button>
      <button @click="displayNum = '0'">0</button>
      <button @click="displayNum = '.'">.</button>
      <button @click="solve">=</button>

    </div>
  </div>
</template>
<script>
import { vModelCheckbox } from 'vue';


export default {
  name: "CalcProject",
  props: {
    msg: String
  },

  data() {
    return {
      operator: '+',
      firstNum: '0',
      secondNum: '0',
      res: '0',
      isoper: false
    }
  },
  //compute is basically compute the value of display number which could be the first one or the second 
  //and if we want to add some digits to the same number we use the set function 
  //note that when we call set() we say displayNum = 'newValue' don't treat it as function

  computed: {
    displayNum: {
      get() {
        return this.isoper && this.secondNum !== '0' ? this.secondNum : this.firstNum;
      },
      set(newValue) {
        if (this.isoper) {
          if (newValue == '.' && !this.check(this.secondNum))
            return;
          this.secondNum = this.secondNum == '0' ? newValue : this.secondNum + newValue;
        }
        else if (!this.isoper) {
          if (newValue == '.' && !this.check(this.firstNum))
            return;
          this.firstNum = this.firstNum == '0' ? newValue : this.firstNum + newValue;
        }
      }
    },
    operation() {
      if (this.isoper && this.secondNum == '0') return `${this.firstNum} ${this.operator}`;
      if (this.isoper && this.secondNum != '0') return `${this.firstNum} ${this.operator} ${this.secondNum}`
    }

  },
  methods: {
    setOper(value) {
      //if isoper ? solve and put the result in num 1
      if (this.isoper && this.secondNum !== '0')
        this.solve();
      this.operator = value;
      this.isoper = true;
    },
    solve() {
      var num1 = parseFloat(this.firstNum);
      var num2 = parseFloat(this.secondNum);
      console.log(num1);
      console.log(num2);
      switch (this.operator) {
        case '+':
          this.firstNum = (num1 + num2);
          break;
        case '-':
          this.firstNum = (num1 - num2);
          break;
        case '*':
          this.firstNum = (num1 * num2);
          break;
        case '/':
          if (num2 != 0)
            this.firstNum = (num1 / num2);
          break;
      }
      this.secondNum = '0';
      this.isoper = false;
    },
    check(num) {
      return num.indexOf('.') === -1 ? true : false;
    }
  },

}

</script>


<style >
body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
}

.calculator {
  border: 1px solid #3e3e42;
  border-radius: 5px;
  overflow: hidden;
  width: 400px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.display {
  padding: 20px;
  font-size: 1.5em;
  text-align: right;
  background-color: #252526;
  color: #d7d7e4;
}

.history {
  font-size: 1em;
}

.buttons {
  display: grid;
  height: 400px;
  grid-template-columns: repeat(4, 1fr);
}

button {
  padding: 15px;
  font-size: 1em;
  border: 1px solid #2d2d30;
  cursor: pointer;
  background-color: #46464a;
}

button:active {
  background-color: #5a5a5f;
}

button.equals {
  grid-column: span 2;
}

button.clear {
  grid-column: span 4;
  background-color: #f44336;
  color: #fff;
}
</style>