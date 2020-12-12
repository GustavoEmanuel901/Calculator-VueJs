<template>
  <div class="calculator">
      <Display :value='displayValue'/>
      <Data label='AC' triple @onCalcButtonClick='clearMemory'/>
      <Data label='/' operation @onCalcButtonClick='setOperation'/>
      <Data label='7' @onCalcButtonClick='addDigit'/>
      <Data label='8' @onCalcButtonClick='addDigit'/>
      <Data label='9' @onCalcButtonClick='addDigit'/>
      <Data label='x' operation @onCalcButtonClick='setOperation'/>
      <Data label='4' @onCalcButtonClick='addDigit'/>
      <Data label='5' @onCalcButtonClick='addDigit'/>
      <Data label='6' @onCalcButtonClick='addDigit'/>
      <Data label='-' operation @onCalcButtonClick='setOperation'/>
      <Data label='1' @onCalcButtonClick='addDigit'/>
      <Data label='2' @onCalcButtonClick='addDigit'/>
      <Data label='3' @onCalcButtonClick='addDigit'/>
      <Data label='+' operation @onCalcButtonClick='setOperation'/>
      <Data label='0' @onCalcButtonClick='addDigit' double/>
      <Data label='.' @onCalcButtonClick='addDigit'/>
      <Data label='=' operation @onCalcButtonClick='setOperation'/>
  </div>
</template>

<script>
    import Display from '../components/Display.vue'
    import Data from '../components/Data.vue'

    export default {
        data: function() {
            return { 
                displayValue: '0',
                clearDisplay: false,
                operation: null,
                values: [0, 0],
                current: 0
            }
        },
        components: { Data, Display },
        methods: { 
            clearMemory() {
                Object.assign(this.$data, this.$options.data())
            },
            setOperation(operation){
                if(this.current === 0) {
                    this.operation = operation
                    this.current = 1
                    this.clearDisplay = true
                } else {
                    const equals = operation === '='
                    const currentOperation = this.operation

                    try {
                        this.values[0] = eval(
                            `${this.values[0]} ${currentOperation} ${this.values[1]}`
                        )
                    } catch (err) {
                        this.$emit('onError', err)
                    }

                    this.values[1] = 0
                    this.displayValue = this.values[0]
                    this.operation = equals ? null : operation
                    this.current = equals ? 0 : 1
                    this.clearDisplay = !equals
                }
            },
            addDigit(n) {
                if (n === '.' && this.displayValue.includes('.')) return

                const clearDisplay = this.displayValue === '0'|| this.clearDisplay
                const currentValue = clearDisplay ? '' : this.displayValue
                const display = currentValue + n

                this.displayValue = display
                this.clearDisplay = false

                if (n !== '.') {
                    const i = this.current
                    const newValue = parseFloat(display)
                    this.values[i] = newValue
                }
            }
        }
    }
</script>

<style>
    .calculator {
        height: 320px;
        width: 235px;
        border-radius: 5px;
        overflow: hidden;

        display: grid;
        grid-template-columns: repeat(4, 25%);
        grid-template-rows: 1fr 48px 48px 48px 48px 48px;
    }

</style>