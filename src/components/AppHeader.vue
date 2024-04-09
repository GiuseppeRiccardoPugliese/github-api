<script>
export default {
    name: 'AppHeader',
    data() {
        return {

        };
    },
    methods: {

    },
}


</script>

<template>

    <nav class="navbar bg-light">
        <div class="container-fluid">
            <a class="navbar-brand"><strong>GitHub Api</strong></a>
            <select name="seleziona" id="">
                <option value="Seleziona" selected disabled>Seleziona un'opzione</option>
                <option value="Repositories" aria-placeholder="Inserisci una tipologia">Repositories</option>
            </select>
        </div>
    </nav>

</template>

<style scoped lang="scss"></style>
