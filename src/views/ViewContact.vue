<template>
  <div class="container mt-3">
    <div class="row">
        <div class="col">
            <p class="h3 text-success fw-bold">View Contact</p>
            <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Optio, dolorum enim doloribus consequatur sapiente pariatur fuga aliquam consequuntur asperiores exercitationem. Quos eos saepe itaque explicabo quo inventore minus asperiores animi.</p>
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

  <div class="container" v-if="!loading && isDone">
    <div class="row align-items-center">
      <div class="col-md-4">
         <img :src="`${contact.photo}`" alt="#" class="contact-img-big"/>
      </div>
      <div class="col-md-6">
        <ul class="list-group">
          <li class="list-group-item">Name: <span class="fw-bold">{{ contact.name }}</span></li>
          <li class="list-group-item">Email: <span class="fw-bold">{{ contact.email }}</span></li>
          <li class="list-group-item">Mobile: <span class="fw-bold">{{ contact.mobile }}</span></li>
          <li class="list-group-item">Company: <span class="fw-bold">{{ contact.company }}</span></li>
          <li class="list-group-item">Title: <span class="fw-bold">{{ contact.title }}</span></li>
          <li class="list-group-item">Group: <span class="fw-bold">{{ group.name}}</span></li>
        </ul>
      </div>
    </div>
    <div class="row mt-3">
        <div class="col">
          <router-link to="/" class="btn btn-success"><i class="fa fa-arrow-alt-circle-left"></i>  Back</router-link>
        </div>
    </div>
  </div>

</template>

<script>
import Spinner from '@/components/Spinner.vue';
import { ContactServices } from '@/services/ContactServices'

export default {
    name: "ViewContact",
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            contact: {},
            errorMessage: null,
            group: {},
            loading: false
        };
    },
    methods: {
        getContactById: async function () {
            try {
                this.loading = true;
                let response = await ContactServices.getContact(this.contactId);
                if (response) {
                    this.loading = false;
                    this.contact = response.data;
                    console.log("conatct:" + this.contact);
                    this.getGroup(this.contact);
                }
                else {
                    this.loading = false;
                    this.errorMessage = "Ooops something went Wrong!";
                    this.$router.push("/");
                }
            }
            catch (error) {
              this.loading = false;
              this.errorMessage = "Ooops something went Wrong!";
                console.log(error);
            }
        },
        getGroup: async function (contact) {
            try {
                console.log("conatcts:" + contact);
                let response = await ContactServices.getGroup(contact);
                if (response) {
                    this.group = response.data;
                }
                else {
                    console.log("Something went wrong!");
                }
            }
            catch (error) {
                console.log(error);
            }
        },
        isDone: function () {
            return Object.keys(this.contact).length > 0 && Object.keys(this.group).length > 0;
        }
    },
    created: async function () {
        await this.getContactById();
    },
    mounted: function () {
        //await this.getGroup();
    },
    components: { Spinner }
}
</script>

<style>

</style>