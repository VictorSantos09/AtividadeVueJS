<script setup>
import { ref } from 'vue'

const titulo = ref('Meu título dinâmico')
const quantidadeLetras = ref()
const isTextoInvertido = ref(false)
const nomes = ref(['Malu', 'Bia', 'Thom'])
const carros = ref([
  { nome: "Fusca", valor: 1000, cor: "Azul" },
  { nome: "Variante", valor: 2000, cor: "Amarela" },
  { nome: "Brasilia", valor: 3000, cor: "Branca" },
])
const nomeParaAdicionar = ref('')

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
        letra.toLowerCase() == 'à').length
}

function adicionarNome() {
  nomes.value.push(nomeParaAdicionar.value)
  nomeParaAdicionar.value = ''
}

function excluir(nome) {
  let index = nomes.value.indexOf(nome)

  if (index >= 0)
    nomes.value.splice(index, 1)
}

</script>

<template>
  <div>

    <h3>{{ titulo }}</h3>
    <button @click="upper()">To Upper Case</button>
    <button @click="lower()">To Lower Case</button>

    <button @click="inverter()">Inverter o título</button>

    <button @click="contarLetras()">Contar quantas letras "a", o título possui</button>

    <div v-if="quantidadeLetras">
      Quantidade de letras a : {{ quantidadeLetras }}
    </div>
    <div v-show="isTextoInvertido">
      O texto está invertido
    </div>

    <br>
    <input type="text" v-model="nomeParaAdicionar" />
    <button @click="adicionarNome()">Adicionar</button><br>

    <ol>
      <li v-for="n in nomes">{{ n }}</li>
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

    <!--Nomes para exlucir-->
    <ol>
      <li v-for="n in nomes">{{ n }}<button @click="excluir(n)">Excluir</button></li>
    </ol>

  </div>
</template>