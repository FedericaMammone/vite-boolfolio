<template>
    <h3>[HOME]</h3>
    <div class="container" style="height: 500px;">
        <ul>
            <li v-for="project in projects" :key="project.id" class="list-unstyled text-center">
                {{ project.title }}
            </li>
        </ul>
    </div>

    <div class="text-center">
        <div v-for="project in projects" :key="project.id" :project="project" class="my-4 mx-4">
        </div>
        <div class="row d-flex justify-content-center my-5">
            <div v-for="(page, index) in pages" :key="index" v-html="page.label" class="col-1 mx-3 py-3 text-white rounded "
                :class="{
                    'bg-primary': !page.active,
                    'bg-secondary': page.active,
                    'd-none': page.url === null
                }" @click="toPage(page.url)">
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
const API_URL = 'http://localhost:8000/api/v1';
export default {
    name: 'Home',
    data() {
        return {
            projects: [],
            pages: []
        }
    },
    methods: {
        toPage(url) {
            axios.get(url)
                .then(res => {
                    const data = res.data.projects;
                    this.projects = data.data;
                    this.pages = data.links;
                })
                .catch(err => console.error(err));
        }
    },
    mounted() {
        axios.get(API_URL + '/projects-page')
            .then(res => {
                // this.projects = res.data.projects;
                const data = res.data.projects;
                this.projects = data.data;
                this.pages = data.links;
            })
            .catch(err => console.error(err));
    }
}
</script>