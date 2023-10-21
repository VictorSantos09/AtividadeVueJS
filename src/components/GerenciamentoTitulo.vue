<script setup>
import { computed, ref } from "vue";

const titulo = ref('Meu título dinâmico')
const quantidadeLetras = ref()
const isTextoInvertido = ref(false)

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
</script>

<template>
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
</template>