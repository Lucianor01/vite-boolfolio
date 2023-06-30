<script>

import axios from 'axios';

export default {
    name: "SingleProject",
    data() {
        return {
            baseUrl: 'http://127.0.0.1:8000',
            project: null,
        }
    },
    beforeMount() {
        // console.log(this.$route);
        this.getSingleProject()
    },
    methods: {

        getSingleProject() {
            axios.get(`${this.baseUrl}/api/projects/${this.$route.params.slug}`)
                .then((response) => {
                    this.project = response.data.project;
                }, error => {
                    if (error.response.status === 404) {
                        this.$router.push({ name: 'not-found' })
                    } else {

                    }
                }
                );
        }
    },
}

</script>

<template>
    <div class="container">
        <h1 class="text-center mt-5 text-uppercase">Single Project</h1>
        <div class="col-3 m-auto mt-4">
            <div class="card h-100">
                <img class="card-img-top" :src="`${baseUrl}/storage/${project.project_image}`" alt="Title">
                <div class="card-body">
                    <h4 class="card-title text-uppercase">{{ project.title }}</h4>
                    <p class="card-text"><strong>Description:</strong> {{ project.description }}</p>
                    <p class="card-text"><strong>Price:</strong> {{ project.price }}&euro;</p>
                    <p class="card-text" v-if="project.type"><strong>Type:</strong> {{ project.type.name }}</p>
                    <div v-if="project.technologies.length > 0" class="card-text d-flex">
                        <p class="me-2"><strong>Technologies:</strong></p>
                        <ul class="list-group list-group-horizontal list-unstyled">
                            <li v-for="(item, index) in project.technologies" :key="index" class="me-1">
                                {{ item.name + (index !== project.technologies.length - 1 ? '/' : '') }}
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>
