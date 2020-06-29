<template>
  <div class="calculator-wrapper">
    <div class="calculator">
      <div class="calculator__display">
        <div class="calculator__display__stack">
          <div v-for="(value,i) of stack" :key="i">{{value}}</div>
        </div>
        <input v-if="singleLineMode"
               type="text"
               @keyup="insertEquation"
               v-model="equation"
               @keypress.enter="executeEquation"/>
        <input v-else
               disabled
               :value="currentDisplayedValue"/>
      </div>
      <div class="calculator__controls">
        <div class="calculator__controls__checkbox">Single line input
          <input type="checkbox" :checked="singleLineMode" @change="toggleSingleMode">
          <div class="info-icon">
            <img src="../assets/images/info.jpg" alt="info">
            <div>By checking single line input, you are allowed only to insert values
              in a single line and execute at the end, example: " 5 10 15 + - "
            </div>
          </div>
        </div>
        <div class="calculator__controls__actions">
          <div class="btn btn--1-of-2 btn--orange" @click="enter">ENTER</div>
          <div class="btn btn--1-of-4 btn--green" @click="reset">AC</div>
          <div class="btn btn--1-of-4 btn--green" @click="clear">Clear</div>
        </div>
        <div class="calculator__controls__numbers">
          <div>
            <div class="btn btn--1-of-3" data-value="0" @click="updateDisplayedValue">0</div>
            <div class="btn btn--1-of-3" data-value="." @click="updateDisplayedValue">.</div>
            <div class="btn btn--1-of-3" data-value="3.14" @click="updateDisplayedValue">PI</div>
            <div
              v-for="(number,i) in numbers"
              class="btn btn--1-of-3"
              :data-value="number"
              @click="updateDisplayedValue"
              :key="i">{{number}}
            </div>
          </div>
          <div>
            <div class="btn btn--green" data-operation="/" @click="executeOperation">&divide;</div>
            <div class="btn btn--green" data-operation="*" @click="executeOperation">&times;</div>
            <div class="btn btn--green" data-operation="-" @click="executeOperation">&minus;</div>
            <div class="btn btn--green" data-operation="+" @click="executeOperation">&plus;</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      stack: [],
      temporaryStack: [],
      numbers: [1,2,3,4,5,6,7,8,9],
      singleLineMode: true,
      currentDisplayedValue: '',
      equation: '',
      operations: {
        "+": (a,b) => parseFloat(a) + parseFloat(b),
        "-": (a,b) => parseFloat(b) - parseFloat(a),
        "*": (a,b) => parseFloat(a) * parseFloat(b),
        "/": (a,b) => parseFloat(b) / parseFloat(a),
      }
    }
  },
  methods: {
    enter() {
      if (this.singleLineMode) {
        this.executeEquation();
        return;
      }
      let value = parseFloat(this.currentDisplayedValue);
      let valueLastChar = this.currentDisplayedValue.slice(-1);
      if (isNaN(value) || valueLastChar == '.') {
        return
      }
      if (value === 0) {
        this.stack.push(0);
      } else {
        this.stack.push(value);
      }
      this.currentDisplayedValue = '';
    },
    reset() {
      this.stack = [];
      if (this.singleLineMode) {
        this.equation = '';
        return
      }
      this.currentDisplayedValue = '';
    },
    clear() {
      if (this.singleLineMode) {
        this.equation = this.equation.slice(0,-1);
        return
      }
      this.currentDisplayedValue = this.currentDisplayedValue.slice(0,-1);
    },
    updateDisplayedValue(event) {
      let value = event.target.getAttribute("data-value");
      if (this.singleLineMode) {
        this.equation += value;
        return;
      }
      if (!this.currentDisplayedValue) {
        this.currentDisplayedValue = value;
      } else {
        this.currentDisplayedValue += value;
      }
    },
    validStackLength() {
      return this.stack.length >= 2;
    },
    concatMinusSign(operation) {
      if (operation === '-' && this.currentDisplayedValue !== '') {
        // concat '-' sign if there is a digit inserted in the input
        this.currentDisplayedValue = '-' + this.currentDisplayedValue;
        return true;
      }
    },
    executeOperation(event) {
      let operation = event.target.getAttribute("data-operation");
      if(!this.validStackLength()) {
        this.concatMinusSign(operation);
        return;
      }
      if(this.concatMinusSign(operation)) {
        return;
      }

      let operationFn;
      Object.keys(this.operations).map(item => {
        if (item === operation) {
          operationFn = this.operations[item];
        }
      });
      let a = this.stack.pop();
      let b = this.stack.pop();
      this.stack.push(operationFn(a,b));
    },
    insertEquation(event) {
      let lastValue = event.target.value.split('').pop();
      let rgx = /[\d*+-/ ]+/;
      if (!rgx.test(lastValue)) {
        this.equation = this.equation.slice(0,-1);
      }
    },
    toggleSingleMode() {
      this.singleLineMode = !this.singleLineMode;
      this.equation = '';
      this.stack = [];
      this.currentDisplayedValue = '';
    },
    executeEquation() {
      this.temporaryStack = [];
      let equation = this.equation.trim().split(' ');
      equation.forEach(value => {
        if (this.operations[value]) {
          let operationFn = this.operations[value];
          let nrOfParams = operationFn.length; // returns the nr of params the function needs
          let params = [];
          for (let i = 0; i < nrOfParams; i++) {
            params.push(this.temporaryStack.pop());
          }
          this.temporaryStack.push(operationFn(...params));
        } else {
          this.temporaryStack.push(value);
        }
      });
      this.stack.length = 0; // reset stack
      let result = parseFloat(this.temporaryStack.pop());
      if (isNaN(result)) {
        result = "0";
      }
      this.stack.push(result);
      this.equation = '';
    }
  }
}
</script>

<style lang="scss" scoped>
  .calculator-wrapper {
   display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 50px;
  }

  .calculator {
    color: #fff;
    width: 450px;
    border-radius: 10px;
    background-color: #000;
    overflow: hidden;

    &__display {
      height: 250px;
      background-color: rgb(204, 201, 182);
      color: #000;
      font-size: 30px;
      text-align: right;
      padding: 15px 40px;
      position: relative;

      input {
        width: 100%;
        position: absolute;
        bottom: 0;
        left: 0;
        padding: 5px 20px;
        outline: none;
        text-align: right;
        font-size: 30px;
        background-color: rgb(204, 201, 182);
        border: none;
        border-top: 1px solid #fafafa;
        &:disabled {
          color: #000;
        }
      }

      &__stack {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        max-height: 170px;
        overflow: scroll;
        padding-right: 15px;
      }
    }
    
    &__controls {
      background-color: #000;
      padding: 10px;

      &__checkbox {
        margin: 0 8px;
        display: flex;
        align-items: center;
        input {
          cursor: pointer;
          margin: 0 10px;
        }

        .info-icon {
          width: 15px;
          display: flex;
          align-items: center;
          position: relative;
          &:hover {
            div {
              display: block;
            }
          }

          div {
            position: absolute;
            bottom: 100%;
            left: 0;
            width: 200px;
            background-color: #000;
            padding: 8px;
            border-radius: 4px;
            display: none;
          }
          img  {
            max-width: 100%;
            cursor: pointer;
          }
        }
      }

      .btn {
        padding: 10px;
        border-radius: 5px;
        margin: 8px;
        text-align: center;
        background: linear-gradient(0deg, rgba(10,10,10,1) 0%, rgba(82,78,78,1) 95%);
        cursor: pointer;
        min-width: 80px;
        font-size: 20px;

        &--1-of-2 {
          width: calc((100% / 2) - 16px);
        }

        &--1-of-3 {
          width: calc((100% / 3) - 16px);
        }

        &--1-of-4 {
          width: calc((100% / 4) - 16px);
        }

        &--green{
          background: linear-gradient(0deg, rgba(23,31,32,1) 0%, rgba(35,151,175,1) 99%);
        }

        &--orange{
          background: linear-gradient(0deg, rgba(171,110,37,1) 0%, rgba(255,143,0,0.9934348739495799) 99%);
        }
      }
      
      &__actions {
        display: flex;
        align-items: center;
        justify-content: space-around;
      }
      
      &__numbers {
        display: flex;
        > div:first-child {
          display: flex;
          flex-wrap: wrap-reverse;
        }

        > div:last-child {
          display: flex;
          flex-direction: column;
        }
      }
    }
  }
</style>
