<script>
import axios from "axios";

export default {
    name: 'ApiGitHub',
    data() {
        return {
            repositories: [], //ARRAY PER LE REPO
            searchInput: '',
        };
    },
    methods: {
        getRepo() {
            const selectedTypology = document.getElementById('seleziona').value; //Variabile per le options

            let url;

            let data = {
                params: {
                    q: this.searchInput,
                },
                headers: {
                    "Authorization": `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`,
                    "X-GitHub-Api-Version": "2022-11-28"
                }
            };

            if (selectedTypology == 'repositories') {
                url = 'https://api.github.com/search/repositories';
            }
            else if (selectedTypology == 'utenti_organizzazioni') {
                url = 'https://api.github.com/search/users';
            }
            console.log(data);

            axios.get(`${url}`, data)
                .then((res) => {
                    this.repositories = res.data.items;
                    console.log(this.repositories);
                })
                .catch((error) => {
                    console.error("Error fetching data from first API:", error);
                });
        }
    },
    mounted() {

    },
}


</script>

<template>
    <!-- NAV -->
    <nav class="navbar bg-light">
        <div class="container-fluid">
            <a class="navbar-brand"><strong>GitHub Api</strong></a>
            <div class="search_input d-flex">
                <input @keyup.enter="getRepo()" v-model="searchInput" class="form-control mx-1" type="search"
                    placeholder="Search" aria-label="Search">
                <select class="mx-1" name="seleziona" id="seleziona">
                    <option disabled>Seleziona un'opzione</option>
                    <option value="repositories" selected>Repositories</option>
                    <option value="utenti_organizzazioni">Utenti/Organizzazioni</option>
                </select>
                <button @click="getRepo()" class="btn btn-outline-success" type="submit">CERCA</button>
            </div>
        </div>
    </nav>

    <!-- CARD REPO -->
    <div class="container">
        <div class="row justify-content-center mt-4">
            <div v-for="repo in repositories" class="card m-2" style="width: 18rem; min-height: 350px;">
                <div class="img_container d-flex justify-content-center">
                    <img :src="repo.owner ? repo.owner.avatar_url : repo.avatar_url" class="card-img-top" alt="repo_img"
                        style="width: 10rem; border-radius: 10px;">
                </div>
                <div class="card-body h-100 d-flex flex-column">
                    <h5 class="card-title">{{ repo.full_name ? repo.full_name : repo.login }}</h5>
                    <h6 v-if="repo.type"> Profilo: {{ repo.type == 'User' ? 'Utente' : 'Organizzazione' }}</h6>
                    <p class="card-text">{{ repo.description }}</p>
                    <span class="text-center border-1 border-bottom py-2"><i class="fa-solid fa-star px-1"></i>{{
                        repo.stargazers_count }}</span>
                    <span class="text-center py-2"><i class="fa-solid fa-circle-exclamation px-1"></i>{{
                        repo.open_issues
                        }}</span>
                    <a :href="repo.html_url" class="btn btn-primary mt-auto">{{ repo.type ? 'Vai al profilo' :
                        'Vai alla repo' }} <i class="fa-solid fa-arrow-up-right-from-square"></i></a>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped lang="scss"></style>
