<template>

  <div class="container mt-3">
    <div class="row">
        <div class="col">
           <p class="h3 text-success fw-bold">Contatct Manager
            <router-link to="/contacts/add" class="btn btn-success btn-sm">
                <i class="fa fa-plus-circle"></i>New</router-link>
           </p>   
           <p class="fst-italic">
            The company itself is a very successful company. Let us see that we are to be exempted from what is held, that is to be borne in the tender or The sayings of those who praise him are often criticized, and so on. They do not know the forgiveness of those who are present with the pain of the truth.
           </p>
        
           <div class="row">
                <div class="col-md-8">
                    <form @submit.prevent="getContactByName(searchVal)">
                        <div class="row">
                            <div class="col-md-8">
                                <div class="row">
                                    <div class="col">
                                        <input type="text" v-model="searchVal" class="form-control" placeholder="Search name">
                                    </div>
                                    <div class="col">
                                        <input type="submit" class="btn btn-outline-dark">
                                    </div>
                                </div> 
                            </div>
                        </div>
                    </form>
                    <pre>{{ searchVal }}</pre>
                </div>
                <div class="col-md-4" v-if="loading">
                    <button class="btn btn-primary">
                        <span class="spinner-border spinner-border-sm"></span>
                        Loading..
                    </button>
                </div>
           </div>

        </div>
    </div>
  </div>

<div v-if="loading">
    <div class="container" >
        <div class="row">
            <div class="col">
                <spinner/>
            </div>
        </div>
  </div>
</div>

<div class="mt-3" v-if="!loading && errorMessage ">
    <div class="container" >
        <div class="row">
            <div class="col">
                <div class="h3 text-danger">{{ errorMessage }}</div>
            </div>
        </div>
  </div>
</div>
  <contact-info :contacts="contacts" @deleteContact="deleteContact" />
</template>

<script>
import { ContactServices } from '@/services/ContactServices';
import ContactInfo from '@/components/ContactInfo.vue';
import Spinner from '@/components/Spinner.vue';

export default {
  components: { ContactInfo,Spinner },
    name:"ContactManger",
    data: function(){
          return{
            loading: false,
            contacts:[],
            errorMessage:false,
            searchVal: ''
          }
    },
    created:  function(){
       this.loadData();
    },
    methods:{
        loadData: async function(){
            try{
            this.loading=true;
            let response=await ContactServices.getAllContacts();  
            this.contacts=response.data;
            this.loading=false;
         }catch(error){
             this.errorMessage=error;
             this.loading=false;
         }
        },
        deleteContact: async function(contactId){
            try{
              let response=await ContactServices.deleteContact(contactId);
              if(response){
                 //get fresh data
                this.loading=true;
                let response=await ContactServices.getAllContacts();  
                this.contacts=response.data;
                this.loading=false;
              }
            }catch(error){
                console.log(error);
             this.$router.push('/');
            }
        },
        getContactByName: function (name){
           let res=this.contacts=this.contacts.filter((cont)=>{
          // cont.name.toLowerCase()===name.toLowerCase()
           let val=name.toLowerCase();
          let match=cont.name.toLowerCase().match(val);
          return match;
        });
           if(!res.length>0 || !name.length>0){
            this.loadData();
           }
        }

    }

}
</script>

<style>

</style>