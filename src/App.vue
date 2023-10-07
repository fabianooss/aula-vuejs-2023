<script setup>
import { computed, ref } from 'vue'

const titulo = ref('Meu título dinâmico')
const quantidadeLetras = ref()
const isTextoInvertido = ref(false)
const nomes = ref(['Malu','Bia','Thom'])
const carros = ref([
  {nome: "Fusca", valor: 1000, cor: "Azul"},
  {nome: "Variante", valor: 2000, cor:"Amarela"},
  {nome: "Brasilia", valor: 3000, cor:"Branca"},
])
const filtro = ref('')
const nomeParaAdicionar = ref('')
const indexEditar = ref(-1)

const nomesFiltrados = computed(() => {
  if (filtro.value == '') {
    return nomes.value
  }
  return nomes.value.filter(n => {
    return n.toLowerCase().startsWith(filtro.value.toLowerCase())
  })
})


function upper() {
  titulo.value = titulo.value.toUpperCase()
}

function lower() {
  titulo.value = titulo.value.toLowerCase()
}

function inverter() {
  let texto_invertido = ''
  for (let i = titulo.value.length - 1; i >= 0; i--) { 
    texto_invertido += titulo.value[i] 
  }
  titulo.value = texto_invertido
  isTextoInvertido.value = !isTextoInvertido.value
}

function contarLetras() {
   quantidadeLetras.value = 
      titulo.value.split('')
       .filter(letra => 
          letra.toLowerCase() == 'â' || 
          letra.toLowerCase() == 'a' || 
          letra.toLowerCase() == 'á' || 
          letra.toLowerCase() == 'à' ).length
}

function adicionarAlterarNome() {
  if (indexEditar.value < 0)
    nomes.value.push(nomeParaAdicionar.value)
  else 
    nomes.value[indexEditar.value] = nomeParaAdicionar.value
  nomeParaAdicionar.value = ''
  indexEditar.value = -1
}

function excluir(nomeParaExcluir) {
  console.log(nomeParaExcluir)
  let idx = nomes.value.indexOf(nomeParaExcluir)
  if (idx > -1) {
    nomes.value.splice(idx, 1)
  }
}

function editar(idx) {
  indexEditar.value = idx
  nomeParaAdicionar.value = nomes.value[idx]
}


</script>

<template>
  <div>

    <h3>{{ titulo }}</h3>
    <button @click="upper()" >To Upper Case</button>
    <button @click="lower()" >To Lower Case</button>

    <button @click="inverter()">Inverter o título</button>

    <button @click="contarLetras()">Contar quantas letras "a", o título possui</button>

    <div v-if="quantidadeLetras">
      Quantidade de letras a : {{  quantidadeLetras }}
    </div>
    <div v-show="isTextoInvertido">
      O texto está invertido
    </div>

    <br>
    <hr>
      <form @submit.prevent="adicionarAlterarNome()">
        <label for="nome">Nome</label>
        <input type="text" v-model="nomeParaAdicionar" required/>
        <button type="submit">
          <span v-if="indexEditar < 0">
            Adicionar
          </span>
          <span v-else>
            Editar
          </span>  
        </button><br>
      </form>  
    <hr>

    <hr>
      <div :class="{ destaque : filtro != '' }">
        <label for="filtro">Filtro</label>  
        <input type="text" id="filtro" v-model="filtro"/>
      </div>
    <hr>
    <ol>
      <li v-for="(n, idx) in nomesFiltrados">{{ n }}  

      <a href="#" @click="editar(idx)">Editar</a>
      <a href="#" @click="excluir(n)">Excluir</a> </li>
    </ol>

    <table>
      <tr>
        <td>Nome</td>
        <td>Cor</td>
        <td>Valor</td>
        <td>Avaliação valor > 1500 = "preço alto" senão "preco baixo"</td> 
      </tr>
      <tr v-for="c in carros">
        <td>{{ c.nome }}</td>
        <td>{{ c.cor }}</td>
        <td>{{ c.valor }}</td>
        <td v-if="c.valor > 1500">
            Preço alto
        </td>
        <td v-else>
            Preço baixo
        </td>

      </tr>
    </table>
   
  </div> 
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.destaque {
  background-color: yellow;
}
</style>
