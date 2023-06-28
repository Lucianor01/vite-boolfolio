<script>

import axios from 'axios';

export default {
    name: "ProjectCardComp",
    data() {
        return {
            projects: [],
            baseUrl: 'http://127.0.0.1:8000',
            currentPage: 1,
            lastPage: null
        }
    },
    mounted() {
        this.getProjects(1)
    },
    methods: {
        getProjects(projectApiPage) {
            axios.get(`${this.baseUrl}/api/projects`, {
                params: {
                    page: projectApiPage
                }
            }).then(res => {
                // DATI DELLA PAGINA CORRENTE
                this.projects = res.data.projects.data
                this.currentPage = res.data.projects.current_page
                this.lastPage = res.data.projects.last_page
            })
        }
    },
}

</script>

<template>
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

    <nav aria-label="Page navigation" class="container mt-5">
        <ul class="pagination    ">
            <li class="page-item" :class="(currentPage === 1) ? 'disabled' : ''">
                <a class="page-link" href="#" aria-label="Previous" @click.prevent="getProjects(currentPage - 1)">
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
            <li class="page-item" :class="(currentPage === item) ? 'active' : ''" aria-current="page"
                v-for="( item, index ) in  lastPage " :key="index">
                <a class="page-link" @click.prevent="getProjects(item)" href="#">{{ item }}</a>
            </li>
            <li class="page-item" :class="(currentPage === lastPage) ? 'disabled' : ''">
                <a class="page-link" href="#" aria-label="Next" @click.prevent="getProjects(currentPage + 1)">
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </ul>
    </nav>
</template>

<style lang="scss" scoped></style>
