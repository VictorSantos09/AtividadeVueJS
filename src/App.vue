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
  let index = carros.value.findIndex(x => x.nome == nome)
  carros.value.splice(index, 1)
}

var carroParaEditar = ref({
  nome: '',
  cor: '',
  valor: 0
})

const isVisibleMenuEditarCarro = ref(false)
function setupMenuEditar(carro) {
  isVisibleMenuEditarCarro.value = !isVisibleMenuEditarCarro.value;
  document.getElementById('nomeVeiculo').value = carro.nome
  document.getElementById('corVeiculo').value = carro.cor
  document.getElementById('valorVeiculo').value = carro.valor
  carroParaEditar = carro
}

function confirmarMenuEditar() {
  carroParaEditar.nome = document.getElementById('nomeVeiculo').value
  carroParaEditar.cor = document.getElementById('corVeiculo').value
  carroParaEditar.valor = document.getElementById('valorVeiculo').value
}

const carroParaAdicionar = ref({
  nome: '',
  cor: '',
  valor: 0
})

const isVisibleMenuAdicionarCarro = ref(false)
function adicionarCarro() {
  carros.value.push(carroParaAdicionar.value)
  carroParaAdicionar.value = ref({
    nome: '',
    cor: '',
    valor: 0
  })

  isVisibleMenuAdicionarCarro.value = !isVisibleMenuAdicionarCarro
}
</script>

<template>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">

  <!--Titulo e Botões-->
  <section class="bg-light rounded-3 p-3">
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
  </section>

  <!--Pessoas Registradas-->
  <section class="mt-5 bg-light p-3">
    <h5>Pessoas Registradas</h5>
    <ol>
      <li v-for="n in nomes">{{ n }} <button class="btn btn-primary btn-sm m-2" @click="excluir(n)">excluir</button></li>
    </ol>

    <input class="form-control" type="text" placeholder="Digite o nome da pessoa a ser adicionada"
      v-model="nomeParaAdicionar" />
    <button class="mt-3 btn btn-primary" @click="adicionarNome()">Adicionar</button><br>
  </section>

  <!--Veiculos Registrados-->
  <section class="mt-5 bg-light rounded-3 p-3">
    <h5>Veiculos Registrados</h5>
    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th class="col">Nome</th>
          <th class="col">Cor</th>
          <th class="col">Valor</th>
          <th class="col">Preço</th>
          <th class="col">Excluir</th>
          <th class="col">Editar</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="c in carros">
          <td scope="row">{{ c.nome }}</td>
          <td scope="row">{{ c.cor }}</td>
          <td scope="row">{{ c.valor }}</td>
          <td scope="row" v-if="c.valor > 1500">
            Preço alto
          </td>
          <td v-else>
            Preço baixo
          </td>
          <td>
            <button class="btn btn-sm btn-primary" @click="excluirCarro(c.nome)">Excluir</button>
          </td>
          <td>
            <button class="btn btn-sm btn-primary" @click="setupMenuEditar(c)">Editar</button>
          </td>
        </tr>
      </tbody>
    </table>

    <button class="m-3 btn btn-primary" @click="isVisibleMenuAdicionarCarro = !isVisibleMenuAdicionarCarro">Adicionar Novo
      Veiculo</button>

    <!--Adicionar Veiculo-->
    <div v-show="isVisibleMenuAdicionarCarro">
      <h5>Adicionar Veiculo</h5>
      <div class="form-floating mb-3">
        <input type="text" class="form-control" v-model="carroParaAdicionar.nome">
        <label for="nomeVeiculo">Nome do Veiculo</label>
      </div>
      <div class="form-floating mb-3">
        <input type="text" class="form-control" v-model="carroParaAdicionar.cor">
        <label for="corVeiculo">Cor do Veiculo</label>
      </div>
      <div class="form-floating mb-3">
        <input type="number" class="form-control" v-model="carroParaAdicionar.valor">
        <label for="valorVeiculo">Valor</label>
      </div>

      <button class="m-3 btn btn-primary" @click="adicionarCarro()">Adicionar</button>
    </div>

    <!--Editar Veiculo-->
    <div v-show="isVisibleMenuEditarCarro" class="bg-light p-3">
      <h5>Editar Veiculo</h5>
      <div class="form-floating mb-3">
        <input type="text" class="form-control" id="nomeVeiculo">
        <label for="nomeVeiculo">Nome do Veiculo</label>
      </div>
      <div class="form-floating mb-3">
        <input type="text" class="form-control" id="corVeiculo">
        <label for="corVeiculo">Cor do Veiculo</label>
      </div>
      <div class="form-floating mb-3">
        <input type="number" class="form-control" id="valorVeiculo">
        <label for="valorVeiculo">Valor</label>
      </div>

      <div class="d-flex">
        <button class="m-2 btn btn-danger" @click="confirmarMenuEditar()">Confirmar</button>
        <button class="m-2 btn btn-primary"
          @click="isVisibleMenuEditarCarro = !isVisibleMenuEditarCarro">Cancelar</button>
      </div>
    </div>
  </section>
</template>