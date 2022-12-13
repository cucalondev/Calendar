<template>
  <Navbar/>
  <div class="flex justify-center items-center h-screen">
    <div class="calendar">
  <Calendar/>
</div>
</div>
</template>
<script setup>
import Calendar from '@/components/Calendar.vue';
import Navbar from '@/components/Navbar.vue';
</script>

<style scoped>

.calendar{
  background:orange;
  color:black;
}

</style>
