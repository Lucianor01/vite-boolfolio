<script>

import axios from 'axios';

export default {
    name: "ProjectCardComp",
    data() {
        return {
            projects: [],
            baseUrl: 'http://127.0.0.1:8000'
        }
    },
    mounted() {
        this.getProjects()
    },
    methods: {
        getProjects() {
            axios.get(`${this.baseUrl}/api/projects`)
                .then(res => {
                    this.projects = res.data.projects
                })
        }
    },
}

</script>

<template>
    <header>
        <div class="container mt-5">
            <div class="row row-gap-4">
                <div class="col-3" v-for="(elem, index) in projects" :key="index">
                    <div class="card h-100">
                        <img class="card-img-top" :src="`${baseUrl}/storage/${elem.project_image}`" alt="Title">
                        <div class="card-body">
                            <h4 class="card-title text-uppercase">{{ elem.title }}</h4>
                            <p class="card-text"><strong>Description:</strong> {{ elem.description }}</p>
                            <p class="card-text"><strong>Price:</strong> {{ elem.price }}&euro;</p>
                            <p class="card-text" v-if="elem.type"><strong>Type:</strong> {{ elem.type.name }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<style lang="scss" scoped></style>
