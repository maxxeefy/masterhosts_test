<template>
    <div class="order">
        <form
            @submit.prevent="submitHandler"
            method="post"
        >
        <div class="order__name">
            {{vehiclesOrderParameters[0].name}}: <input type="text" id="name" v-model="name">
            <label for="name" v-if="this.nameMistake == true || this.vehiclesOrderParameters[0].necessary && this.vehiclesOrderParameters[0].necessary == 1">Обязательно для заполнения</label>
        </div>
        <div class="order__email">
            {{vehiclesOrderParameters[1].name}}: <input type="text" id="email" v-model="email">
            <label for="email" v-if="this.emailMistake == true || vehiclesOrderParameters[1].necessary && vehiclesOrderParameters[1].necessary == 1">Обязательно для заполнения</label>
        </div>
        <div class="order__address">
            {{vehiclesOrderParameters[2].name}}: <input type="text" id="address" v-model="address">
        </div>
        <div class="order__delivery">
            {{vehiclesOrderParameters[3].name}}: 
            <select v-on:change="changeTime">
                <TimeOfDelivery 
                    :key="time"
                    v-for="time of vehiclesOrderParameters[3].value"
                    v-bind:time="time"
                />
            </select>
        </div>
        <div class="order__color">
            {{vehiclesOrderParameters[4].name}}: 
            <select v-on:change="changeColor">
                <VehicleColors 
                    :key="color"
                    v-for="color of vehiclesColors"
                    v-bind:color="color"
                />
            </select>
        </div>
        <div class="order__count">
            {{vehiclesOrderParameters[5].name}}: 
            <div class="carousel">
                <button @click.prevent="showPrev">-</button>
                <VueSlickCarousel class="slider" :arrows="false" v-bind="settings" ref="carousel">
                    <div class="slide">1</div>
                    <div class="slide">2</div>
                    <div class="slide">3</div>
                    <div class="slide">4</div>
                    <div class="slide">5</div>
                    <div class="slide">6</div>
                    <div class="slide">7</div>
                    <div class="slide">8</div>
                    <div class="slide">9</div>
                    <div class="slide">10</div>
                </VueSlickCarousel>
                <button @click.prevent="showNext">+</button>
            </div>
        </div>
        <button type="submit" @click="submitForm" class="submitButton">Купить</button>
        <button @click="cancelForm" class="cancelButton">Отмена</button>
        </form>
    </div>
</template>

<script>
import TimeOfDelivery from './TimeOfDelivery'
import VehicleColors from './VehicleColors'

import VueSlickCarousel from 'vue-slick-carousel'
import 'vue-slick-carousel/dist/vue-slick-carousel.css'
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'

export default {
    props: {
        vehiclesOrderParameters: {
            type: Array,
            required: true
        },
        vehiclesColors: {
            type: Array,
            required: true
        },
        unitCode: {
            type: String,
            required: true
        },
        unitPrice: {
            type: String,
            required: true
        }
    },
    data () {
        return {
            name: "",
            email: "",
            address: "",
            vehicleValue: 1,
            deliveryTime: "Утро",
            vehicleColor: "Красный",
            nameMistake: false,
            emailMistake: false,
            orderBuy: false,
            settings: {
                "edgeFriction": 0.35,
                "infinite": true,
                "speed": 100,
                "slidesToShow": 1,
                "slidesToScroll": 1,
                "draggable": false
            }
        }
    },
    components: {
        TimeOfDelivery,
        VehicleColors,
        VueSlickCarousel
    },
    methods: {
        submitHandler() {
            if(!this.nameMistake && !this.emailMistake) {
                alert("Спасибо за покупку!")
                this.$emit('submitHandler', false)
            }
        },
        submitForm() {
            this.checkName()
            this.checkEmail()
            if(!this.nameMistake && !this.emailMistake) {
                const order = {
                    unitCode: this.unitCode,
                    unitPrice: this.unitPrice,
                    parameters:[
                        {name:this.name, email:this.email, address:this.address},
                        {name:"Интервал доставки", value:this.deliveryTime},
                        {name:"Цвет", value:this.vehicleColor},
                        {name:"Количество", value:this.vehicleValue}]
                }
                const dataPostJSON = JSON.stringify(order);
                console.log(dataPostJSON)
                this.name=""
                this.email=""
                this.address=""  
            }    
        },
        checkName() {
            this.nameMistake = false
            const orderName = document.getElementById('name')
            const nameArr = this.name.split(' ')
            const NAME_REGEXP = /^[a-zA-ZА-ЯЁа-яё -]+$/i;
            function validateName(value) {
                return NAME_REGEXP.test(value);
            }
            if(nameArr.length < 2 && !this.nameMistake) {
               document.querySelector("label[for=name]").innerHTML = "Имя должно содержать два слова"
               orderName.style.borderColor="red"
               this.nameMistake = true
            } 
            else {
                for(let i=0; i < nameArr.length; i++) {
                    if(nameArr[i].length < 2 && !this.nameMistake) {
                        document.querySelector("label[for=name]").innerHTML = "Слова должны быть не короче двух символов"
                        orderName.style.borderColor="red"
                        this.nameMistake = true
                    }
                }
            }
            if (validateName(this.name) && !this.nameMistake) {
                orderName.style.borderColor="grey"
                document.querySelector("label[for=name]").innerHTML = ""
            } else if (!validateName(this.name) && !this.nameMistake){
                document.querySelector("label[for=name]").innerHTML = "Имя должно быть на русском или английском языке и содержать пробел или дефис"
                orderName.style.borderColor="red"
                this.nameMistake = true
            }
        },
        checkEmail() {
            this.emailMistake = false
            const EMAIL_REGEXP = /^(([^<>()[\].,;:\s@"]+(\.[^<>()[\].,;:\s@"]+)*)|(".+"))@(([^<>()[\].,;:\s@"]+\.)+[^<>()[\].,;:\s@"]{2,})$/iu;
            function validateEmail(value) {
                return EMAIL_REGEXP.test(value);
            }
            const orderEmail = document.getElementById('email')

            if (validateEmail(this.email) && !this.emailMistake) {
                document.querySelector("label[for=email]").innerHTML = ""
                orderEmail.style.borderColor="grey"
            } else {
                document.querySelector("label[for=email]").innerHTML = "Некорректный email"
                orderEmail.style.borderColor="red"
                this.emailMistake = true
            }
        },
        changeTime: function(e){
            this.deliveryTime = e.target.value
        },
        changeColor: function(e){
            this.vehicleColor = e.target.value
        },
        showNext() {
            this.$refs.carousel.next()
            this.vehicleValue++
                if (this.vehicleValue == 11) {
                    this.vehicleValue = 1
                }
        },
        showPrev() {
            this.$refs.carousel.prev()
            this.vehicleValue--
                if (this.vehicleValue == 0) {
                    this.vehicleValue = 10
                }
        },
        cancelForm() {
            this.$emit('cancelForm', false)
        }
    }
}
</script>

<style>
.order {
    display: flex;
    width: 60%;
}
.carousel {
    width: 100px;
    margin: 0 auto;
}
.slide {
    background: cornflowerblue;
    color: white;
}
form {
    width: 100%;
}
.order__name,
.order__email,
.order__address,
.order__delivery,
.order__color {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
}
.order__delivery,
.order__color {
    align-items: center;
}
.order__delivery,
.order__color,
.order__count {
    margin-bottom: 10px;
}
.order__delivery select,
.order__color select {
    width: 50%;
    margin-left: 10px;
    font-size: 0.9rem;
}
.order__name label,
.order__email label {
    font-size: 0.7rem;
    color: darkred;
}
.order__name input,
.order__email input,
.order__address input {
    padding-left: 10px;
    width: 85%;
    align-self: center;
    border-radius: 10px;
}
.carousel {
    display: flex;
    flex-direction: column;
    margin: 5px auto;
}
.carousel button {
    margin: 5px;
    border: 1px solid black;
    border-radius: 10px;
}
.carousel button:hover {
    box-shadow: 0 0 3px rgba(0,0,0,0.8);
}
.carousel button:active {
    border-color: red;
}
.carousel button:focus,
.submitButton:focus,
.cancelButton:focus,
.order__name input:focus,
.order__email input:focus,
.order__address input:focus {
    outline: none;
}
.submitButton:hover,
.cancelButton:hover {
    box-shadow: 0 0 3px rgba(0,0,0,0.8);
}
.submitButton {
    width: 80%;
    height: 30px;
    color: white;
    background: red;
    border: none;
    font-weight: bold;
    font-size: 1.1rem;
    margin-bottom: 10px;
}
.cancelButton {
    width: 80%;
    height: 30px;
    color: white;
    background: cornflowerblue;
    border: none;
    font-weight: bold;
    font-size: 1.1rem;
    margin-bottom: 10px;
}
</style>