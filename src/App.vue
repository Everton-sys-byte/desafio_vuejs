<template>
  <!-- dolar serve para passar o objeto do evento como $event -->
  <div @click="getPosition($event)" class="page">
    <div v-for="position in positions" :key="position.index" 
    class="square" 
    :style="{
      top: position.positionY, left: position.positionX
    }"></div>

    <div class="buttons">
        <button @click="goBack($event)" class="undo" :disabled="positions.length === 0">Voltar</button>
        <button @click="goForward($event)" class="redo" :disabled='removedPositions.length === 0'>Refazer</button>
    </div>
  </div>
</template>

<script setup>
import {ref} from 'vue'  
  let positions = ref([])
  let removedPositions = ref([])

  const getPosition = (event) => {
    positions.value.push(
      {
        positionX: event.clientX + "px",
        positionY: event.clientY + "px"
      }
    )
  }

  const goBack = (event) => {
    event.stopPropagation()
    removedPositions.value.push(positions.value[positions.value.length - 1])
    positions.value.splice(positions.value.length - 1)
  }

  const goForward = (event) => {
    event.stopPropagation()
    positions.value.push(removedPositions.value[removedPositions.value.length - 1])
    removedPositions.value.splice(removedPositions.value.length - 1)
  }

</script>

<style>
*{
  margin: 0px;
  padding: 0px;
}

.page {
  width: 100vw;
  height: 100vh;
}

.square {
  width: 10px;
  height: 10px;
  position: absolute;
  background: red;
}

button {
  z-index: 10;
  padding: 10px;
  margin-top: 5px;
  margin-left: 5px;
  cursor: pointer;
}

</style>
