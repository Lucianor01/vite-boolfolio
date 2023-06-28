<script>

import axios from 'axios';

export default {
    name: "ProjectComp",
    data() {
        return {
            projects: [],
            baseUrl: 'http://127.0.0.1:8000/api'
        }
    },
    mounted() {
        this.getProjects()
    },
    methods: {
        getProjects() {
            axios.get(`${this.baseUrl}/projects`)
                .then(res => {
                    this.projects = res.data.projects
                })
        }
    },
}

</script>

<template>
    <header>
        <div class="container">
            <div class="row">
                <div class="col-2" v-for="(elem, index) in projects" :key="index">
                    <div class="card">
                        <img class="card-img-top" src="holder.js/100x180/" alt="Title">
                        <div class="card-body">
                            <h4 class="card-title">{{ elem.title }}</h4>
                            <p class="card-text">Text</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<style lang="scss" scoped></style>
