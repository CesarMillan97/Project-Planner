<template>
  <div class="home">
    <div v-if="projects.length">
      <FilterNav @filterChange="current = $event" :current="current"/>
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @update="handleUpdate" @toggleComplete="handleToggleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'
export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data(){
    return {
      projects:[],
      projectsScope: [],
      current: 'all',
    }
  },
  methods:{
    handleUpdate(id){
      this.projects = this.projects.filter((project ) => {
        return project.id !== id
      })
    },
    handleToggleComplete(id){
      let p = this.projects.find(project => {
        return project.id == id
      })
      p.complete = !p.complete
    }
  },
  computed: {
    filteredProjects(){
      switch(this.current) {
        case 'all':
          return this.projects;
        break;
        case 'completed':
          return this.projects.filter(project => project.complete)
        break;
        case 'ongoing':
          return this.projects.filter(project => !project.complete)
        break;
      }
    }
  },
  updated(){
    console.log('Hello world');
  },
  beforeMount(){
    console.log('Before mount');
  },
  mounted(){
    fetch("http://localhost:3000/projects")
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err)) 
    console.log('Mount');
  },
}
</script>
