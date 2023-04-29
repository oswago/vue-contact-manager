<template>
    <div class="container mt-2" v-if="contacts.length>0" >
        <div class="row">
                    <div class="card my-2 list-group-item-success shadow-lg col-md-6" v-for="contact in contacts" :key="contact.id">
                        <div class="card-body">
                            <div class="row align-items-center">
                                <div class="col-sm-4">
                                <img class="contact-img" :src="contact.photo" alt="#"/>
                                </div>
                                <div class="col-sm-7">
                                <ul class="list-group">
                                    <li class="list-group-item">Name: <span class="fw-bold">{{contact.name}}</span></li>
                                    <li class="list-group-item">Email: <span class="fw-bold">{{contact.email}}</span></li>
                                    <li class="list-group-item">Mobile: <span class="fw-bold">{{ contact.mobile }}</span></li>
                                </ul>
                                </div>
                                <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                                <router-link :to="`/contacts/view/${contact.id}`" class="btn btn-warning my-1">
                                    <i class="fa fa-eye"></i>
                                </router-link>
                                <router-link :to="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1">
                                    <i class="fa fa-pen"></i>
                                </router-link>
                                <button class="btn btn-danger my-1" @click="$emit('deleteContact',contact.id)">
                                    <i class="fa fa-trash"></i>
                                </button>
                                </div>
                            </div>
                        </div>
                    </div>
        </div>
  </div>
</template>

<script>
export default {
    name:"ContactInfo",
    props:{
        contacts: {
            type:Object
        }
    },

}
</script>
