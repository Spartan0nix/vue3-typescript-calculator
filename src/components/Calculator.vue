<template>
  <div class="calculator">
      <section class="display">
          <p class="total" v-show="trace != '0'">{{ trace }}</p>
          <p>{{ display }}</p>
        </section>
      <section class="clear-number">
          <button class="btn btn-clear" @click="clear">C</button>
          <button class="btn btn-operator btn-erase" @click="erase">
              <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-backspace" width="35" height="35" viewBox="0 0 24 24" stroke-width="1" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M20 6a1 1 0 0 1 1 1v10a1 1 0 0 1 -1 1h-11l-5 -5a1.5 1.5 0 0 1 0 -2l5 -5z" />
                <path d="M12 10l4 4m0 -4l-4 4" />
            </svg>
          </button>
      </section>
      <section class="user-panel">
          <div class="number">
              <button class="btn btn-number" @click="append('7')">7</button>
              <button class="btn btn-number" @click="append('8')">8</button>
              <button class="btn btn-number" @click="append('9')">9</button>
              <button class="btn btn-number" @click="append('4')">4</button>
              <button class="btn btn-number" @click="append('5')">5</button>
              <button class="btn btn-number" @click="append('6')">6</button>
              <button class="btn btn-number" @click="append('1')">1</button>
              <button class="btn btn-number" @click="append('2')">2</button>
              <button class="btn btn-number" @click="append('3')">3</button>
              <button class="btn btn-number" @click="append('0')">0</button>
              <button class="btn btn-number" @click="coma">.</button>
              <button class="btn btn-result" @click="equal">=</button>
          </div>
          <div class="operator">
              <button class="btn btn-operator" @click="divide">/</button>
              <button class="btn btn-operator" @click="times">x</button>
              <button class="btn btn-operator" @click="subtract">-</button>
              <button class="btn btn-operator" @click="add">+</button>
          </div>
      </section>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    setup() {
        const display = ref<string>('0');
        const trace = ref<string>('0');

        const clear = () => {
            display.value = '0';
            trace.value = '0';
        }

        const erase = () => {
            if(display.value.length === 1){
                display.value = '0';
                return
            }
            if(display.value != '0'){
                display.value = display.value.slice(0,-1);
                return
            }   
        }

        const append = (num: string) => {
            if(display.value === '0'){
                display.value = num
                return
            }
            display.value += num;
        }

        const coma = () => {
            if(display.value.toString().search(',') > -1){
                return
            }
            display.value += '.'
        }

        const updateTrace = (operator: string) => {
            if(display.value != '0'){
                trace.value === '0' ? trace.value = display.value : trace.value += ` ${display.value}`
            }
            if(trace.value.toString().slice(-1) === operator){
                return
            }
            trace.value += ` ${operator}`
            display.value = '0';
        }

        const add = () => {
            updateTrace('+')
        }

        const divide = () => {
            updateTrace('/')
        }

        const times = () => {
            updateTrace('*')
        }

        const subtract = () => {
            updateTrace('-')
        }

        const equal = () => {
            trace.value += display.value;
            display.value = '0'
            trace.value = eval(trace.value);
        }

        return { display, trace, clear, erase, append, coma, add, divide, times, subtract, equal }
    },
})
</script>

<style>
    .calculator {
        display: flex;
        flex-direction: column;
        max-width: 750px;
        margin: 0 auto;
    }
    .display {
        display: flex;
        flex-direction: column;
        font-size: 2rem;
        background-color: #474747;
        color: #F4F5F6;
        border: 1px solid #F7F5F3;
        align-items: flex-end;
    }
    .display p {
        margin: 0.3rem 1.4rem 0.4rem 0;
    }
    .display .total {
        font-size: 1rem;
        margin: 0.8rem 1.4rem 0 0;
    }
    .clear-number {
        display: flex;
    }
    .btn-clear {
        flex-grow: 1;
    }
    .user-panel {
        display: flex;
    }
    .number {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        flex-grow: 1;
    }
    .operator {
        display: flex;
        flex-direction: column;
    }
    /* Button */
    .btn {
        padding: 8px;
        font-size: 1.8rem;
        min-width: 6rem;
        background-color: #474747;
        color: #F4F5F6;
        outline: none;
        border: 1px solid #F7F5F3;
    }
    .btn:hover {
        cursor: pointer;
        filter: brightness(0.9);
    }
    .btn-erase {
        display: flex;
        justify-content: center;
    }
    .btn-result {
        background-color: #1A936F;
    }
    .btn-operator {
        background-color: #AA200E;
    }
</style>