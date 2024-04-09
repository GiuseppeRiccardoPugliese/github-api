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
            <div class="search_input d-flex">
                <input class="form-control mx-1" type="search" placeholder="Search" aria-label="Search">
                <select class="mx-1" name="seleziona" id="">
                    <option value="Seleziona" selected disabled>Seleziona un'opzione</option>
                    <option value="Repositories" aria-placeholder="Inserisci una tipologia">Repositories</option>
                    <option value="Repositories" aria-placeholder="Inserisci una tipologia">Utenti</option>
                </select>
                <button class="btn btn-outline-success" type="submit">CERCA</button>
            </div>
        </div>
    </nav>

</template>

<style scoped lang="scss"></style>
