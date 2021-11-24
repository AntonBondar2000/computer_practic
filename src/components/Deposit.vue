<template>
    <main>
        <div class="calculate">
            <div class="errors" v-if="errors.length != 0">
                <p v-for="(item, index) in errors" :key=index>
                    {{item}}
                </p>
            </div>
            <div class="block-input">
                <input type="text" id="input_amount" v-model="amount" @blur="inputBlur" @focus="inputFocus" @keyup="inputKeyup">
                <p>Сумма</p>
            </div>
            
            <div class="calculate_date">
                <div class="block-input">
                    <input type="text" id="input_period" v-model="period" @keyup="periodKeyup">
                    <p>Срок</p>
                </div>
                <select id="input_type_period" v-model="period_type">
                    <option value="">Укажите период</option>
                    <option value=1>Месяц</option>
                    <option value=12>Год</option>
                    <option value=3>Квартал</option>
                </select>
            </div>
            <div class="block-input">
                <input type="text" id="input_rate" v-model="rate" @keyup="rateKeyup">
                <p>Процентная ставка</p>
            </div>
        </div>
        <div class="result">
            <p class="result__all">
                Начисленные проценты: {{ sum_all }}
            </p>
            <button 
                class="calculate_button" 
                @click="considerCredit"
            >
                Рассчитать вклад
            </button>
        </div>
    </main>
</template>

<script>
export default {
    name: 'Deposit',
    data() {
        return {
            amount: null,
            period: null,
            period_type: "",
            rate: null,
            sum_all: 0,
            every_month: 0,
            overpayment: 0,
            errors: []
        }
    },
    methods:{
        validateInput(){
            if (!this.amount || !this.amount.match(/^[0-9.]*$/))
                this.errors.push("Сумма должна быть числом")

            if (!this.period || !this.period_type || this.period * this.period_type > 1200)
                this.errors.push("Срок кредита не более 100 лет")

            if (!this.rate || !this.rate.match(/^[0-9.]*$/))
                this.errors.push("Ставка должна быть числом")
        },
        displayMoney(x){
            return parseInt(x * 100) / 100
        },
        inputBlur(){
            this.amount = this.amount.replace(/[^\d]/g, '').replace(/\B(?=(?:\d{3})+(?!\d))/g, ' ')
        },
        inputFocus(){
            this.amount = this.amount.replace(/\s/g, '')
        },
        inputKeyup(){
            this.amount = this.amount.replace(/[^\d]/g, '')
        },
        rateKeyup(){
            this.rate = this.rate.replace(/[^\d]/g, '')
        },
        periodKeyup(){
            this.period = this.period.replace(/[^\d]/g, '')
        },
        considerCredit(){
            this.errors = []
            this.inputFocus()
            this.validateInput()
            if (this.errors.length == 0){
                let rate = this.rate / 100 / 12 
                const tmp = Math.pow(1 + rate, this.period * this.period_type)
                const res = this.amount * tmp 
                this.sum_all = this.displayMoney(
                    res - this.amount
                )
                this.every_month = this.displayMoney(res)
                this.overpayment = this.displayMoney(
                    res * this.period * this.period_type - this.amount
                )
            }
            this.inputBlur()
        },
    }
}
</script>

<style scoped lang="scss">
main{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 7px;
    .calculate{
        height: 60%;
        width: 45%;
        padding-left: 100px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        .block-input{
            width: 100%;
            height: 96px;
            position: relative;
            input{
                background-color: rgba(#000, .7);
                color: white;
                font-size: 18px;
                height: 100%;
                width: 100%;
                border-radius: 25px;
                padding: 10px 21px;
                border: none;
                outline: none;
            }
            p{
                position: absolute;
                color: white;
                font-size: 18px;
                top: 7px;
                left: 21px;
            }
        }
        .calculate_date{
            display: flex;
            justify-content: space-between;
            width: 100%;
            select{
                background-color: rgba(#000, .7);
                color: white;
                font-size: 18px;
                height: 96px;
                width: 100%;
                border-radius: 25px;
                padding: 10px 21px;
                border: none;
                outline: none;
                cursor: pointer;
                margin-left: 20px;
                appearance: none;
            }
        }
    }
    .result{
        height: 60%;
        width: 45%;
        padding: 66px 50px 50px 50px;
        border: 1px dashed rgba(#000, 0.8);
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        p{
            font-size: 21px;
            color: black;
            
        }
        button{
            cursor: pointer;
            background-color: rgba(#000, .7);
            color: white;
            font-size: 18px;
            height: 96px;
            border-radius: 25px;
            width: 80%;
            margin: 0 auto;
            padding: 10px 21px;
            border: none;
            outline: none;
        }
    }
}
</style>
