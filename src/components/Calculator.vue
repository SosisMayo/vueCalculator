<template>
  <div class="calculator">
    <div class="container">
      <div class="previousDisplay">{{previous}} {{operatornya}}</div>
      <div class="display">{{current || '0'}}</div>
      <div @click="clear" class="btn">C</div>
      <div @click="percent" class="btn">%</div>
      <div @click="hapus" class="delete btn">DEL</div>
      <div @click="divide" class="btn operator">÷</div>
      <div @click="number('7')" class="btn">7</div>
      <div @click="number('8')" class="btn">8</div>
      <div @click="number('9')" class="btn">9</div>
      <div @click="times" class="btn operator">x</div>
      <div @click="number('4')" class="btn">4</div>
      <div @click="number('5')" class="btn">5</div>
      <div @click="number('6')" class="btn">6</div>
      <div @click="minus" class="btn operator">-</div>
      <div @click="number('1')" class="btn">1</div>
      <div @click="number('2')" class="btn">2</div>
      <div @click="number('3')" class="btn">3</div>
      <div @click="plus" class="btn operator">+</div>
      <div @click="negative" class="btn negative">±</div>
      <div @click="number(0)" class="zero btn">0</div>
      <div @click="dot" class="btn">.</div>
      <div @click="total" class="btn operator total">=</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data(){
    return {
      previous: "",
      current: '',
      operator: null,
      operatorClick: false,
      operatorSelanjutnya: false,
      operatornya:'',
      terakhir:'',
      samadengan:0,
    }
  },
  methods:{
    setPrevious(){
      this.operatorClick = true;
      this.previous = this.current;
    },
    clear(){
      this.current= '';
      this.previous='';
      this.operatorSelanjutnya=false;
      this.operatorClick=false;
      this.operatornya='';
    },
    negative(){
      this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`;
    },
    percent(){
      this.current = `${parseFloat(this.current)/100}`;
    },
    number(num){
      if(this.operatorClick){
        this.current='';
        this.operatorClick=false;
        this.operatorSelanjutnya=true;
      }
      if(this.samadengan>0 && this.operatorClick==false){
        this.clear();
        this.samadengan=0;
      }
      if(num==0 && this.current=="" && this.previous==""){
        this.current = "";
      }
      else{
      this.current = this.current + num;
      }
    },
    dot(){
      if(this.current.indexOf('.') === -1){
        this.number('.');
      }
    },
    divide(){
      if(this.operatorSelanjutnya){
        this.total();
        this.operatorSelanjutnya=false;
      }
      this.operator = (a,b) => a/b;
      this.setPrevious();
      this.operatornya='÷';
    },
    times(){
      if(this.operatorSelanjutnya){
        this.total();
        this.operatorSelanjutnya=false;
      }
      this.operator = (a,b) => a*b;
      this.setPrevious();
      this.operatornya='x';
    },
    plus(){
      if(this.operatorSelanjutnya){
        this.total();
        this.operatorSelanjutnya=false;
      }
      this.operator = (a,b) => a+b;
      this.setPrevious();
      this.operatornya='+';
    },
    minus(){
      if(this.operatorSelanjutnya){
        this.total();
        this.operatorSelanjutnya=false;
      }
      this.operator = (a,b) => a-b;
      this.setPrevious();
      this.operatornya='-';
    },
    total(){
      this.terakhir=`${this.previous + this.operatornya + this.current}`;
      this.current= `${this.operator(parseFloat(this.previous),parseFloat(this.current))}`;
      this.previous=this.terakhir;
      this.operatornya='=';
      this.samadengan++;
      this.operatorSelanjutnya=false;
    },
    hapus(){
      this.current=this.current.slice(0,(this.current.length)-1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  margin: 0 auto;
  width: 50%;
  font-size: 50px;
  display:grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display{
  grid-column: 1/5;
  background-color: black;
  opacity: .7;
  color: white;
  text-align: right;
}

.btn{
  background-color: rgb(238, 231, 231);
  border: 1px solid black;
}

.btn:hover{
  background-color: lightgrey;
}

.negative{
  border-bottom-left-radius: 20px ;
}

.total{
  border-bottom-right-radius: 20px ;
}

.operator{
  background-color: rgb(87, 201, 209);
}

.operator:hover{
  background-color: rgb(72, 164, 170);
}

.previousDisplay{
  grid-column: 1/5;
  text-align: right;
  font-size: 20px;
  background-color: black;
  opacity: .7;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  color: whitesmoke;
  padding: 10px;
}
</style>
