<script setup>
import { computed, ref } from "vue";
const nomes = ref(['Malu', 'Bia', 'Thom'])
const nomeParaAdicionar = ref('')
function adicionarNome() {
    nomes.value.push(nomeParaAdicionar.value)
    nomeParaAdicionar.value = ''
}

function excluir(nome) {
    let index = nomes.value.indexOf(nome)

    if (index >= 0)
        nomes.value.splice(index, 1)
}

const filtro = ref('')

const nomesFiltrados = computed(() => {
    if (filtro.value == '') {
        return nomes.value
    }

    return nomes.value.filter(n => {
        return n.toLowerCase().startsWith(filtro.value.toLowerCase())
    })
})
</script>

<template>
    <section class="mt-5 bg-light p-3">
        <!--Filtrar Pessoa-->
        <div class="p-3">
            <h5>Filtrar Pessoa</h5>
            <label class="p-1 d-flex" for="campoFiltro">Filtro</label>
            <input class="form-control" type="text" name="" id="campoFiltro" placeholder="digite o nome a ser filtrado"
                v-model="filtro">
        </div>

        <h5>Pessoas Registradas</h5>
        <ol>
            <li v-for="n in nomesFiltrados">{{ n }} <button class="btn btn-primary btn-sm m-2"
                    @click="excluir(n)">excluir</button>
            </li>
        </ol>

        <form action="" @submit.prevent="adicionarNome()">
            <input class="form-control" type="text" placeholder="Digite o nome da pessoa a ser adicionada"
                v-model="nomeParaAdicionar" required />
            <button class="mt-3 btn btn-primary" type="submit">Adicionar</button>
        </form>
    </section>
</template>