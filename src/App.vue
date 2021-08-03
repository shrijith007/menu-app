<template>
<body>
<div class="wrapper">
    <div class="sidebar">
        <h2>Selection Menu</h2>
        <div v-for="element in rd" :key="element.id">
        <ul>
            <li><button  @click="togglecommon(element.category)">{{element.category.toUpperCase()}}</button><p>Results found: {{element.restaurantList.length}}</p></li>
        </ul>
        </div>
        <div>
        <ul>
           <li><button  @click="togglecommon('Swiggy Xclusive')">Swiggy Xclusive</button><p>Results found: {{finalonlyswiggy.length}}</p></li>
            <li><button  @click="togglestoallrestaurants('See All Restaurants')">See All Restaurants</button><p>Results found: {{finalallrestaurant.length}}</p></li>
        </ul>
        </div> 
    </div>

<div class="main_content">
<basecard v-if='switchforallrestaurant'>
<section id="See All Restaurants">

<p >
  <Seeall
  :restaurants="finalallrestaurant"
  ></Seeall>
</p>
</section>
</basecard>


<basecard v-if='switchforcommon'>
<section id="popular brands"> 
 
<p >
 <Popularbrands
  :restaurants="popularbrands"
  ></Popularbrands>
</p>
</section>
</basecard>

<basecard v-if='switchforcommon'>
<section id="offers near you"> 
<p >
  <Offernu
  :restaurants="offersforu"
  ></Offernu>
</p>
</section>
</basecard>

<basecard v-if='switchforcommon'>
<section id="Express delivery">
<p >
<Expressdelivery
  :restaurants="expressdelivery"
  ></Expressdelivery>
</p>
</section>
</basecard>

<basecard v-if='switchforcommon'>
<section id="Gourmet">
<p >
  <Gourmet
  :restaurants="gourmet"
  ></Gourmet>
</p>
</section>
</basecard>


<basecard v-if='switchforcommon'>
<section id="Swiggy Xclusive">

  <Swiggyx
  :restaurants="finalonlyswiggy"
  ></Swiggyx>

</section>
</basecard>


</div>
</div>

</body>


</template>

<script>
import Expressdelivery from "./Cards/Eo.vue"
import Gourmet from "./Cards/Gm.vue"
import Offernu from "./Cards/Onu.vue"
import Popularbrands from "./Cards/Pb.vue"
import Seeall from "./Cards/Sa.vue"
import Swiggyx from "./Cards/Sx.vue"

export default {
  components: {Expressdelivery,Gourmet,Offernu,Popularbrands,Seeall,Swiggyx},

data(){
  return{
  selectedoption:"",
  tempitems:{},
  name:[],
  tempallrestaurant:[],
  gourmet:[],
  expressdelivery:[],
  offersforu:[],
  popularbrands:[],
  allrestaurant:[],
  onlyswiggy:[],
  finalallrestaurant:[],
  finalonlyswiggy:[],
  switchforcommon:true,
  switchforallrestaurant:false,
  seall:"See All Restaurants",
  so:"Swiggy Xclusive",
  tempitemswithid:{}
  }
},

computed:{
  rd(){
  Object.assign(this.tempitems,this.$store.state.data)
  this.fetchallrestaurant(this.tempitems)
  this.fetchonlyswiggy(this.tempitems)
 this.assigning_particular_category(this.tempitems)
  return this.tempitems
  },
  
},
methods: {


    togglecommon(id) {  
      this.scroll(id)
  this.switchforcommon=true,
  this.switchforallrestaurant=false
    },
    togglestoallrestaurants(id) {   
 this.scrolltop(id)
 this.switchforcommon=false,
this.switchforallrestaurant=true

    
    },
    createexclusive(){
      this.exclusives.push(this.seeall())
      return this.exclusives
    },
    fetchallrestaurant(element){
      for (const key1 in element){
        for (const key2 in element[key1].restaurantList){
          this.allrestaurant.push(element[key1].restaurantList[key2])
        } 
      }
      this.finalallrestaurant=this.allrestaurant
      
   },
 
 fetchonlyswiggy(element){
  for (const key1 in element){
        for (const key2 in element[key1].restaurantList){
          if(element[key1].restaurantList[key2].isExlusive==true){
            this.onlyswiggy.push(element[key1].restaurantList[key2])
          }
        }
      }
      this.finalonlyswiggy=this.onlyswiggy
      
 },
 scroll(id){
   if(this.switchforallrestaurant!=true)
         document.getElementById(id).scrollIntoView({
        behavior: "smooth"
      });
 },
 scrolltop(id){
   if(this.switchforallrestaurant===true)
   document.getElementById(id).scrollIntoView({
        behavior: "smooth"
      });

 },
 giveid(element){
   for(const key1 in element){
  for (const key2 in element[key1]['restaurantList']){
    element[key1]['restaurantList'][key2]["id"]=key2
  }
   }
   return element
 },
 
  assigning_particular_category(objects){
    for(const key in objects){
      if(objects[key]['category']=="Gourmet"){
        for(const keylocal in objects[key]['restaurantList']){
          this.gourmet.push(objects[key]['restaurantList'][keylocal])
        }
      }
      if(objects[key]['category']=="popular brands"){
 for(const keylocal in objects[key]['restaurantList']){
          this.popularbrands.push(objects[key]['restaurantList'][keylocal])
        }
      }
      if(objects[key]['category']=="offers near you"){
        for(const keylocal in objects[key]['restaurantList']){
          this.offersforu.push(objects[key]['restaurantList'][keylocal])
        }
      }
      if(objects[key]['category']=="Express delivery"){
      for(const keylocal in objects[key]['restaurantList']){
          this.expressdelivery.push(objects[key]['restaurantList'][keylocal])
        }
      }
    }



  },

inter(enter){
  if (enter){
    console.log(true)
  }
  else{
    console.log(false)
  }
}
    
    
    
    }



}

</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Josefin+Sans&display=swap');

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  list-style: none;
  text-decoration: none;
  font-family: 'Josefin Sans', sans-serif;
}

body{
   background-color: #f3f5f9;
}

.wrapper{
  display: flex;
  position: relative;
}

.wrapper .sidebar{
  width: 210px;
  height: 100%;
  background: rgb(223, 242, 245);
  padding: 30px 0px;
  position: fixed;
  border-radius: 20px;
}

.wrapper .sidebar h2{
  color: #fff;
  text-transform: uppercase;
  text-align: center;
  margin-bottom: 30px;
}

.wrapper .sidebar ul li{
  padding: 25px;
  border-bottom: 10px solid black;
  border-bottom: 10px solid rgba(0,0,0,0.05);
  border-top: 0px solid black(255,255,255,0.05);
  border-radius: 20px;
  color: black;
}    

.wrapper .sidebar ul li button{
 color: black;
 display: block;
 width: 10rem;
 height: 2rem ;
 border-radius: 5px;
 font-size:15px ;
 border:none

}
.wrapper .sidebar ul li button:focus{
 color: rgb(123, 222, 235);
}

.wrapper .sidebar ul li button.active{
 color: grey;
}

.wrapper .sidebar ul li button:hover{
 color: rgb(123, 222, 235);
}


 
.wrapper .main_content{
  width: 100%;
  margin-left: 200px;
}

.wrapper .main_content .header{
  padding: 20px;
  background: #fff;
  color: #717171;
  border-bottom: 1px solid #e0e4e8;
}

.wrapper .main_content .info{
  margin: 20px;
  color: #717171;
  line-height: 25px;
   margin: 50px;
}

.wrapper .main_content .info div{
  margin-bottom: 20px;
 
}

h1{
padding: 10px;
}
p{
  padding: 5px;
}
</style>