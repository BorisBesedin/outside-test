<template>
    <form class="popup" @submit.prevent>
        <button class="popup__close" @click="$emit('close-popup')"></button>
        <h2 class="popup__title">Налоговый вычет</h2>
        <p class="popup__text">Используйте налоговый вычет чтобы погасить ипотеку досрочно. 
            Размер налогового вычета составляет не более 13% от своего 
            официального годового дохода.</p>
        <div class="popup__salary">
            <label for="salary">Ваша зарплата в месяц</label>
            <input type="text" id="salary" name="salary" @change="format" placeholder="Ваша зарплата" autocomplete="off">
            <button class="popup__salary-btn" @click="setTaxesPerYears">Расчитать</button>
        </div>

        <div class="popup__taxes" v-if="taxesList.length">
            <p class="popup__taxes-title">Итого можете внести в качестве досрочных:</p>
            <div class="popup__checkbox" v-for="year in taxesList" :key="year.id">
                <input type="checkbox" v-bind:id="year.id">
                <label v-bind:for="year.id">{{ year.sum }} рублей <span> {{ year.text }} </span></label>
            </div>
        </div>

        <div class="popup__options options">
            <p class="options__text">Что уменьшаем?</p>
            <ul class="options__list">
                <li class="options__item">
                    <input class="options__radio" type="radio" id="payment" name="option-decrease" value="payment" checked>
                    <label class="options__label" for="payment">Платеж</label>
                </li>
                <li class="options__item">
                    <input class="options__radio" type="radio" id="period" name="option-decrease" value="period">
                    <label class="options__label" for="period">Срок</label>
                </li>
            </ul>
        </div>
        <button type="submit" class="popup__button">Добавить</button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            flatCost: 2000000,
            salary: 0,
            taxesList: []
        }
    },
    computed: {
        maxTaxes: function() {
            const max = 260000
            return (this.flatCost * 0.13) <= max ? this.flatCost * 0.13 : max
        },
        taxPerYear: function() {
            return Math.floor((this.salary * 12) * 0.13)
        }
    },
    methods: {
        setTaxesPerYears() {
            if (this.salary.length >=4) {
                this.taxesList = []
                const years = Math.floor(this.maxTaxes / this.taxPerYear)
                for(let i = 0; i < years; i++) {
                    this.taxesList.push({
                        id: `year${i}`,
                        text: i !== 1 ? `в ${i + 1}-й год`: `во ${i + 1}-й год`,
                        sum: this.taxPerYear
                    })
                }
                this.taxesList.push({
                    id: `year${years.length}`,
                    text: years !== 1 ? `в ${years + 1}-й год`: `во ${years + 1}-й год`,
                    sum: this.maxTaxes - (this.taxPerYear * years)
                })
            } else {
                alert('Сильно маленькая зарплата. Считать противно.')
            }
            
        },
        format() {
            const salary = document.querySelector('#salary')
            this.salary = salary.value
            salary.value = new Intl.NumberFormat('ru-RU', {
                currency: 'rub',
                style: 'currency'
            }).format(salary.value);            
        }
    }
    
}
</script>

<style scoped>
.popup {
    margin-top: 120px;
    margin-bottom: 40px;
    position: relative;
    max-width: 552px;
    min-width: 320px;
    background-color: #ffffff;
    padding: 32px;
    border-radius: 30px;    
}

p, label {
    font-size: 14px;
    line-height: 24px;
}

.popup__close {
    position: absolute;
    top: 27px;
    right: 27px;
    width: 18px;
    height: 18px;
    background-color: transparent;
    border: none;
    cursor: pointer;
}

.popup__close::after,
.popup__close::before {
    content: "";
    position: absolute;
    top: 8px;
    left: -3px;
    width: 24px;
    height: 3px;
    background-color: #EA0029;
    border-radius: 30px;
}

.popup__close::after {
    transform: rotate(-45deg);
}

.popup__close::before {
    transform: rotate(45deg);
}

.popup__title {
    font-size: 28px;
    line-height: 40px;
    text-align: left;
}

.popup__text {
    margin-top: 16px;
    color: #808080;
    text-align: left;
}

.popup__salary {
    margin-top: 24px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}
label {    
    color: #000000;
    font-weight: bold;
}
input {
    margin-top: 8px;
    width: 100%;
    padding: 8px 10px;
    border: 1px solid #DFE3E6;
    border-radius: 3px;
    color: #000000;
    font-size: 14px;
    line-height: 24px;
    outline: none;
}

input:hover {
    border-color: #000000;
}

input:focus {
    border-color: #DFE3E6;
}

.popup__salary-btn {
    margin-top: 8px;
    font-size: 14px;
    line-height: 24px;
    font-weight: bold;
    color: #EA0029;
    border: none;
    background-color: transparent;
    cursor: pointer;
    outline: none;
}

.popup__salary-btn:hover {
    color: #F53A31;
}

.popup__button {
    width: 100%;
    margin-top: 40px;
    color: #ffffff;
    padding: 16px 32px;
    background: linear-gradient(to right,#FF5E56, #DC3131);
    border-radius: 5px;
    cursor: pointer;
    outline: none;
    font-size: 16px;
    line-height: 24px;
    font-weight: bold;
    border: none;
}

.popup__button:hover {
    background: #EA0029;
}

.popup__taxes {
    margin-top: 16px;
}

.popup__taxes-title {
    font-weight: bold;
    text-align: left;
}

.popup__checkbox {
    display: flex;
    align-items: center;
    border-bottom: 1px solid #DFE3E6;
    padding: 16px 0;
}

.popup__checkbox input {
    display: none;    
}
.popup__checkbox label {
    position: relative;
    padding-left: 30px;
    font-weight: normal;
    cursor: pointer;
}

.popup__checkbox span {
    font-size: 16px;
    line-height: 24px;
    font-weight: normal;
    color: #808080;
}

.popup__checkbox label::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 20px;
    height: 20px;
    border: 1px solid #DFE3E6;
    border-radius: 6px;
}

.popup__checkbox label:hover::before {
    border: 1px solid #000000;
}

.popup__checkbox input:checked + label::before {
    background: linear-gradient(to right,#FF5E56, #DC3131);
}

.popup__checkbox input:checked + label::after {
    content: "";
    position: absolute;
    top: 6px;
    left: 5px;
    width: 10px;
    height: 4px;
    border: 3px solid #ffffff;
    transform: rotate(-45deg);
    border-top: none;
    border-right: none;
}

.options {
    margin-top: 24px;
    display: flex;
    align-items: center;
}

.options__list {
    margin-left: 32px;
    display: flex;
}

.options__item {
    margin-right: 16px;
}

.options__radio {
    display: none;
}

.options__label {
    display: block;
    padding: 6px 12px;
    font-weight: normal;
    background-color: #EEF0F2;
    border-radius: 50px;
    cursor: pointer;
}

.options__label:hover {
    background-color: #DFE3E6;
}

.options__radio:checked + label {
    background: linear-gradient(to right,#FF5E56, #DC3131);
    color: #ffffff;
}

.options__text {
    font-weight: bold;
}

@media (max-width: 600px) {
    .popup {
        max-width: none;
        flex-grow: 1;
        margin-top: 0;
        margin-bottom: 0;
        padding-bottom: 60px;
        width: 100%;
        border-radius: 0;
    }

    .options {
        flex-direction: column;
        align-items: flex-start;
    }

    .options__list {
        margin-left: 0;
        margin-top: 24px;
    }
}
</style>