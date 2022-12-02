<template lang="">
    <div class="calc">
        <div class="logo"></div>
        <div class="screen">{{ screen }}</div>
        <calc-button @click="digit('7')" class="digit">7</calc-button>
        <calc-button @click="digit('8')" class="digit">8</calc-button>
        <calc-button @click="digit('9')" class="digit">9</calc-button>
        <calc-button @click="action('divide')" class="actions">&divide;</calc-button>
        <calc-button @click="action('backspace')" class="backspace">&#9003;</calc-button>
        <calc-button @click="action('clear')" class="clear">C</calc-button>
        <calc-button @click="digit('4')" class="digit">4</calc-button>
        <calc-button @click="digit('5')" class="digit">5</calc-button>
        <calc-button @click="digit('6')" class="digit">6</calc-button>
        <calc-button @click="action('multiple')" class="actions">&times;</calc-button>
        <calc-button @click="action('exponent')"><span>x<sup>y</sup></span></calc-button>
        <calc-button @click="action('oneonx')">1/x</calc-button>
        <calc-button @click="digit('1')" class="digit">1</calc-button>
        <calc-button @click="digit('2')" class="digit">2</calc-button>
        <calc-button @click="digit('3')" class="digit">3</calc-button>
        <calc-button @click="action('minus')" class="actions">&minus;</calc-button>
        <calc-button @click="action('square')"><span>x<sup>2</sup></span></calc-button>
        <calc-button @click="action('sqrt')">&radic;</calc-button>
        <calc-button @click="action('sign')" class="digit">&#8723;</calc-button>
        <calc-button @click="digit('0')" class="digit">0</calc-button>
        <calc-button @click="digit('.')" class="digit">.</calc-button>
        <calc-button @click="action('plus')">&plus;</calc-button>
        <calc-button @click="action('eval')" class="eval actions">&equals;</calc-button>
    </div>
</template>
<script>
export default {
    data() {
        return {
            screen:     '0',
            errorFlag:  false,
            emptyFlag:  true,
            lastAction: null,
            memory:     null
        }
    },
    methods: {
        digit(num) {
            if (!this.errorFlag) {
                if (num == '.') {
                    if (this.emptyFlag || (this.screen == '0')) {
                        this.screen = '0.';
                        this.emptyFlag = false;
                    } else {
                        if(!this.screen.includes('.')) {
                            this.screen += '.';
                        }
                    }
                } else if (this.emptyFlag || this.screen == '0') {
                    this.screen = num;
                    this.emptyFlag = false;
                } else if (this.screen.toString().replace(/\D/g, '').length < 16){
                    this.screen += num;
                }
            }
        },
        action(button) {
            switch(button) {
                case 'backspace':
                    if (!this.errorFlag && !this.emptyFlag) {
                        if ((this.screen.toString()[0] == '-') && (this.screen.toString().length == 2)) {
                            this.screen = 0;
                        } else if (this.screen.toString().length == 1) {
                            this.screen = 0;
                        } else {
                            this.render(Number(this.screen.toString().slice(0, -1)));
                        }
                    }
                break;
                case 'clear':
                    this.screen = 0;
                    this.errorFlag = false;
                    this.emptyFlag = true;
                break;
                case 'oneonx':
                    if (!this.errorFlag) {
                        this.render(1 / +this.screen);
                        this.emptyFlag = true;
                    }
                break;
                case 'square':
                    if (!this.errorFlag) {
                        this.render(this.screen**2);
                        this.emptyFlag = true;
                    }
                break;
                case 'sqrt':
                    if (!this.errorFlag) {
                        this.render(Math.sqrt(this.screen));
                        this.emptyFlag = true;
                    }
                break;
                case 'sign':
                    if (!this.errorFlag) {
                        this.render(this.screen*-1);
                    }
                break;
                default:
                if (!this.errorFlag) {
                    if ((this.memory !== null) && this.lastAction) {
                        switch (this.lastAction) {
                            case 'divide':
                                this.render(this.memory / +this.screen);
                            break;
                            case 'multiple':
                                this.render(this.memory * +this.screen);
                            break;
                            case 'minus':
                                this.render(this.memory - +this.screen);
                            break;
                            case 'plus':
                                this.render(this.memory + +this.screen);
                            break;
                            case 'exponent':
                                this.render(Math.pow(this.memory, +this.screen));
                            break;
                        }
                    }
                    this.memory = +this.screen;
                    this.lastAction = button;
                    this.emptyFlag = true;
                }
            }
        },
        render(num) {
            if (isNaN(num) || (Math.abs(num) > 9007199254740991)) {
                this.errorFlag = true;
                this.screen = 'error';
            } else {
                if (Math.abs(num) >= 1) {
                    this.screen = parseFloat(num.toPrecision(17));
                } else if (Math.abs(num) >= 0.000001) {
                    this.screen = parseFloat(num.toFixed(16));
                } else {
                    num = num.toFixed(16);

                    while (num.match(/^\d+\.\d*0$/)) {
                        num = num.slice(0, -1);
                    }

                    if (num.match(/^\d+\.$/)) num = num.slice(0, -1);

                    this.screen = num;
                }
            }
        }
    }
}
</script>
<style>
@font-face {
    font-family: 'DS-Digital';
    src: url(~@/fonts/DS-Digital.woff2) format('woff2');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    border: none;
}
html, body {
    width: 100%;
    height: 100%;
}
body {
    position: relative;
    min-height: 635px;
    min-width: 335px;
}
.calc {
    display: grid;
    width: 635px;
    height: 335px;
    background: #5bc9e1;
    padding: 5px;
    gap: 5px;
    grid-template-columns: repeat(6, 100px);
    grid-template-rows: repeat(6, 50px);
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%)
}
.logo {
    grid-column: 1/2;
    grid-row: 1/3;
    background: url(~@/images/128.png) no-repeat center center;
    user-select: none;
}
.screen {
    grid-column: 2/7;
    grid-row: 1/3;
    background: rgba(72, 183, 73, .5);
    color: #314967;
    font: 58px/70px 'DS-Digital';
    text-align: right;
    padding: 10px;
    border: 3px solid #314967;
    border-radius: 8px;
    margin: 5px 0;
}
.button sup {
    font-size: 15px;
}
.button span {
    margin-top: -10px;
    display: inline-block;
    position: relative;
    z-index: -1;
}
</style>
