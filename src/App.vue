<template>
    <div id="app">
        <Table/>
    </div>
</template>

<script>
// @ is an alias to /src
import Table from '@/components/Table.vue'

export default {
    name: 'App',
    components: {
		Table
	}
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 200px;
}
</style>
