<script>

import ProjectList from '../pages/ProjectList.vue';

export default {
    name: "MainComp",
    components: {
        ProjectList
    },
    data() {
        return {

        }
    },
    methods: {

    },
}

</script>

<template>
    <ProjectList />
    <router-view></router-view>
</template>

<style lang="scss" scoped></style>
