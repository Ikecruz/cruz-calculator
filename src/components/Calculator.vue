<template>
  <div class="cal-contain">
      <div class="display" :class="{ 'fontchange' : font }">{{ current || 0 }}</div>
      <div @click="clear" class="btn rand">C</div>
      <div @click="sign" class="btn rand">+/-</div>
      <div @click="percent" class="btn rand">%</div>
      <div @click="divide" class="btn operator">&div;</div>
      <div @click="append(7)" class="btn">7</div>
      <div @click="append(8)" class="btn">8</div>
      <div @click="append(9)" class="btn">9</div>
      <div @click="times" class="btn operator">x</div>
      <div @click="append(4)" class="btn">4</div>
      <div @click="append(5)" class="btn">5</div>
      <div @click="append(6)" class="btn">6</div>
      <div @click="minus" class="btn operator">-</div>
      <div @click="append(1)" class="btn">1</div>
      <div @click="append(2)" class="btn">2</div>
      <div @click="append(3)" class="btn">3</div>
      <div @click="add" class="btn operator">+</div>
      <div @click="append(0)" class="btn zero">0</div>
      <div @click="dot" class="btn">.</div>
      <div @click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
    data() {
        return{
            previous: null,
            current: '',
            operator: '',
            operatorClicked: false,
            font: false,
        }
    },
    methods: {
        clear() {
            this.current = ''
            this.font = false
        },
        sign() {
            this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
        },
        percent() {
            this.current = `${parseFloat(this.current) / 100}`
        },
        append(num) {
            if (this.operatorClicked) {
                this.current = ''
                this.operatorClicked = false;
            }
            this.current = `${this.current}${num}`
            this.fontMod()
            console.log(this.current.length)
            console.log(this.font)
        },
        dot() {
            if (this.current.indexOf('.') === -1 ) {
                this.append('.')
            }
        },
        setPrevious(){
            this.previous = this.current
            this.operatorClicked = true
        },
        divide() {
            this.operator = (a, b) => b / a
            this.setPrevious()
        },
        times() {
            this.operator = (a, b) => a * b
            this.setPrevious()
        },
        minus() {
            this.operator = (a, b) => b - a
            this.setPrevious()
        },
        add() {
            this.operator = (a, b) => a + b
            this.setPrevious()
        },
        equal() {
            if (this.current != '' && this.previous != '') {
                this.current = `${this.operator(
                    parseFloat(this.current),
                    parseFloat(this.previous)
                )}`
                this.previous = null 
            } else {
                console.log('Empty operands')
            } 
        },
        fontMod() {
            if (this.current.length >= 8) {
                this.font = true
            }
        }
    }
}
</script>

<style scoped>
    .cal-contain{
        margin: 0 auto;
        width: 280px;
        font-size: 1.8em;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(70px, auto);
        background: black;
    }
    .display{
        grid-column: 1 / 5;
        padding-right: 20px;
        display: flex;
        justify-content: flex-end;
        align-items: center;
        color: white;
        font-size: 1.8em;
        font-weight: 100;
        transition: 0.5s;
    }
    .btn{
        background: #111;
        margin: 5px;
        padding: 5px;
        border-radius: 90px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        cursor: pointer;
    }
    .zero{
        grid-column: 1 / 3;
        padding-left: 15px;
        justify-content: flex-start;
    }
    .operator{
        background: orange;
    }
    .rand{
        background: rgb(138, 138, 138);
        color: black;
    }
    .fontchange{
        font-size: 1.3em;
    }
</style>