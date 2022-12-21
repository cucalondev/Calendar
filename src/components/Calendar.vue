<template>
  <div id="calendar" class="px-2">
    <div class="flex justify-between items-center">
      <button @click="prev" class="rounded-md hover:bg-gray-400">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M15.41 7.41L14 6L8 12L14 18L15.41 16.59L11.83 13H20V11H11.83L15.41 7.41Z" fill="currentColor"/>
        </svg>
      </button>
      <button @click="next" class="rounded-md hover:bg-gray-400">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M8.59 16.59L13.17 12L8.59 7.41L10 6L16 12L10 18L8.59 16.59Z" fill="currentColor"/>
        </svg>
      </button>
    </div>
    <h3 class="text-xl text-center">{{currentMonthName}}, {{currentYear}}</h3>
    <div class="calendar-body grid grid-cols-7 gap-1">
      <div v-for="day in daysWeek" :key="day" class="font-bold">{{day}}</div>
      <div v-for="day in firstDay" :key="day" class="font-bold">{{}}</div>
      <div :class="todayDate(date)" class="w-60" v-for="(date,idx) in lastDateOfMonth" v-bind:key="idx">
        <button @click="openModal(date)" class="rounded-md hover:bg-gray-400">{{date}}</button>
      </div>
    </div>
  </div>
    <modal v-if="modalIsOpen" @close="modalIsOpen = false">
      <div class="modal-overlay" @click="$emit('close')">
    <div class="modal-container" @click.stop>
      <div class="modal-header">
        <h3>Add event</h3>
        <button @click="closeModal()">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M19 6.41L17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12L19 6.41Z" fill="currentColor"/>
          </svg>
        </button>
      </div>
      <div class="modal-body">
        <!-- Form to add event goes here -->
        <form @submit.prevent="addEvent">
  <div class="mb-4">
    <label for="event-title" class="block font-bold text-gray-700 mb-2">Title</label>
    <input id="event-title" class="form-input" v-model="eventTitle" required>
  </div>
  <div class="mb-4">
    <label for="event-description" class="block font-bold text-gray-700 mb-2">Description</label>
    <textarea id="event-description" class="form-input" v-model="eventDescription"></textarea>
  </div>
  <div class="mb-4">
    <label for="event-start" class="block font-bold text-gray-700 mb-2">Start time</label>
    <input id="event-start" class="form-input" type="time" v-model="eventStart" required>
  </div>
  <div class="mb-4">
    <label for="event-end" class="block font-bold text-gray-700 mb-2">End time</label>
    <input id="event-end" class="form-input" type="time" v-model="eventEnd" required>
  </div>
  <div class="modal-footer">
    <button class="btn" type="submit">Add event</button>
  </div>
</form>
      </div>
    </div>
    </div>
  </modal>
  <div v-for="event in events" v-bind:key="event.id">
      <h4>{{event.title}}</h4>
      <p>{{event.description}}</p>
      <p>{{event.start}} - {{event.end}}</p>
</div>
</template>
<script setup>
import {ref,computed} from "vue";
const daysWeek = ref(['Mon','Wed','Tue','Thu','Fri','Sat','Sun']);
const currentMonthInNumber = ref(new Date().getMonth()) 
const currentYear = ref(new Date().getFullYear());
const events =ref([]);
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
    return calendarDate===toDay ? "bg-blue-500 text-white" : ""
}
const modalIsOpen = ref(false);

function openModal(date) {
  modalIsOpen.value = true;
}
function closeModal() {
  modalIsOpen.value = false;
}
function addEvent() {
      modalIsOpen.value = false;
    }
const currentMonthName = computed(()=>{
   return new Date(currentYear.value,currentMonthInNumber.value).toLocaleString("default",{month:"long"})});
const lastDateOfMonth = computed(()=>{
    return new Date(currentYear.value,currentMonthInNumber.value+1,0).getDate()});
    const firstDay = computed(() => {
  return new Date(currentYear.value, currentMonthInNumber.value,0).getDay();
});
</script>
<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 10;
}

.modal-container {
  position: relative;
  z-index: 11;
  background-color: #fff;
  max-width: 500px;
  margin: 2rem auto;
  padding: 2rem;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
</style>