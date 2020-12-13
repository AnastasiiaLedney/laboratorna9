<template>
<div v-if="showNewGroupForm" class="whiteBoard">
<form
 v-on:submit.prevent ="addNewGroup" 
 class="newForm" >
  
  <button v-on:click="hide" class="x">X</button> 
<input v-model ="newGroup.OwnerName" id="new1" placeholder="ПІБ"><br>
 <input v-model="newGroup.CarBrand" id="new2" placeholder="Марка "><br>
 <input  v-model="newGroup.CarNumber" id="new3" placeholder="Номер"> <br>
<input  v-model="newGroup.CarColor" id="new4" placeholder="Колір "><br>
 <input type="file" iv-on:change="selectCover" >  
<img :src ="newGroup.Cover" alt="Cover" width="100" height="200" ><br>
<button type="submit" id="bt1">Додати</button>
<button type="reset" id="bt2"> Очистити</button>
</form>
</div> 
</template>
<script>
export default {
  name:"NewGroupForm", 
    data(){
        return{
            showNewGroupForm: false,
            newGroup: this.modelValue,
        }
    },
    props:{
     modelValue: Object
    }, 
    methods:{
        addNewGroup(){
           console.log(this.newGroup);
            this.$emit("update:modelValue", this.newGroup);
            this.hide();
        }, 
        show(){
            this.showNewGroupForm = true;
        },
        hide(){
            this.showNewGroupForm = false;
        },
        selectCover(event){
            const cover = event.target.files[0];
            this.newGroup.Cover = URL.createObjectURL(cover);
        },
       
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
</style>