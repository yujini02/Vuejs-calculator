<script>
  export default {
    data(){
      return {
        output : null, //화면 표시값
        prev : null, //이전 값(누적값)
        cur : null, //현재 입력값
        operator : null, // 연산자 저장
        operatorActions : {
          '+' : (a, b) => a + b, //덧셈 연산 함수
          '-' : (a, b) => a - b,
          '*' : (a, b) => a * b,
          '/' : (a, b) => a / b
        }
      }
    },
    methods:{
      clear(){
        this.output = null;
        this.prev = null;
        this.cur = null;
        this.operator = null;
      },
      calculate(n){
        if(!this.cur && !this.prev){
          alert('숫자를 먼저 입력하세요');
          return;
        }
        if(this.operator !== '' && !this.cur){
          alert('사칙연산 기호를 연달아 누를 수 없습니다.');
          return;
        }
        if(n === '=' && this.prev === this.cur){
          return;
        }
        this.cur = Number(this.cur);  //문자열을 숫자로 변환
        if(this.operator !== null){
          this.prev=this.operatorActions[this.operator](this.prev, this.cur); //a=this.prev , b=this.cur
          if(n === '='){
            // = 버튼을 눌렀다면 결과를 화면에 출력
            this.output = this.prev; //결과값
            this.operator = null; // 다음 연산을 위해 연산자 초기화
            this.cur = this.prev; // 결과값을 현재값으로 유지 (계속 계산 가능하게)
          } else{
            // =이 아닌 다른 연산자(+ - * /)를 눌렀을 경우
            this.output = null; // 화면 지우고
            this.operator = n; // 새로운 연산자를 저장
            this.cur = null;  // 다음 숫자 입력을 위해 현재값 초기화
          }
        } else{
          // 이전에 연산자가 없는 경우 (첫 번째 연산자 클릭 상황)
          this.output = null; // 화면 지우기
          this.operator = n; //연산자 저장
          this.prev = this.cur; // 현재 숫자를 이전 숫자에 저장
          this.cur = null; // 다음 숫자 입력을 위해 현재값 초기화
        }
      },
      userInput(n){
        this.cur = this.cur === null ? n : (this.cur += n); //null이 아니면 n을 더함(문자열로)
        this.output = this.cur;
      },
      operation(e){
        const n = e.target.value; //value값을 n에 넣음
        console.log(n);
        if(n === 'C'){
          this.clear();
        }else if(['+','-','*','/','='].includes(n)){ //연산자일 경우
          this.calculate(n);
        }else{
          this.userInput(n); //숫자일 경우
        }
      }

    }
  }
</script>

<template>
    <div class="calculator">
      <form name="forms">
        <input type="text" name="output" v-model="output" readonly />
        <input type="button" class="clear" value="C" @click="operation" />
        <input type="button" class="operator" value="/" @click="operation" />
        <input type="button" value="1" @click="operation" />
        <input type="button" value="2" @click="operation" />
        <input type="button" value="3" @click="operation" />
        <input type="button" class="operator" value="*" @click="operation" />
        <input type="button" value="4" @click="operation" />
        <input type="button" value="5" @click="operation" />
        <input type="button" value="6" @click="operation" />
        <input type="button" class="operator" value="+" @click="operation" />
        <input type="button" value="7" @click="operation" />
        <input type="button" value="8" @click="operation" />
        <input type="button" value="9" @click="operation" />
        <input type="button" class="operator" value="-" @click="operation" />
        <input type="button" class="dot" value="." @click="operation" />
        <input type="button" value="0" @click="operation" />
        <input type="button" class="operator result" value="=" @click="operation" />
      </form>
    </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.calculator {
  width: 287px;
  border: 1px solid #333;
  background-color: #ccc;
  padding: 5px;
}

.calculator form {
  display: grid;
  grid-template-columns: repeat(4, 65px);
  grid-auto-rows: 65px;
  grid-gap: 5px;
}

.calculator form input {
  border: 2px solid #333;
  cursor: pointer;
  font-size: 19px;
}

.calculator form input:hover {
  box-shadow: 1px 1px 2px #333;
}

.calculator form .clear {
  background-color: #ed4848;
}

.calculator form .operator {
  background-color: orange;
}

.calculator form .dot {
  background-color: green;
}

.calculator form input[type='text'] {
  grid-column: span 4;
  text-align: right;
  padding: 0 10px;
}

.calculator form .clear {
  grid-column: span 3;
}

.calculator form .result {
  grid-column: span 2;
}
</style>
