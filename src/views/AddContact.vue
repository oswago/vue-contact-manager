<template>
  <div class="container mt-3">
    <div class="row">
        <div class="col">
            <p class="h3 text-success fw-bold">Add Contact</p>
            <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Optio, dolorum enim doloribus consequatur sapiente pariatur fuga aliquam consequuntur asperiores exercitationem. Quos eos saepe itaque explicabo quo inventore minus asperiores animi.</p>
        </div>
    </div>
  </div>
  <div class="container mt-3">
     <div class="row">
        <div class="col-md-4">
             <form @submit.prevent="submitCreate()">
                <div class="mb-2">
                   <input required type="text" v-model="contact.name" class="form-control" placeholder="Name"/> 
                </div>
                <div class="mb-2">
                   <input required type="text" v-model="contact.photo" class="form-control" placeholder="Phot url"/> 
                </div>
                <div class="mb-2">
                   <input required type="email"  v-model="contact.email" class="form-control" placeholder="Email"/> 
                </div>
                <div class="mb-2">
                   <input required type="number"  v-model="contact.mobile"  class="form-control" placeholder="Mobile"/> 
                </div>
                <div class="mb-2">
                   <input required type="text"  v-model="contact.company" class="form-control" placeholder="Company"/> 
                </div>
                <div class="mb-2">
                   <input required type="text"  v-model="contact.title" class="form-control" placeholder="Title"/> 
                </div>
                <div class="mb-2">
                   <select required class="form-control"  v-model="contact.groupId" >
                      <option value="">Select Group</option>
                      <option v-for="group in groups" :key="group.id" :value="group.id">{{ group.name }}</option>
                   </select>
                </div>
                <div class="mb-2">
                   <input type="submit"  class="btn btn-success" value="Create"/> 
                </div>
             </form>
        </div>
        <div class="col-md-4">
            <img :src="contact.photo" alt="" class="contact-img">
        </div>
     </div>
  </div>
</template>

<script>
import { ContactServices } from '@/services/ContactServices'
export default {
 name:"AddContact",
 data: function(){
   return{
      contact:{
         name:'',
        company:'',
        title:'',
        mobile:'',
        email:'',
        photo:'',
        groupId:''
      },
      groups:[],
      errorMessage:null

   }
 },
 created: function (){
   this.getAllGroups();
 },

methods:{
    getAllGroups:   async  function(){
         try{
            let respose=await ContactServices.getGroups();
            this.groups=respose.data;
         }catch(error){
            console.log(error);
         }
         },
    submitCreate:async  function(){
         try{
            let response= await ContactServices.createContact(this.contact);
            if(response){
               return this.$router.push('/');
            }else{
               this.errorMessage='Opps Something went wrong!';
               return this.$router.push('/contacts/add');
            }
         }catch(error){
            this.errorMessage=error;
              console.log(error);
         }
    },

    getNone(){
      console.log("nothing");
    }
}
 
}
</script>
