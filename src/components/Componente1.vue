<template>
	<button @click='emitirEvento'>Generar Evento</button>
</template>

<script setup>
	import { defineEmits } from 'vue';

	const emit = defineEmits();

	function emitirEvento(){
		emit('eventoGenerado', 'Evento generado desde el Componente1');
	}
</script>