<template>
      <div id="calendar">
    <div class="calendar-header">
      <div class="flex justify-between items-center px-2">
        <button @click="prev">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M15.41 7.41L14 6L8 12L14 18L15.41 16.59L11.83 13H20V11H11.83L15.41 7.41Z" fill="currentColor"/>
          </svg>
        </button>
        <button @click="next">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M8.59 16.59L13.17 12L8.59 7.41L10 6L16 12L10 18L8.59 16.59Z" fill="currentColor"/>
          </svg>
        </button>
      </div>
    </div>
      <h3 class="text-xl text-center">{{currentMonthName}}, {{currentYear}}</h3>
        <div class="calendar-body grid grid-cols-7 gap-1 px-2">
      <div v-for="day in daysWeek" :key="day" class=" font-bold">{{day}}</div>
      <div v-for="day in firstDay" :key="day" class=" font-bold">{{}}</div>
      <div :class="todayDate(date)" class="w-60" v-for="(date,idx) in lastDateOfMonth" v-bind:key="idx">{{date}}
    </div>
    </div>
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
    const firstDay = computed(() => {
  return new Date(currentYear.value, currentMonthInNumber.value, 1).getDay();
});

</script>
<style scoped>
</style>