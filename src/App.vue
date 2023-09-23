<script setup>
import panel from './components/panel.vue'
</script>

<template>
  <header>
    <div class="frame">
      <panel letter="あ" status="hit" />
      <panel letter="い" status="blow"/>
      <panel letter="う" status="miss"/>
      <div class="square"></div>
    </div>
  </header>

  <main>
  </main>
</template>

<style scoped>
.frame {
  
}
</style>
