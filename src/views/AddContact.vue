<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold">Add Contact</p>
        <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Sed asperiores beatae vel minus tempora dolores eum, harum provident deleniti exercitationem dignissimos? Blanditiis, quo provident consequatur magnam cupiditate excepturi ex consectetur?</p>
      </div>
    </div>
  </div>

  <!-- Spinner -->
  <div class="" v-if="loading">
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <SpinnerComp/>
            </div>
        </div>
    </div>
  </div>

  <!-- Error Message -->
  <div v-if="!loading && errorMessage">
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h4 text-danger fw-bold">{{ errorMessage }}</p>
            </div>
        </div>
    </div>
  </div>

  <div class="container" v-if="!loading">
    <div class="row">
      <div class="col-md-4">
        <form @submit.prevent="submitCreate()">
          <div class="mb-2">
            <input required v-model="contact.name" type="text" class="form-control" placeholder="Name">
          </div>
          <div class="mb-2">
            <input required v-model="contact.photo" type="text" class="form-control" placeholder="Photo URL">
          </div>
          <div class="mb-2">
            <input required v-model="contact.email" type="email" class="form-control" placeholder="Email">
          </div>
          <div class="mb-2">
            <input required v-model="contact.mobile" type="number" class="form-control" placeholder="Mobile">
          </div>
          <div class="mb-2">
            <input required v-model="contact.company" type="text" class="form-control" placeholder="Company">
          </div>
          <div class="mb-2">
            <input required v-model="contact.title" type="text" class="form-control" placeholder="Title">
          </div>
          <div class="mb-2">
            <select required v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
              <option value="">Select Group</option>
              <option :value="group.id" v-for="group of groups" :key="group.id">{{ group.name }}</option>
            </select>
          </div>
          <div class="mb-2">
            <input type="submit" class="btn btn-success" value="Create">
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
import { ContactService } from '@/services/ContactService'
import SpinnerComp from '@/components/SpinnerComp.vue';
    export default {
        components: { SpinnerComp },
        name:"AddContact",
        data : function(){
          return {
            contact: {
              name: '',
              photo: '',
              email:'',
              mobile:'',
              company:'',
              title:'',
              groupId:''
            },
            groups: [],
            loading: false,
            errorMessage : null
          }
        },
        created : async function(){
          try {
            this.loading = true;
            let response = await ContactService.getAllGroups();
            this.groups = response.data;
            this.loading = false;
          } catch (error) {
            this.errorMessage = error;
            this.loading = false;
            console.log(error);
          }
        },
        methods: {
          submitCreate : async function(){
            try {
              this.loading = true;
              let response = await ContactService.createContact(this.contact);
              this.loading = false;
              if(response){
                return this.$router.push(`/`);
              }else{
                return this.$router.push(`/contacts/add`);
              }
            } catch (error) {
              this.errorMessage = error;
              this.loading = false;
              console.log(error);
            }
          }
        }
    }
</script>

<style>

</style>