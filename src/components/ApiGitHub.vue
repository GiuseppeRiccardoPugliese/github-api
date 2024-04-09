<script>
import axios from "axios";

export default {
    name: 'ApiGitHub',
    data() {
        return {
            repositories: [], //ARRAY PER LE REPO
        };
    },
    methods: {

    },
    mounted() {
        const data = {
            params: {
                // q: searchInput.value
                q: 'vuejs'
            },
            headers: {
                "Authorization": `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`,
                "X-GitHub-Api-Version": "2022-11-28"
            }
        };
        axios.get('https://api.github.com/search/repositories', data)
            .then((res) => {
                // res.data.forEach(element => {
                //     this.repositories.push(element.name);
                // });
                // console.log(this.repositories[0]);
                this.repositories = res.data.items;

            })
            .catch((error) => {
                console.error("Error fetching data from first API:", error);
            });
    },
}


</script>

<template>
    <div class="container">
        <div class="row justify-content-center mt-4">
            <div v-for="repo in repositories" class="card m-2" style="width: 18rem; min-height: 350px;">
                <img :src="repo.owner.avatar_url" class="card-img-top" alt="repo_img">
                <div class="card-body h-100 d-flex flex-column">
                    <h5 class="card-title">{{ repo.full_name }}</h5>
                    <p class="card-text">{{ repo.description }}</p>
                    <a :href="repo.html_url" class="btn btn-primary mt-auto">Vai al repo <i
                            class="fa-solid fa-arrow-up-right-from-square"></i></a>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped lang="scss"></style>
