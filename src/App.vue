<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { ref, onMounted } from 'vue'
import { ethers } from 'ethers'
import Token from './NEELToken.json'

const name = ref('')
const symbol = ref('')
const currentAddress = ref('')
const address = ref('')
const amount = ref(0)
const balance = ref(0)

const provider = new ethers.providers.Web3Provider(window.ethereum)
const contract = new ethers.Contract(Token.address, Token.abi, provider.getSigner())

window.ethereum.on('accountsChanged', init)

onMounted(init)

async function init(){
  name.value = await contract.name()
  symbol.value = await contract.symbol()
  balance.value = await contract.balanceOf(provider.getSigner().getAddress())
  currentAddress.value = await provider.getSigner().getAddress()
}

async function call(){  
  await contract.transfer(address.value, amount.value)
}

</script>

<template>
  <h1>{{ name }}</h1>
  <h3>Account: {{ currentAddress }}</h3>
  <h3>Balance: {{ balance }} {{ symbol }}</h3>

  <h2>Transfer</h2>
  <input type="text" placeholder="address" v-model="address" />
  <input type="number" placeholder="amount" v-model="amount">
  <button @click="call">Send tokens</button>

</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
