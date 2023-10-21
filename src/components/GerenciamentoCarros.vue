<script setup>
import { computed, ref } from "vue";
const filtroCarro = ref('')
const carros = ref([
    { nome: "Fusca", valor: 1000, cor: "Azul" },
    { nome: "Variante", valor: 2000, cor: "Amarela" },
    { nome: "Brasilia", valor: 3000, cor: "Branca" },
])

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

const carrosFiltrados = computed(() => {
    if (filtroCarro.value == '') {
        return carros.value
    }

    return carros.value.filter(c => {
        var result = c.nome.toLowerCase().startsWith(filtroCarro.value.toLowerCase())
        orderCarByValue()
        return result
    })
})

function orderCarByName() {
    carrosFiltrados.value.sort((c1, c2) => {
        return c1.nome.localeCompare(c2.nome)
    })
}

function orderCarByNameDesc() {
    carrosFiltrados.value.sort((c1, c2) => {
        return c2.nome.localeCompare(c1.nome)
    })
}

function orderCarByValue() {
    carrosFiltrados.value.sort((c1, c2) => {
        return c1.valor - c2.valor
    })
}

function orderCarByValueDesc() {
    carrosFiltrados.value.sort((c1, c2) => {
        return c2.valor - c1.valor
    })
}

function orderCarByColor() {
    carrosFiltrados.value.sort((c1, c2) => {
        return c1.cor.localeCompare(c2.cor)
    })
}

function orderCarByColorDesc() {
    carrosFiltrados.value.sort((c1, c2) => {
        return c2.cor.localeCompare(c1.cor)
    })
}
</script>

<template>
    <section class="mt-5 bg-light rounded-3 p-3">
        <!-- Filtrar Carro-->
        <div class="p-3">
            <h5>Filtrar Carro</h5>
            <label class="p-1 d-flex" for="campoFiltroCarro">Filtro</label>
            <input class="form-control" type="text" name="" id="campoFiltroCarro" placeholder="digite o carro para filtrar"
                v-model="filtroCarro">
        </div>
        <h5>Veiculos Registrados</h5>
        <table class="table table-striped table-hover">
            <thead>
                <tr>
                    <th class="col">Nome
                        <button class="btn btn-sm ms-1 btn-success" @click="orderCarByName()">↑</button>
                        <button class="btn btn-sm ms-1 btn-success" @click="orderCarByNameDesc()">↓</button>
                    </th>
                    <th class=" col">Cor
                        <button class="btn btn-sm ms-1 btn-success" @click="orderCarByColor()">↑</button>
                        <button class="btn btn-sm ms-1 btn-success" @click="orderCarByColorDesc()">↓</button>
                    </th>
                    <th class="col">Valor
                        <button class="btn btn-sm ms-1 btn-success" @click="orderCarByValue()">↑</button>
                        <button class="btn btn-sm ms-1 btn-success" @click="orderCarByValueDesc()">↓</button>
                    </th>
                    <th class="col">Preço</th>
                    <th class="col">Excluir</th>
                    <th class="col">Editar</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="c in carrosFiltrados">
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
        <button class="m-3 btn btn-primary"
            @click="isVisibleMenuAdicionarCarro = !isVisibleMenuAdicionarCarro">Adicionar</button>

        <!--Adicionar Veiculo-->
        <div v-show="isVisibleMenuAdicionarCarro">
            <h5>Adicionar Veiculo</h5>
            <form action="" @submit.prevent="adicionarCarro()">
                <div class="form-floating mb-3">
                    <input type="text" class="form-control" v-model="carroParaAdicionar.nome" required>
                    <label for="nomeVeiculo">Nome do Veiculo</label>
                </div>
                <div class="form-floating mb-3">
                    <input type="text" class="form-control" v-model="carroParaAdicionar.cor" required>
                    <label for="corVeiculo">Cor do Veiculo</label>
                </div>
                <div class="form-floating mb-3">
                    <input type="number" class="form-control" v-model="carroParaAdicionar.valor" required>
                    <label for="valorVeiculo">Valor</label>
                </div>
                <button class="m-3 btn btn-primary" type="submit">Salvar</button>
            </form>
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