<script setup>
import GerenciamentoCarros from './components/GerenciamentoCarros.vue'
import GerenciamentoNomes from './components/GerenciamentoNomes.vue'
import GerenciamentoTitulo from './components/GerenciamentoTitulo.vue'

</script>

<template>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">

  <!--Titulo e Botões-->
  <GerenciamentoTitulo />

  <!--Pessoas Registradas-->
  <GerenciamentoNomes />

  <!--Veiculos Registrados-->
  <GerenciamentoCarros />
</template>