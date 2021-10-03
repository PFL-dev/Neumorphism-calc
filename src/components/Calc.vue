<script>
export default {
  name: "Calc",
  data() {
    return {
      display: 0,
      numberToggle: 1,
      firstNumber: [],
      secondNumber: [],
      operator: undefined,
      handleKeyPress: () => {
        let keyPress = (event) => {
          if (!isNaN(event.key)) {
            this.numberSet(event);
          } else if (event.key === "+" || event.key ===  "-" || event.key ===  "*" || event.key ===  "/") {
            this.setOperator(event);
          } else if (event.key === "=" || event.key ===  "Enter") {
            this.calculate();
          }
        }
        document.addEventListener("keydown", keyPress)
      },
      numberSet: (event) => {
        let numberInput;
        if(event.key) {
          numberInput = event.key;
        } else if (!isNaN(event.target.textContent)) {
          numberInput = +event.target.textContent;
        }

        if ((event.target !== event.currentTarget) && numberInput) {
          if (this.numberToggle === 1 && this.firstNumber.length < 9) {
            this.firstNumber += numberInput;
            this.display = this.firstNumber;
          } else if (this.numberToggle === 2 && this.secondNumber.length < 9) {
            this.secondNumber += numberInput;
            this.display = this.secondNumber;
          }
        }
      },
      resetNumber: () => {
        this.display = 0;
        this.firstNumber = [];
        this.secondNumber = [];
        this.numberToggle = 1;
      },
      setOperator: (event) => {
        
        let operator;
        if (event.key) {
          operator = event.key;
        } else if (event.target.textContent) {
          operator = event.target.textContent
        }
 
        if (this.numberToggle === 1) {
          this.numberToggle = 2;
          this.operator = operator;
        } else if (this.numberToggle === 2) {
          this.calculate();
        }
      },
      calculate: () => {
        let result;

        function expo(x, f) {
          return Number.parseFloat(x).toExponential(f);
        }

        if (this.operator === "+") {
          result = +this.firstNumber + +this.secondNumber;
        } else if (this.operator === "-") {
          result = +this.firstNumber - +this.secondNumber;
        } else if (this.operator === "X" || this.operator === "*") {
          result = +this.firstNumber * +this.secondNumber;
        } else if (this.operator === "/") {
          if (+this.secondNumber !== 0) {
            result = +this.firstNumber / +this.secondNumber;
          } else {
            result = "Error";
          }
        } else {
          result = this.firstNumber.length > 0 ? this.firstNumber : 0;
        }

        if (result.toString().length <= 9 || result === "Error") {
          this.display = result;
        } else if (result.toString().length > 9 && result !== "Error") {
          this.display = expo(result, 2);
        } else {
          this.display = "Error";
        }

        this.numberToggle = 1;
        this.firstNumber = result;
        this.secondNumber = [];
      },
    };
  },
  beforeMount() {
    this.handleKeyPress();
  }
};
</script>

<template>
  <div class="calc">
    <div class="calc__main">
      <div class="calc__main__result">{{ display }}</div>
      <div class="calc__main__signs">
        <div class="calc__main__signs__numpad" v-on:click="numberSet">
          <span class="btn-calc">7</span>
          <span class="btn-calc">8</span>
          <span class="btn-calc">9</span>
          <span class="btn-calc">4</span>
          <span class="btn-calc">5</span>
          <span class="btn-calc">6</span>
          <span class="btn-calc">1</span>
          <span class="btn-calc">2</span>
          <span class="btn-calc">3</span>
          <span class="btn-calc btn-calc--zero-nbr">0</span>
          <span class="btn-calc">.</span>
        </div>
        <div class="calc__main__signs__operators">
          <span class="btn-calc" v-on:click="resetNumber">AC</span>
          <span class="btn-calc" v-on:click="setOperator">/</span>
          <span class="btn-calc" v-on:click="setOperator">-</span>
          <span class="btn-calc" v-on:click="setOperator">X</span>
          <span class="btn-calc btn-calc--plus-equal" v-on:click="setOperator"
            >+</span
          >
          <span class="btn-calc btn-calc--plus-equal" v-on:click="calculate"
            >=</span
          >
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap");
.calc {
  font-family: "Orbitron", sans-serif;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #786b80;
  color: #e3d4ec;
  ::selection {
    background-color: #e3d4ec;
    color: #786b80;
  }
  &__main {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
    padding: 1.5rem;
    width: 540px;
    height: 520px;
    border-radius: 30px;
    box-shadow: 20px 20px 60px #665b6d, -20px -20px 60px #8a7b93;
    &__result {
      align-self: center;
      border-radius: 10px;
      width: 420px;
      font-size: 3rem;
      text-align: center;
      box-shadow: inset 5px 5px 10px #665b6d, inset -5px -5px 10px #8a7b93;
    }
    &__signs {
      display: flex;
      justify-content: space-between;
      &__numpad {
        width: 285px;
        display: flex;
        flex-wrap: wrap;
      }
      &__operators {
        width: 200px;
        display: flex;
        flex-wrap: wrap;
        align-items: flex-end;
      }
    }
  }
  .btn-calc {
    user-select: none;
    cursor: pointer;
    display: flex;
    justify-content: center;
    margin: 10px;
    align-items: center;
    width: 75px;
    height: 75px;
    border-radius: 10px;
    box-shadow: 6px 6px 12px #665b6d, -6px -6px 12px #8a7b93;
    &:active {
      box-shadow: inset 6px 6px 12px #665b6d, inset -6px -6px 12px #8a7b93;
    }
    &--zero-nbr {
      width: 170px;
    }
    &--plus-equal {
      height: 120px;
    }
  }
}
</style>
