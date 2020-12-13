<template>

    {{selected}}
  <input type="button" id="bt1"  value="Додати" v-on:click="showForm">
  <input type="button" id="bt2"  value="Редагувати" v-on:click="showEditForm">
  <input type="button" id="bt3"  value="Видалити" v-on:click="deleteGroup">
   <input type="text"  id="fnd" placeholder="Шукати за номером " v-model="searchCarNumberString">

  <div class="wrap">
  <new-group-form
   v-model = "newGroup"
   @submit.prevent="addNewGroup"
    ref="newGroupForm">
    </new-group-form>
<form v-on:submit.prevent = "editSelectedGroup" class="newForm" v-if="ShowEditGroupForm">
  <button v-on:click="closeForm" class="x">X</button> 
   
<input v-model.lazy ="editGroup.OwnerName" placeholder="ПІБ" id="new1"><br>
 <input v-model.lazy="editGroup.CarBrand" placeholder="Марка" id="new2"><br>
 <input  v-model.lazy="editGroup.CarNumber" placeholder="Номер" id="new3"> <br>
 <input  v-model.lazy="editGroup.CarColor" placeholder="Колір" id="new4">
<button type="submit" id="b1">Редагувати</button>
<button type="reset" id="b2">Очистити</button>
</form>
<ul>
  <group-template 
  v-for="b in filteredGroups" 
  :key="b.Id" 
  class="groupVue"
   v-on:click ="selectGroup(b.Id)" 
   v-bind:group="b">

   

  </group-template>
</ul>
</div>

</template>

<script>
import GroupTemplate from "./components/GroupTemplate.vue";
import NewGroupForm from "./components/NewGroupForm.vue";
export default
{
  name:"App",
  components:{
GroupTemplate,
NewGroupForm
  },
  data()
  { return{
    searchCarNumberString:"",
    selected:-1,
    ShowNewGroupForm: true,
    ShowEditGroupForm: false,
   
   groups:[
     { 
       Id:4454545,
      OwnerName:"Грись Василь Васильович",
      CarBrand:"BMW",
      CarNumber: "АО 5654 ОО",
      CarColor:"Білий",
      Cover:"data:image/jpeg;base64,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",
     },
     {
        Id:124578,
      OwnerName:"Думнич Іван Михайлович",
      CarBrand:"Mercedes",
      CarNumber: "АА 6684 ОО",
      CarColor:"Червоний",
      Cover:"https://img1.goodfon.ru/wallpaper/nbig/a/c6/avto-mazda-concept-krasnyy-na.jpg",
     },
     {
       Id:12157854,
      OwnerName:"Розман Марія Михайлівна",
      CarBrand:"Nissan",
      CarNumber: "АО 1114 КО",
      CarColor:"Чорний",
      Cover:"https://nastol.net/wallpaper/big/1/3736813-vnedorozhnik-nissan-belyy-fon-ikstreyl-krossover.jpg",
      
     }
   ],
   newGroup:
      { Id:"",
        OwnerName:"",
        CarBrand:"",
        CarNumber:"",
        CarColor:"",
        Cover:""
      },
 
  };
  },
methods:{
   
  addNewGroup()
  { console.log(this.newGroup);
    let newGroupCopy = Object.assign({},this.newGroup);
    newGroupCopy.Id = parseInt(Date.now());
    this.groups.push(newGroupCopy);
    this.ShowNewGroupForm=false;
  },
   showForm()
   {
     this.$refs.newGroupForm.show();
   },
    selectGroup(id)
    {
      this.selected= id;
    },
    showEditForm()
    {
      if(this.selected>=0){
        let index= this.groups.findIndex(group=>group.Id == this.selected);
      this.editGroup = this.groups[index];
      this.ShowEditGroupForm= true;
      }
      else alert("Оберіть об'єкт");
    }, 
    editSelectedGroup()
    {
      this.ShowEditGroupForm = false;
    },

    deleteGroup()
    {
      let index= this.groups.findIndex(group=>group.Id == this.selected);
      if(this.selected>=0){
      this.groups.splice(index,1);
      }
    },
    closeForm()
    {
      this.ShowNewGroupForm = false;
    },
    
   
},
computed:
{
sortedGroups()
{ function CompareGroups(group1,group2){

if(group1.CarBrand>group2.CarBrand){
return 1;
}
if(group1.CarBrand<group2.CarBrand)
 
   return -1;
   
 if(group1.CarColor>group2.CarColor)
 
   return 1;
   

 if(group1.CarColor<group2.CarColor)
 
   return 1;
 
 
   return 0;
 

}
  return [...this.groups].sort(CompareGroups);
},
filteredGroups()
{
  if(this.searchCarNumberString=="")
  return this.sortedGroups;
  return this.sortedGroups.filter(b=>b.CarNumber.includes(this.searchCarNumberString));
},
selectedIndex(){
  if(this.selected>0)
  return  this.groups.findIndex(group=>group.Id == this.selected);
   return -1;
}
}
}
</script>

<style scoped>
.x
{
 position: absolute;
 top: 1%;
 right: 0%;
 background: rgb(255, 60, 0);
  color: #fff;
  font-family: 'Helvetica', 'Arial', sans-serif;
  font-size: 2em;
  font-weight: bold;
  text-align: center;
  width: 40px;
  height: 40px;
  border-radius: 6px;
}
 
#fnd
{
  background:#EEE8AA;
  color:black;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
   font-family: "Comic Sans MS", cursive, sans-serif;
     font-size: 20px;
     color:  black;
}
 #new1
 {
   background:#EEE8AA;
  color:black;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
  font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 20px;
  color:  black;
  
 }
  #new2
 {
   background:#EEE8AA;
  color:black;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
  font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 20px;
  color:  black;
  
 }
  #new3
 {
   background:#EEE8AA;
  color:black;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
  font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 20px;
  color:  black;
  
 }
  #new4
 {
   background:#EEE8AA;
  color:black;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
  font-family: "Comic Sans MS", cursive, sans-serif;
  font-size: 20px;
  color:  black;
  
 }


#bt1{
  background:#FFA500;
  color:#fff;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
}
#bt1:hover{
  background:#fff;
  color:#808000;
}
#bt1:before,#bt1:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:0;
  background: #1AAB8A;
  transition:400ms ease all;
}
#bt1:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
}
#bt1:hover:before,#bt1:hover:after{
  width:100%;
  transition:800ms ease all;
}
#bt2{
  background:#FFA500;
  color:#fff;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
}
#bt2:hover{
  background:#fff;
  color:#808000;
}
#bt2:before,#bt2:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:0;
  background: #1AAB8A;
  transition:400ms ease all;
}
#bt2:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
}
#bt2:hover:before,#bt2:hover:after{
  width:100%;
  transition:800ms ease all;
}
#bt3{
  background:#FFA500;
  color:#fff;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
}
#bt3:hover{
  background:#fff;
  color:#808000;
}
#bt3:before,#bt3:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:0;
  background: #1AAB8A;
  transition:400ms ease all;
}
#bt3:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
}
#bt3:hover:before,#bt3:hover:after{
  width:100%;
  transition:800ms ease all;
}
#b1{
  background:#FFA500;
  color:#fff;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
}
#b1:hover{
  background:#fff;
  color:#808000;
}
#b1:before,#b1:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:0;
  background: #1AAB8A;
  transition:400ms ease all;
}
#b1:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
}
#b1:hover:before,#b1:hover:after{
  width:100%;
  transition:800ms ease all;
}
#b2{
  background:#FFA500;
  color:#fff;
  border:none;
  position:relative;
  height:40px;
  font-size:1.1em;
  padding:0 2em;
  cursor:pointer;
  transition:800ms ease all;
  outline:none;
}
#b2:hover{
  background:#fff;
  color:#808000;
}
#b2:before,#b2:after{
  content:'';
  position:absolute;
  top:0;
  right:0;
  height:2px;
  width:0;
  background: #1AAB8A;
  transition:400ms ease all;
}
#bt2:after{
  right:inherit;
  top:inherit;
  left:0;
  bottom:0;
}
#bt2:hover:before,#bt2:hover:after{
  width:100%;
  transition:800ms ease all;
}

 .whiteBoard
 {
   width:1000px;
    height: 1000px;
    background: rgba(255, 255, 255, 0.342);
    position: absolute;
    z-index: 9;
    top: 50%;
    left: 20%;
 }

  


form{
position: fixed;
z-index: 10;
background: #EEE8AA;
margin: 100px auto;
width: 50%;
height: 40%;
}
.wrap{
  position: relative;
}

</style>