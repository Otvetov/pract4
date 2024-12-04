<script setup lang="ts">
 import {useGenderStore} from "~/stores/gender";
 const errorMessages=ref()
 const genderStore=useGenderStore()
 const authStore=useAuthStore()
 const router=useRouter()
 const fio=ref('')
 const email=ref('')
 const password=ref('')
 const birthday=ref('')
 const gender=ref(0)
 const register=async ()=>{
   if (fio.value
       && email.value
       && password.value
       && gender.value){
     try{
       await authStore.signup({
         fio:fio.value,
         email:email.value,
         password:password.value,
         birthday:birthday.value,
         gender_id:gender.value,
       })
       await router.push('/')
     }catch (e:any){
      errorMessages.value=e.message
     }
   }
 }
</script>

<template>
  <div class="row mt-4">
    <div class="col md-4"></div>
    <div class="col md-4">
      <div class="card">
        <div class="card-body">
          <form @submit.prevent="register" action="">
            <h1 align="center">Registration</h1>
            <div>
              <label for="email" class="form-label">Email address</label>
              <input v-model="email" type="email" class="form-control" id="email" placeholder="name@example.com">
            </div>
            <div>
              <label for="name" class="form-label">Name</label>
              <input v-model="fio" type="name" class="form-control" id="name" >
            </div>
            <div>
              <label for="password" class="form-label">password</label>
              <input v-model="password" type="password" class="form-control" id="password" >
            </div>
            <div>
              <label for="birthday" class="form-label">birthday</label>
              <input v-model="birthday" type="date" class="form-control" id="birthday" >
            </div>

            <label for="gender" class="form-label">gender</label>
            <select v-model="gender" class="form-select" aria-label="Default select example">
              <option value="0"  selected>gender</option>
              <option
                  v-for="gender in genderStore.genders"
                  :key="gender.id"
                  :value="gender.id">{{ gender.name }}</option>
            </select>
            <button class="btn btn-success" type="submit">SingUp</button>
          </form>
          <div v-if="errorMessages"  class="alert alert-danger" role="alert">
            {{errorMessages}}
          </div>
        </div>

      </div>

    </div>

    <div class="col md-4"></div>
  </div>
</template>