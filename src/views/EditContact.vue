<template>
  <div class="container mt-3">
    <div class="row">
        <div class="col">
            <p class="h3 text-success fw-bold">Edit Contact</p>
            <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Optio, dolorum enim doloribus consequatur sapiente pariatur fuga aliquam consequuntur asperiores exercitationem. Quos eos saepe itaque explicabo quo inventore minus asperiores animi.</p>
        </div>
    </div>
  </div>
  <div class="container mt-3">
     <div class="row">
        <div class="col-md-4">
             <form @submit.prevent="submitUpdate()">
                <div class="mb-2">
                   <input required type="text" v-model="contact.name" class="form-control" placeholder="Name"/> 
                </div>
                <div class="mb-2">
                   <input type="text" required v-model="contact.photo" class="form-control" placeholder="Phot url"/> 
                </div>
                <div class="mb-2">
                   <input type="email" required v-model="contact.email" class="form-control" placeholder="Email"/> 
                </div>
                <div class="mb-2">
                   <input type="number" required v-model="contact.mobile" class="form-control" placeholder="Mobile"/> 
                </div>
                <div class="mb-2">
                   <input type="text" required v-model="contact.company" class="form-control" placeholder="Company"/> 
                </div>
                <div class="mb-2">
                   <input type="text" required v-model="contact.title" class="form-control" placeholder="Title"/> 
                </div>
                <div class="mb-2">
                   <select required v-model="contact.groupId" class="form-control">
                      <option value="">Select Group</option>
                      <option v-for="group in groups" :key="group.id" :value="group.id" >{{ group.name }}</option>
                   </select>
                </div>
                <div class="mb-2">
                   <input type="submit" class="btn btn-success" value="Update"/> 
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
 name:"EditContact",
 data: function(){
   return{
      contactId: this.$route.params.contactId,
      contact: {
         "name":'',
         "company":'',
         "title":'',
         "photo":'',
         "mobile":'',
         "email":'',
         "groupId":''
      },
      errorMessage: null,
      groups: {}
   }
 },
 methods:{
     getContact: async function(){
      try{
         let response=await ContactServices.getContact(this.contactId);
         if(response){
            this.contact=response.data;
         }else{
          this.errorMessage='Oops no contact with given id';
          this.$router.push('/');
         }
      }catch(error){
         this.errorMessage=error;
         console.log(error);
      }
     },
     getAllGroups: async function(){
      try{
            let respose=await ContactServices.getGroups();
            this.groups=respose.data;
         }catch(error){
            console.log(error);
         }
     },
     submitUpdate: async function(){
      try{
         console.log("contact:"+this.contact);
            let response= await ContactServices.updateContact(this.contact,this.contactId);
            if(response){
               return this.$router.push('/');
            }else{
               this.errorMessage='Opps Something went wrong!';
               console.log(this.errorMessage);
               return this.$router.push(`/contacts/view/${this.contactId}`);
            }
         }catch(error){
            this.errorMessage=error;
              console.log(error);
         }
     }
 },
 created: function(){
    this.getContact();
    this.getAllGroups();
 }
}
</script>

<style>

</style>