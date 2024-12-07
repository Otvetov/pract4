<script setup lang="ts">
import { ref } from "vue";
import { useUserStore } from "~/stores/user";
import {useGenderStore, useReviewStore} from "#imports";
import {use} from "h3";

const userStore = useUserStore();
const reviewStore = useReviewStore();
const genderStore=useGenderStore()
const fio=ref('')
const email=ref('')
const birthday=ref('')
const gender=ref(0)

const readUserData= async ()=>{
  await userStore.fetchUserData()
  fio.value=userStore.userData.fio
  email.value=userStore.userData.email
  birthday.value=userStore.userData.birthday
  gender.value=userStore.userData.gender.id
}
 readUserData()


reviewStore.fetchReviewsByUserId();

const activeTab = ref("profile");

const removeReview = async (id: number) => {
  await reviewStore.removeReview(id)
  alert(`Your review has been removed!`);
};
const ratingStore=useRatingStore()
ratingStore.fetchRatingsByUserId()
const removeRating = async (id: number) => {
  await ratingStore.removeRating(id)
  alert(`Your rating has been removed!`);


};

  const editProfile = async () =>{
    try{
      await userStore.updateProfile({
        fio: fio.value,
        email: email.value,
        birthday: birthday.value,
        gender_id: gender.value,
      });
      await userStore.fetchUserData()
      alert('Edit Profile')
    }catch (error){
    alert(error.message)
  }
}


</script>

<template>
  <div v-if="userStore.userData" class="row">
    <div class="col">
      <h3>Name: {{ userStore.userData.fio }}</h3>
      <h3>COUNT SCORES: {{ userStore.userData.ratingCount }}</h3>
      <h3>COUNT REVIEWS: {{ userStore.userData.reviewCount }}</h3>
    </div>

    <div class="tabs">
      <div
          class="tab"
          :class="{ active: activeTab === 'profile' }"
          @click="activeTab = 'profile'"
      >
        My Profile
      </div>
      <div
          class="tab"
          :class="{ active: activeTab === 'reviews' }"
          @click="activeTab = 'reviews'"
      >
        My Reviews
      </div>
      <div
          class="tab"
          :class="{ active: activeTab === 'scores' }"
          @click="activeTab = 'scores'"
      >
        My Scores
      </div>
    </div>

    <div v-if="activeTab === 'profile'" class="tab-content">
      <p>EMAIL: {{ userStore.userData.email }}</p>
      <p>BIRTHDAY: {{ userStore.userData.birthday }}</p>
      <p>GENDER: {{ userStore.userData.gender.name }}</p>
      <div class="col-6">
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
          Edit
        </button>
        <button type="button" class="btn btn-outline-danger">Delete account</button>
      </div>
    </div>

    <div v-if="activeTab === 'reviews'" class="tab-content">
      <div class="row">
        <div class="col">
          <div v-for="review in reviewStore.userReviews" :key="review.id" class="film-item">
            <div class="film-info">
              <p><strong>Film:</strong> {{ review.film.name }}</p>
              <p><strong>Approved:</strong> {{ review.is_approved ? 'Yes' : 'No' }}</p>
              <p><strong>Date:</strong> {{ review.created_at }}</p>
              <p><strong>Message:</strong> {{ review.message }}</p>
            </div>
            <button @click="removeReview(review.id)" class="remove-btn">Remove</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="activeTab === 'scores'" class="tab-content">
      <div v-for="rating in ratingStore.ratings" :key="rating.id" class="film-item">
        <div class="film-info">
          <p><strong>Film:</strong> {{ rating.film.name}}</p>
          <p><strong>Date:</strong> {{ rating.created_at}}</p>
          <p><strong>Score:</strong> {{ rating.score }}</p>
        </div>
        <button @click="removeRating(rating.id)" class="remove-btn">Remove</button>
      </div>
    </div>
  </div>

  <div v-else class="text-center">
    <div class="spinner-grow" style="width: 3rem; height: 3rem;" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>

  <!-- Button trigger modal -->


  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Modal title</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <form  @submit.prevent="editProfile" id="editProfile" action="">
            <h1 align="center">Edit Profile</h1>
            <div>
              <label for="email" class="form-label">Email address</label>
              <input v-model="email" type="email" class="form-control" id="email" placeholder="name@example.com">
            </div>
            <div>
              <label for="name" class="form-label">Name</label>
              <input v-model="fio" type="name" class="form-control" id="name" >
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
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="submit" form="editProfile" data-bs-dismiss="modal" class="btn btn-primary">Save changes</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.film-item {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.remove-btn {
  background-color: red;
  color: white;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
}

.remove-btn:hover {
  background-color: darkred;
}

.tabs {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  border-bottom: 2px solid #ddd;
}

.tab {
  padding: 10px 20px;
  cursor: pointer;
  border: 1px solid #ccc;
  border-bottom: none;
  background-color: #f9f9f9;
  font-weight: bold;
}

.tab.active {
  border-bottom: 2px solid black;
}

.tab:hover {
  background-color: #e9e9e9;
}

.tab-content {
  margin-top: 20px;
}
</style>
