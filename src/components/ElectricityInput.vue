<template>
    <div id="electricityInput"
        class="w-full pt-20 md:pt-48 px-8 flex-col justify-around items-center text-2xl text-main">
        <div id="doknowcost" class="flex-col justify-around">
            <p class="w-72 text-center">1. Wissen Sie wie hoch ihre Stromkosten im letzten Jahr waren?</p>
            <form class="values w-48 flex justify-around mt-4 mb-12">
                <input type="radio" value="true" name="doknowcost" v-model="doKnowCost" /><label>Ja</label>
                <input type="radio" value="false" name="doknowcost" v-model="doKnowCost"
                    @change="handleFulfilled(false)" /><label>Nein</label>
            </form>
        </div>
        <div v-if="doKnowCost == 'true'" id="money" class="flex-col items-center">
            <p>2. Wie hoch waren ihre Stromkosten im letzten Jahr?</p>
            <div class="inputLine flex">
                <input type="text" placeholder="720" v-model="electricityBill" @change="handleFulfilled(true)"
                    class="w-64 border-b-2 md:w-76 xl:w-90 pt-2 cursor-pointer text-main px-2" />
                <p class="ml-2 mt-1">€</p>
            </div>
        </div>
        <div v-if="doKnowCost == 'false'" id="doKnowAmount" class="flex-col justify-around">
            <p class="w-72 text-center">2. Wissen Sie wie viele kWh Strom ihr Haushalt im letzten Jahr verbraucht hat?
            </p>
            <form class="values w-48 flex justify-around mt-4 mb-12">
                <input type="radio" value="true" name="doKnowAmount" v-model="doKnowAmount"
                    @change="handleFulfilled(false)" /><label>Ja</label>
                <input type="radio" value="false" name="doKnowAmount" v-model="doKnowAmount"
                    @change="handleFulfilled(true)" /><label>Nein</label>
            </form>
        </div>
        <div v-if="doKnowAmount == 'true' && doKnowCost == 'false'" id="amount" class="flex-col items-center">
            <p>3. Wie viele kWh hat ihr Haushalt im letzten Jahr verbraucht?</p>
            <div class="inputLine flex">
                <input type="text" placeholder="4000" v-model="electricityAmount" @change="handleFulfilled(true)"
                    class="w-64 border-b-2 md:w-76 xl:w-90 pt-2 cursor-pointer text-main px-2" />
                <p class="ml-2 mt-1">kWh</p>
            </div>
        </div>
        <div v-if="doKnowAmount == 'false' && doKnowCost == 'false'" id="people" class="flex-col items-center">
            <p>3. Wie viele Personen wohnen in ihrem Haushalt?</p>
            <input type="range" v-model="peopleInHouse" min="1" max="20">
            <p>{{peopleInHouse}}</p>
        </div>


    </div>
</template>
<script>
    import store from "../store";
    import calculations from "../data/calculations";
    export default {
        data() {
            return {
                doKnowCost: null,
                doKnowAmount: null,
                electricityBill: 0,
                electricityAmount: 0,
                peopleInHouse: 4
            }
        },
        methods: {
            handleFulfilled(value) {
                store.commit("handleFulfilled", value)
                if (value == true) {
                    var data = {
                        id: "electricity",
                        doKnowCost: this.doKnowCost,
                        doKnowAmount: this.doKnowAmount,
                        electricityBill: this.electricityBill,
                        electricityAmount: this.electricityAmount,
                        peopleInHouse: this.peopleInHouse
                    }
                    calculations.calElectricity();
                    store.commit("pushData", data)
                }
            }
        }
    }
</script>