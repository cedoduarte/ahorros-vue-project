<template>
  <h1>Ahorros -> $ {{ ahorros }}</h1>
  <button @click="deposita(100)">Depositar $100</button><br /><br />
  <button @click="retira(100)" :disabled="deshabilitaBoton">Retirar $100</button>

  <div>
    <h4>Movimientos:</h4>
    <ul>
      <li v-for="(movimiento, index) in reverseArray" :key="index">
        <span v-if="movimiento.monto > 0">Depósito</span>
        <span v-else>Retiro</span>
        | Monto: {{ movimiento.monto }} | Saldo: {{ movimiento.saldoActual }}
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";

interface IMovimiento {
  monto: number;
  saldoActual: number;
}

const ahorros = ref<number>(0);
const movimientos = ref<IMovimiento[]>([]);

const updateMovimientos = (cantidad: number) => {
  movimientos.value = [...movimientos.value, {
    monto: cantidad,
    saldoActual: ahorros.value
  }];
}

const deposita = (cantidad: number) => {
  ahorros.value += cantidad;
  updateMovimientos(+cantidad);
}

const retira = (cantidad: number) => {
  ahorros.value -= cantidad;
  updateMovimientos(-cantidad);
}

const deshabilitaBoton = computed(() => {
  return ahorros.value <= 0;
});

const reverseArray = computed(() => {
  return [...movimientos.value].reverse();
});
</script>

<style scoped>
</style>
