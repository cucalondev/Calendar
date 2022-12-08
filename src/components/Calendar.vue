<template>
<div class="container">
    <section>
        <h1 class="text-center">Calendar</h1>
    </section>
    <h1>{{currentMonth}} of {{currentYear}}</h1>
    <section>
    <div class="days flex">
    <p class="w-40" v-for="(day,index) in days" v-bind:key="index">
    {{day}}
    </p>
    </div>
    </section>
    <section>
    <table class="dates flex flex-wrap">
        <tr class="w-40" v-for="(date,idx) in 30" v-bind:key="idx">{{date}}</tr>
    </table>
    </section>
</div>
</template>
<script setup>
import {ref} from "vue";
const days = ref(['L','M','X','J','V','S','D']);
const currentMonth=ref(new Date().toLocaleString("default",{month:"long"}));
const currentYear = ref(new Date().getFullYear());
</script>
<style scoped>
table tr,td,th{
    border:0.5px solid;
    border-color:blue;
}
</style>