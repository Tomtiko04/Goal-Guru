<template>
  <header>
    <h1 class="logo">TODO</h1>
    <div @click="handleMoon">
      <MoonIcon v-if="theme" fillColor="hsl(0, 0%, 98%)" :size="24" :style="{cursor: 'pointer'}"/>
      <SunIcon v-else fillColor="hsl(0, 0%, 98%)" :size="24" :style="{cursor: 'pointer'}"/>
    </div>
  </header>
</template>


<script setup>
import { inject } from 'vue';
import MoonIcon from 'vue-material-design-icons/MoonWaningCrescent.vue';
import SunIcon from 'vue-material-design-icons/WhiteBalanceSunny.vue';

const theme = inject('theme');

function handleMoon() {
  theme.value = !theme.value;
}

</script>


<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.logo{
    color: hsl(0, 0%, 98%);
    letter-spacing: .7rem;
    font-weight: 700;
    font-size: 1.7rem;
}
</style>
