<template>
<div class="container">
    <button @click="show=!show" type="button" class="text-gray-900 bg-gradient-to-r from-lime-200 via-lime-400 to-lime-500 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-lime-300 dark:focus:ring-lime-800 shadow-lg shadow-lime-500/50 dark:shadow-lg dark:shadow-lime-800/80 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2">Mes</button>
    <section v-if="show">
    <h1>{{currentMonthName}} {{currentYear}}</h1>
    <div class="days flex">
    <p class="w-40" v-for="(day,index) in daysWeek" v-bind:key="index">
    {{day}}
    </p>
    </div>
    </section>
    <section v-if="show">
    <table class="dates flex flex-wrap">
        <tr class="w-40" v-for="day in firstDay" v-bind:key="day">{{}}</tr>
        <tr :class="todayDate(date)" class="w-40" v-for="(date,idx) in lastDateOfMonth" v-bind:key="idx">{{date}}
        </tr>
    </table>
    </section>
    <section class="flex justify-between">
        <button @click="prev" class="bg-transparent hover:bg-blue-500 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent rounded">
        Prev
        </button>
        <button @click="next" class="bg-transparent hover:bg-blue-500 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent rounded">
        Next
        </button>
    </section>
</div>
</template>
<script setup>
import {ref,computed} from "vue";
const daysWeek = ref(['Mon','Wed','Tue','Thu','Fri','Sat','Sun']);
const currentMonthInNumber = ref(new Date().getMonth()) 
const currentYear = ref(new Date().getFullYear());
const show = ref(false);
function prev(){
    if(currentMonthInNumber.value===0){
        currentYear.value--;
        currentMonthInNumber.value=11;
    }
    else{
        currentMonthInNumber.value--
    }
}
function next(){
    if(currentMonthInNumber.value===11){
        currentYear.value++;
        currentMonthInNumber.value=0;
    }
    else{
        currentMonthInNumber.value++
    }
}
function todayDate(date){
    let calendarDate = new Date(currentYear.value,currentMonthInNumber.value,date).toDateString();
    let toDay = new Date().toDateString();
    return calendarDate===toDay ? "text-blue-400" : ""
}
const currentMonthName = computed(()=>{
   return new Date(currentYear.value,currentMonthInNumber.value).toLocaleString("default",{month:"long"})});
const lastDateOfMonth = computed(()=>{
    return new Date(currentYear.value,currentMonthInNumber.value+1,0).getDate()});
const firstDay = computed(()=>{
    return new Date(currentYear.value,currentMonthInNumber.value+1,0).getDate()});
</script>
<style scoped>
</style>