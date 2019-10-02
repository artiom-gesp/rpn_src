<template>
    <form @submit="calc" action="javascript:void(0);" id="rpn-form">
        <input type="text" pattern="([-]{0,1}[0-9]+[.]{0,1}[0-9]*[.+\*x\-\/ ]*)+" ref="input" id="rpn-input"/>
        <button type="submit" id="rpn-calc">Calculate</button>
        <div v-if="result !== ''" id="rpn-result">{{ result }}</div>
    </form>
</template>

<script>
    export default {
        name: "rpn",
        methods: {
            calc: function () {
                var stack = [];
                var size = 0;
                let expression = this.$refs.input.value;

                for (var i = 0; i < expression.length; i++) {

                    // Skip spaces.
                    while (expression[i] === ' ')
                        i++;

                    // If char is a number -> find next element to push on the stack.
                    if (!isNaN(expression[i]) || (expression[i] === '-' && !isNaN(expression[i + 1])
                    && expression[i + 1] !== ' ')) {
                        let parsed = parseFloat(expression.substring(i, expression.length));
                        if (typeof parsed !== 'undefined') {
                            let toPush = parsed;
                            ++size;
                            stack.push(toPush);
                            i += toPush.toString().length;
                        }
                    }

                    // If char is not a number than it is either an operator or a dot.
                    // If it is a dot (that is not part of any real number), skip it.
                    else if (expression[i] && expression[i] !== '.') {
                        let v1 = stack.pop();
                        let v2 = stack.pop();

                        // If poped elements are not defined, the expression is not valid.
                        // If it is valid, evaluate it.
                        if (!isNaN(v1) && !isNaN(v2)) {
                            stack.push(eval(v2 + ' ' + (expression[i] === 'x' ? '*' : expression[i]) + ' ' + v1))
                            --size;
                        }
                        else {
                            alert('Please enter a valid expression');
                            return;
                        }
                    }
                }
                if (size === 1)
                    this.result = stack.pop();
                else
                    alert('Please enter a valid expression');
            }
        },
        data() {
            return {
                result: ''
            }
        }
    }
</script>

<style scoped>
    #rpn-form
    {
        position: relative;
        top: 50%;
        font-family: unset;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    #rpn-form > button, #rpn-form > div, #rpn-form > input
    {
        height: 40px;
        padding: 10px;
        box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.5);
        border-radius: 10px;
        outline: none;
    }

    #rpn-input{
        width: 500px;
        border-radius: 10px;
        border: 1px solid lightblue;
    }

    #rpn-calc
    {
        width: 100px;
        margin-left: 10px;
        border: 1px solid #004a69;
        background: #004a69;
        color: white;
        font-weight: bold;
        -webkit-box-sizing: content-box;
        -moz-box-sizing: content-box;
        box-sizing: content-box;
    }

    #rpn-result
    {
        position: absolute;
        top: -120px;
        left: 50%;
        transform: translateX(-50%);
        border: 1px solid lightblue;
        background: white;
        width: auto;
        border-radius: 50% !important;
        vertical-align: middle;
        line-height: 40px;
    }

</style>