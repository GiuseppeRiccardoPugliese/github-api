<script>
import axios from "axios";
import config from "../config.js";

export default {
    name: 'ApiGitHub',
    data() {
        return {
            repositories: [], //ARRAY contenitore RISULTATI
            searchInput: '',  //Input di RICERCA
            token: config.token, //Token
            showErrorMessage: false, // Mostra il messaggio di errore
            errorMessage: '', // Testo del messaggio di errore
            searchDone: false, // Variabile per vedere se la ricerca è stata effettuata
            loading: false, // Stato di caricamento
        };
    },
    methods: {
        async getRepo() {
            //VALIDAZIONE
            if (!this.searchInput) {
                this.errorMessage = 'Il campo di ricerca è obbligatorio';
                this.showErrorMessage = true;
                return;
            }
            if (this.searchInput.length < 3) {
                this.errorMessage = 'Il campo di ricerca deve essere almeno di 3 caratteri';
                this.showErrorMessage = true;
                return;
            } else {
                this.errorMessage = '';
                this.showErrorMessage = false;
            }

            // Imposto lo stato di caricamento su true del loader prima di fare la chiamata API
            this.loading = true;

            const selectedTypology = document.getElementById('seleziona').value; //Variabile per le options
            let url; //Url per la condizione in base alla ricerca di REPO oppure USER/ORG

            //CONDIZIONE PER LA CHIAMATA IN BASE ALLA SELEZIONE FATTA NELLA SELECT
            if (selectedTypology == 'repositories') {
                url = 'https://api.github.com/search/repositories'; //Allora URL REPO
            } else if (selectedTypology == 'utenti_organizzazioni') {
                url = 'https://api.github.com/search/users'; //Allora URL USER/ORG
            }

            try {
                const response = await axios.get(url, {
                    params: {
                        q: this.searchInput,
                    },
                    headers: {
                        "Authorization": `Bearer ${this.token}`,
                        "X-GitHub-Api-Version": "2022-11-28"
                    }
                });

                // Aggiorno i dati
                this.repositories = response.data.items;

                this.searchDone = true; //Imposto la variabile su true quando e' effettuata la chiamata

                // console.log('log array repo', this.repositories);
            } catch (error) {
                console.error("Error fetching data from API:", error);
                this.repositories = []; // Pulisco l'array iniziale in caso di errore
                this.errorMessage = 'Si è verificato un errore durante il recupero dei dati';
                this.showErrorMessage = true;
            } finally {
                // Imposto lo stato del loader su false quando ho ottenuto i risultati o quando si è verificato un errore
                this.loading = false;
            }
        }
    },
};
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

    <!-- Loader -->
    <div class="loader-container" v-if="loading">
        <div class="loader"></div>
    </div>

    <!-- Messaggio di errore -->
    <div class="error-message" v-if="showErrorMessage">
        {{ errorMessage }}
    </div>

    <!-- Se non c'e' nessun risultato allora: -->
    <h1 v-if="repositories.length === 0 && searchDone" class="text-center">Non esiste nessun risultato correlato
        alla ricerca</h1>

    <!-- CARD REPO -->
    <div class="container">
        <div class="row justify-content-center mt-4">
            <div v-for="repo in repositories" class="card m-3 shadow" :class="!repo.type ? 'repo_card' : 'user_card'"
                style="width: 18rem; min-height: 350px;">
                <div class="img_container d-flex justify-content-center align-items-center">
                    <img :src="repo.owner ? repo.owner.avatar_url : repo.avatar_url" class="card-img-top rounded-circle"
                        alt="repo_img">
                </div>
                <div class="card-body h-100 d-flex flex-column">
                    <h5 class="card-title">{{ repo.full_name ? repo.full_name : repo.login }}</h5>
                    <h6 v-if="repo.type"> Profilo: {{ repo.type == 'User' ? 'Utente' : 'Organizzazione' }}</h6>
                    <p class="card-text overflow-auto">{{ repo.description }}</p>
                    <span v-if="!repo.type" class="text-center border-1 border-bottom py-2"><i
                            class="fa-solid fa-star px-1"></i>{{
                                repo.stargazers_count }}</span>
                    <span v-if="!repo.type" class="text-center py-2"><i
                            class="fa-solid fa-circle-exclamation px-1"></i>{{
                                repo.open_issues
                            }}</span>
                    <a :href="repo.html_url" class="btn btn-primary mt-auto">{{ repo.type ? 'Vai al profilo' :
                        'Vai alla repo' }} <i class="fa-solid fa-arrow-up-right-from-square"></i></a>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
//Messaggio Errore Validazione
.error-message {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #ffcccc;
    padding: 10px;
    border-radius: 5px;
    z-index: 999;
}

//Card w/scroll-bar
.repo_card {
    background: linear-gradient(to bottom right, #4e9df4, #8a3ab9);
}

.user_card {
    background: linear-gradient(to right, #ff9ff3, #ff6bcb);
}

.card {
    .img_container {
        width: 100%;
        height: 100%;
        padding-top: 10px;

        img {
            width: 10rem;
        }
    }

    .overflow-auto {
        overflow-y: scroll;
        max-height: 80px;
    }

    .overflow-auto::-webkit-scrollbar {
        width: 8px;
    }

    .overflow-auto::-webkit-scrollbar-track {
        background-color: #f5f5f5;
    }

    .overflow-auto::-webkit-scrollbar-thumb {
        background-color: #ccc;
        border-radius: 10px;
        border: 2px solid #f5f5f5;
    }

    .overflow-auto::-webkit-scrollbar-thumb:hover {
        background-color: #aaa;
    }

    .overflow-auto::-webkit-scrollbar-thumb:active {
        background-color: #888;
    }
}

//Card shadow
.card.shadow {
    box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.card.shadow:hover {
    transform: scale(1.05);
}

// LOADER SETUP
.loader-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.7);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;

    .loader {
        border: 8px solid #f3f3f3;
        border-top: 8px solid #3498db;
        border-radius: 50%;
        width: 50px;
        height: 50px;
        animation: spin 1s linear infinite;
    }
}


@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}
</style>