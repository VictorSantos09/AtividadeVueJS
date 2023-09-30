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

function excluirCarro(nome) {
  let index = carros.value.find(x => x.nome.match(nome))
}

</script>

<template>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
  <div class="container">
    <!--Titulo e Botões-->
    <div class="bg-light rounded-3 p-3">
      <h3>{{ titulo }}</h3>
      <button class="btn btn-primary m-2" @click="upper()">To Upper Case</button>
      <button class="btn btn-primary m-2" @click="lower()">To Lower Case</button>
      <button class="btn btn-primary m-2" @click="inverter()">Inverter o título</button>
      <button class="btn btn-primary m-2" @click="contarLetras()">Contar quantas letras "a", o título possui</button>

      <div v-if="quantidadeLetras">
        Quantidade de letras a : {{ quantidadeLetras }}
      </div>

      <div v-show="isTextoInvertido">
        O texto está invertido
      </div>
    </div>

    <!--Nomes Registrados-->
    <div class="mt-5 bg-light p-3">
      <h5>Nomes Registrados</h5>
      <ol>
        <li v-for="n in nomes">{{ n }}</li>
      </ol>

      <input class="form-control" type="text" placeholder="Digite o nome da pessoa" v-model="nomeParaAdicionar" />
      <button class="btn btn-primary" @click="adicionarNome()">Adicionar</button><br>
    </div>

    <!--Veiculos Registrados-->
    <div class="mt-5  bg-light rounded-3 p-3">
      <h5>Veiculos Registrados</h5>
      <table class="table">
        <tr>
          <th class="col">Nome</th>
          <th class="col">Cor</th>
          <th class="col">Valor</th>
          <th class="col">Preço</th>
          <th class="col">Excluir</th>
          <th class="col">Editar</th>
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
          <td><button class="btn btn-primary" @click="excluirCarro(c.nome)">Excluir</button></td>
          <td>Editar</td>
        </tr>
      </table>
    </div>

    <!--Pessoas Registradas-->
    <div class="mt-5 bg-light rounded-3 p-3">
      <h5>Pessoas Registradas</h5>
      <ol class="list-group">
        <li class="list-group-item" v-for="n in nomes">{{ n }}<button class="m-1 btn btn-sm btn-primary"
            @click="excluir(n)">Excluir</button></li>
      </ol>
    </div>
  </div>
</template>