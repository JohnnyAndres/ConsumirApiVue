<template>
    <div style="margin:10px" >
        <h1 style="text-align:center" >Todos los partidos de tu liga y equipo favoritos</h1>
        <div class="thumbnail" >
            <div v-for="(match, index) in randomMatches" :key="index" class="thumbnail--container">
                <div class='thumbnail--item' >
                    <h3 style="text-align:center" class="thumbnail__item--title" >{{match.title}}</h3>
                    <img v-show="index != showIndex" :src="match.thumbnail" @click="showVideo(index)" class="thumbnail__item--image" alt="">
                    <div v-show="index == showIndex" v-html="match.embed" class="thumbnail__item--video"></div>
                    <h1 class="thumbnail__item--des" >Mira los mejores momentos de {{match.title}} </h1>
                </div>
            </div>    
        </div> 
    </div>
</template>

<script>
export default {
  name: 'Random Results',
  props: ["data"],
  data (){
      return {
          randomMatches:[],
          showIndex:1000,
      }
  },
  methods:{
    getMatches(){
        for (var i= 0; i<=4; i++ ){
            var random = Math.floor(Math.random()*this.data.length)
            this.randomMatches.push(this.data[random])
        }
        console.log(this.randomMatches)
    },
    showVideo(index){
        this.showIndex = index
        console.log(this.showIndex)
    }
  },
  watch: {
    data: function () {
        this.getMatches()
      },
    },
}
</script>

<style scoped>
    .thumbnail{
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        padding: 20px;
    }
    .thumbnail--item{
        border: solid;
        border-radius: 10px;
        margin: 10px;
        display: flex;
        flex-direction:column ;
        align-items: center;
        position: relative;
    }
    .thumbnail__item--video{
        width: 90%;
        height: 248px;
        object-fit: cover;
        overflow:hidden;
        margin: 10px;
    }
    .thumbnail__item--image{
        width: 98%;
        height: 210px;
        overflow:hidden;
        margin: 10px;
    }
    .thumbnail--container:nth-child(5){
        grid-column-start: span 2;
    }
    .thumbnail__item--title{
        margin: 5px 0 0 0;
        font-family:'Times New Roman', Times, serif;
        font-size: 16px;
    }
    .thumbnail__item--des{
        background: linear-gradient(to top, rgba(0,0,0,0.3) 0%, rgba(0,0,0,0) 100%); 
        font-size: 20px;
        color: white;
        opacity: 1;
        padding: 10px; 
        top:0;
        bottom: 0;
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        width: 96%;
        border-radius: 10px;
        color:black;
    }
    @media (min-width: 640px){
        .thumbnail{
        grid-template-columns: repeat(3, 1fr);
        }
        .thumbnail__item--video{
            height: 390px;
        }
        .thumbnail__item--image{
            height: 390px;
        }
    }
    @media (min-width: 1800px){
        .thumbnail{
        grid-template-columns: repeat(3, 1fr);
        }
        .thumbnail__item--video{
            height: 400px;
        }
        .thumbnail__item--image{
            height: 400px;
        }
    }
</style>
