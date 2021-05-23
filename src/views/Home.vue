<template>
    <b-container align-v="center" >
    <b-row>
      <job-card v-for="job in displayJobs" :key="job.id" :name="job.name" :id="job.id" ></job-card>  
    </b-row>
      <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      first-text="First"
      prev-text="Prev"
      next-text="Next"
      last-text="Last"
      @input="paginate(currentPage)"
    ></b-pagination>
</b-container>
</template>

<script>
// @ is an alias to /src
import JobCard from '@/components/JobCard.vue';
import {mapGetters} from 'vuex';
export default {
  name: "Home",
  components: {
    JobCard
  },
  computed:{
    ...mapGetters(["jobs","displayJobs","rows"])
  },
  data(){
    return{
      // jobs:[],
      // displayJobs:[],
      currentPage:1,
      // rows:1,
      perPage:3
    }
  },
  mounted(){
    this.fetchData();
  },
  methods:{
    async fetchData(){
     await this.$store.dispatch("fetchJobs");
      console.log('test ',this.$store.getters.jobs);
      // const res = await fetch("jobs.json"); 
      // const val = await res.json();
      // this.jobs = val;
      // this.displayJobs = this.jobs.slice(0,3)
      // this.rows = this.jobs.length
      console.log(this.jobs);
    },
    paginate(currentPage){
      // const start = (currentPage-1) * this.perPage;
      // this.displayJobs = this.jobs.slice(start, start+3) 
      this.$store.dispatch("paginate",{currentPage, perPage:this.perPage})
    }
  }
};
</script>
