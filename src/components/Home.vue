<template>
    <div>
        <header class="header" >
            <div class="header__left" >
                <img class="header__img" src="../assets/balon.png" alt="logo">
                <a class="home__button" href="/">INICIO</a>
            </div>
            <div class="header__right" >
                <select v-model="selectedLeague" @change="getLeagueMatches()" >
                        <option :value="null" disabled>Seleccione una liga</option>
                        <option v-for="(name, index) in leagues" :key="index" > {{name}} </option>
                </select>
                <select v-model="selectedTeam" @change="getTeamMatches()" >
                        <option :value="null" disabled >Seleccione un equipo</option>
                        <option v-for="(team, index) in teams" :key="index" > {{team}} </option>
                </select>
            </div>
        </header>
        <RandomResults v-if="showHomeMatches" :data = data />
        <ResultsByLeague v-if="showMatchesByLeague" :data = data :selectedLeague = selectedLeague />
        <ResultsByTeam v-if="showMatchesByTeam" :data = data :selectedTeam = selectedTeam />
    </div>
</template>

<script>
import RandomResults from './RandomResults'
import ResultsByLeague from './ResultsByLeague'
import ResultsByTeam from './ResultsByTeam'

import axios from 'axios'

export default {
  name: 'Results',
  data (){
      return {
          selectedLeague:null,
          selectedTeam:null,
          showHomeMatches:true,  
          showMatchesByLeague:false,  
          showMatchesByTeam:false,
          data: [],
          leagues:[],
          teams:[],
      }
  },
  components: {
    RandomResults,
    ResultsByLeague,
    ResultsByTeam
  },
  methods:{
      getLeagueMatches(){
        this.showHomeMatches = false
        this.showMatchesByLeague = true
        this.showMatchesByTeam = false
      },
      getTeamMatches(){
        this.showHomeMatches = false
        this.showMatchesByLeague = false
        this.showMatchesByTeam = true
      }
  },
  mounted () {
    axios
        .get('https://www.scorebat.com/video-api/v1/')
        .then((response) => {
            this.data = response.data
            this.leagues = this.data.map((match)=>{
                return match.competition.name
            })
            this.leagues = this.leagues.filter((item,index)=>{
                return this.leagues.indexOf(item) === index;
            })
            for (var match of this.data){
                this.teams.push(match.side1.name)
                this.teams.push(match.side2.name)
            }
            this.teams = this.teams.filter((item,index)=>{
                return this.teams.indexOf(item) === index;
            }) 
            console.log(this.data)
        })   
        
    },
}
</script>

<style scoped>
    .header {
        width : 100%; 
        height: 80px;
        background-color: #16697a;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .home__button{
        color: white;
        text-decoration: none;
        font-family:'Times New Roman', Times, serif;
        font-size: 16px;
        margin-left:10px ;
    }    
    .header__img{ 
        width: 70px;
        margin: 0px 20px 0px 10px;
    }
    .header__left{
        display: flex;
        align-items: center;
    }
    .header__right{
        margin: 10px;
    }
    .header__right select {
        margin: 0px 10px 0px 0px;
        width: 220px;
        height: 50px;
        padding: 2px;
        background: #16697a;
        color: white;
        font-family:'Times New Roman', Times, serif;
        font-size: 16px;
        border: none;
    }
</style>