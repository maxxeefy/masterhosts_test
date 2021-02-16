<template>
    <div class="card">
        <h1>{{vehicles.brand}} {{vehicles.name}}</h1>
        <img :src="vehicles.image" class="card__image">
        <div class="card__description">
            <VehicleProp 
                :key="propertie.name"
                v-for="propertie of vehiclesProp"
                v-bind:propertie="propertie"
            />
            <select class="vehicle__color">
                <option disabled selected>Выберите цвет</option>
                <VehicleColors 
                    :key="color"
                    v-for="color of vehiclesColors"
                    v-bind:color="color"
                />
            </select>
            <span class="vehicle__price">Цена: {{vehicles.unitPrice}}</span>
            <button v-on:click="buy = true" class="vehicle__buy">Купить</button>
        </div>
        <div v-if="buy" class="vehicle__order">
                <OrderForm 
                    v-bind:vehiclesOrderParameters="vehiclesOrderParameters"
                    v-bind:vehiclesColors="vehiclesColors"
                    v-bind:unitCode="unitCode"
                    v-bind:unitPrice="vehicles.unitPrice"
                    @submitHandler="submitHandler"
                    @cancelForm="cancelForm"
                />
        </div>
    </div>
</template>

<script>
import VehicleProp from './VehicleProp'
import VehicleColors from './VehicleColors'
import OrderForm from './OrderForm'

export default {
    props: {
        vehicles: {
            type: Object,
            required: true
        },
        vehiclesProp: {
            type: Array,
            required: true
        },
        vehiclesColors: {
            type: Array,
            required: true
        },
        vehiclesOrderParameters: {
            type: Array,
            required: true
        },
        unitCode: {
            type: String,
            required: true
        }
    },
    components: {
        VehicleProp,
        VehicleColors,
        OrderForm
    },
    data() {
        return {
            buy: false,
            orderName: '',
            orderEmail: ''
        }
    },
    methods: {
        submitHandler(data) {
            this.buy = data
        },
        cancelForm(data) {
            this.buy = data
        }
    }
}
</script>

<style>
.card__image {
    height: 30%;
    width: 30%;
    float: left;
}
.card__description {
    display: flex;
    flex-direction: column;
}
.vehicle__color {
    align-self: center;
    width: 92%;
    margin: 20px 0;
}
.vehicle__price {
    text-align: left;
    margin: 0 0 20px 40px;
    font-weight: bold;
    font-size: 1.5rem;
}
.vehicle__buy {
    width: 92%;
    height: 30px;
    color: white;
    background: red;
    border: none;
    font-weight: bold;
    font-size: 1.1rem;
    margin: 0 auto;
}
.vehicle__buy:hover {
    box-shadow: 0 0 10px rgba(0,0,0,0.5);
}
.vehicle__buy:focus {
    outline: none;
}
.vehicle__order {
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    background: white;
    max-width: 800px;
    min-height: 500px;
    margin-left: auto;
    margin-right: auto;
    left: 0;
    right: 0;
    top: 20%;
    z-index: 10;
    border: 1px solid black;
    border-radius: 10px;
    box-shadow: 0 0 15px rgba(0,0,1,0.8);
}

@media screen and (max-width: 480px) {
    .card__image {
        height: 50%;
        width: 50%;
        float: none;
    }
    .vehicle__order {
        top: 45%;
    }
    .vehicle__buy {
        margin: 0 auto 50px;
    }
}
@media screen and (max-device-width: 320px) {
    .vehicle__order {
        display: flex;
        width: 320px;
        margin: 0 auto;
    }
}
</style>
