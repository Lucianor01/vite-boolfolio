<script>

import axios from 'axios';

export default {
    name: "ProjectList",
    data() {
        return {
            projects: [],
            baseUrl: 'http://127.0.0.1:8000',
            currentPage: 1,
            lastPage: null,
            types: null,
            selectedType: "all",
            technologies: null,
            selectedTechnologies: []
        }
    },
    mounted() {
        this.getProjects(1);
        this.getTypes();
        this.getTechnologies();
    },
    watch: {
        selectedTechnologies: {
            handler: 'getProjects',
            deep: true
        }
    },
    methods: {
        getProjects(projectApiPage) {

            const params = {
                page: projectApiPage
            }

            if (this.selectedType !== 'all') {
                params.type_id = this.selectedType
            }

            if (this.selectedTechnologies.length > 0) {
                params.technologies_ids = this.selectedTechnologies.join(',')
            }



            axios.get(`${this.baseUrl}/api/projects`, { params }).then(res => {
                // DATI DELLA PAGINA CORRENTE
                this.projects = res.data.projects.data
                this.currentPage = res.data.projects.current_page
                this.lastPage = res.data.projects.last_page
            })
        },

        getTypes() {
            axios.get(`${this.baseUrl}/api/types`).then(res => {
                this.types = res.data.types
            })
        },

        getTechnologies() {
            axios.get(`${this.baseUrl}/api/technologies`).then(res => {
                this.technologies = res.data.technologies
            })
        }
    },
}

</script>

<template>
    <div class="container mt-5">
        <!-- SELEZIONE LE CATEGORIES ONE TO MANY -->
        <div class="mb-3">
            <label for="" class="form-label">Types Filter</label>
            <select @change="getProjects()" v-model="selectedType" class="form-select form-select-lg" name="" id="">
                <option value="all" selected>-- All --</option>
                <option :value="item.id" v-for="(item, index) in types" :key="index">{{ item.name }}</option>
            </select>
        </div>
        <!-- TECHNOLOGIES -->
        <div class="list-group-inline form-check-inline mt-3">
            <div>
                <span>Technologies Checkbox</span>
            </div>
            <label class="list-group-item-inline form-check-inline" v-for="(item, index) in technologies" :key="index">
                <input class="form-check-input me-1" type="checkbox" :value="item.id" v-model="selectedTechnologies">
                {{ item.name }}
            </label>
        </div>
        <!-- CICLO PROJECT -->
        <div class="row row-gap-4 mt-5">
            <div class="col-3" v-for="(elem, index) in projects" :key="index">
                <router-link class="text-decoration-none" :to="{ name: 'project', params: { slug: elem.slug } }">
                    <div class="card h-100">
                        <img class="card-img-top" :src="`${baseUrl}/storage/${elem.project_image}`" alt="Title">
                        <div class="card-body">
                            <h4 class="card-title text-uppercase">{{ elem.title }}</h4>
                            <p class="card-text"><strong>Description:</strong> {{ elem.description }}</p>
                            <p class="card-text"><strong>Price:</strong> {{ elem.price }}&euro;</p>
                            <p class="card-text" v-if="elem.type"><strong>Type:</strong> {{ elem.type.name }}</p>
                        </div>
                    </div>
                </router-link>
            </div>
        </div>
    </div>
    <!-- PAGE NAVIGATION -->
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
