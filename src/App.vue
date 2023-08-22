<script>
import ProjectCard from './components/ProjectCard.vue'

import axios from 'axios';

const API = "http://localhost:8000/api/v1"

export default {
  name: 'App',
  components: {

    ProjectCard
  },
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
    axios.get(API + '/projects-page')
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

<template>
  <h1 class="text-center">Projects</h1>
  <div class="row justify-content-center">
    <project-card v-for="project in projects" :key="project.id" :project="project" class="my-3 mx-3" />
    <div class="pages row justify-content-center">
      <div v-for="(page, index) in pages" :key="index" v-html="page.label"
        class="col mx-3 py-2 text-white rounded fs-4 text-center" :class="{
          'bg-primary': !page.active,
          'bg-secondary': page.active,

          'd-none': page.url === null
        }" @click="toPage(page.url)">
      </div>
    </div>
  </div>
</template>

