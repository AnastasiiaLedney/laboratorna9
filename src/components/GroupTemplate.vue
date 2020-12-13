<template>
 
        <li >

    <h2>{{group.OwnerName}}</h2>
    <p class="CarBrand"> Марка: {{group.CarBrand}}</p>
    <p class="CarNumber"> Номер: {{group.CarNumber}}</p>
    <p class="CarColor"> Колір: {{group.CarColor}}</p>
    <img  alt="Cover" :src="group.Cover" class="cover">

  </li>
    
</template>
<script>
export default {
    name:"GroupTemplate",
  props:{
      group: Object
  },
  data(){
      return{
      }
  }   
}
</script>
<style scoped>
.cover{
  position: absolute;
  width: 170px;
  height: 170x;
  top: 60%;
  right: 30%;
}

.CarNumber{
   font-family: "Comic Sans MS", cursive, sans-serif;
     font-size: 20px;
     color:  black;
}
.CarBrand{
  font-family: "Comic Sans MS", cursive, sans-serif;
     font-size: 20px;
     color:  black;
}
.groupVue{
position: absolute;
border-radius: 10px 60px 50px;
width: 400px;
height: 320px;
position: relative;
background: #778899;
text-align: center;
margin: 10px;
}

.CarColor{
  font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 20px;
  color:  black;
}
h2
{
   font-family: "Arial Black", Gadget, sans-serif;
   font-size: 20px;
   color:  black;
}

 li
 {
    float: left;
    list-style: none;
 }
</style>