<script setup lang="ts">
import {fi} from "cronstrue/dist/i18n/locales/fi";
import {useReviewStore} from "~/stores/review";
import RatingStars from "~/components/RatingStars.vue";

const filmStore = useFilmStore();
const reviewsStore = useReviewStore();
const route = useRoute();
const authStore = useAuthStore();
filmStore.fetchFilmById(route.params.id);
reviewsStore.fetchReviewsByFilmId(route.params.id);
const message = ref('');
const sendReview = async () => {
  if (message.value) {
    await reviewsStore.addReview({
      film_id: route.params.id,
      message: message.value

    });
  }
}

</script>

<template>
  <div v-if="filmStore.film" class="container mb-4">
    <div class="row">
      <div class="card mb-4">
        <div class="d-flex justify-content-center">

          <iframe width="560" height="315" :src="filmStore.film.link_video" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
          <div class="col-md-2">
              <RatingStars v-if="authStore.authData" :film-id="filmStore.film.id"/>

          </div>
        </div>

        <div class="card-body">
          <h5 class="card-title">{{filmStore.film.name}}</h5>
          <p class="card-text">Rating:{{filmStore.film.ratingAvg}} ★ </p>
          <p class="card-text">Duration:{{filmStore.film.duration}} min.</p>
          <p class="card-text">Country:{{filmStore.film.country.name}}</p>
          <p class="card-text">Year:{{filmStore.film.year_of_issue}}</p>
          <p class="card-text">Age Restriction:{{filmStore.film.age}}+</p>
          <p class="card-text">Add Date:{{filmStore.film.created_at}}</p>
          <p class="card-text">
            <p class="card-text">
              Genres:
              <template v-for="category in filmStore.film.categories"
                        :key="category.id">
                {{category.name}}
              </template>
            </p>
          </p>
          <a href="_blank">KinoPoisk</a>
        </div>
      </div>
    </div>

<div v-if="authStore.authData" >
    <div class="col-12 mt-4">
      <h5>Add review</h5>
      <textarea v-model="message" class="form-control mb-3" placeholder="Your message"></textarea>
      <div ><button @click="sendReview"  class="btn btn-primary">Submit</button></div>
    </div>
</div>

    <div v-else>
      <p>Authorization for add review</p>
    </div>


    <div>
      <div>
        <h4>Reviews</h4>
        <div v-for="review in reviewsStore.reviews" :key="review.id" class="border-bottom py-2">
          <h5>{{ review.user.fio }}</h5>
          <p>{{ review.message }}</p>
          <small class="text-muted">{{ review.created_at }}</small>
        </div>
      </div>
    </div>
  </div>



  <div v-else class="text-center mt-5">
    <div class="spinner-border" style="width: 3rem; height: 3rem;" role="status">
      <span  class="visually-hidden">Loading...</span>
    </div>
  </div>
</template>
<style scoped>
.container {
  font-family: Arial, sans-serif;
  margin: 20px;
}

.card {
  border: 1px solid #ccc;
  border-radius: 5px;
}


.text-muted {
  color: #ccc;
}

textarea {
  resize: none;
}

button {
  cursor: pointer;
}
</style>