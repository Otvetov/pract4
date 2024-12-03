<script setup lang="ts">
import {useCategoryStore} from "~/categories/catogory";
import {useCountryStore} from "~/stores/countre";

const categoryStore=useCategoryStore()
const filmStore = useFilmStore();
const countryStore=useCountryStore()

const category=ref(null)
const country=ref(null)
const sort=ref('name')
const filter= () =>{
  filmStore.addCategoryToParams(category.value);
  filmStore.addCountryToParams(country.value);
  filmStore.addSortToParams(sort.value);
  filmStore.fetchFilms();
}
const resetFilter=()=>{
  category.value=null
  country.value=null
  sort.value='name'
  filter()
}

</script>
<template>
  <div class="row">
    <div class="col-md-4">
      <select v-model="category" @change="filter" class="form-select" aria-label="Default select example">
        <option :value="null" selected>Select ganre</option>
        <option v-for="category in categoryStore.categories" :key="category.id" :value="category.id">{{category.name}} ({{category.filmCount}})</option>
      </select>
    </div>
    <div class="col-md-4">
      <select v-model="country" @change="filter" class="form-select" aria-label="Default select example">
        <option selected :value="null">Select country</option>
        <option v-for="country in countryStore.countries"
                :key="country.id"
                :value="country.id">{{country.name}}</option>
      </select>
    </div>
    <div class="col-md-2">
      <select v-model="sort" @change="filter" class="form-select" aria-label="Default select example">
        <option value="name">Name</option>
        <option value="year">Year</option>
        <option value="rating">rating</option>
      </select>
    </div>
    <div class="col-md-2">
      <button @click="resetFilter" class="btn btn-outline-warning" type="submit">Reset</button>
    </div>
  </div>
  <div v-if="!filmStore.isLoading" class="row row-cols-1 row-cols-md-3 g-4">
    <div class="col" v-for="film in filmStore.films" :key="film.id">
      <div class="card h-100">
        <img v-if="film.link_img" :src="film.link_img" class="card-img-top" alt="...">
        <img v-else src="https://cdn1.ozone.ru/s3/multimedia-i/6838746030.jpg" class="card-img-top" alt="...">
        <div class="card-body">
          <h5 class="card-title">{{film.name}}</h5>
          <p class="card-text">{{film.ratingAvg}}</p>
          <p class="card-text">{{film.duration}} min.</p>
          <p class="card-text"></p>
          <template v-for="category in film.categories" :key="category.id">
            {{category.name}}
          </template>
        </div>
        <div class="cart-footer">
          <button class="btn btn-outline-primary mt-3 " type="submit">смотреть</button>
        </div>
      </div>
    </div>



  </div>
  <div v-else class="text-center mt-5">
    <div class="spinner-border" style="width: 3rem; height: 3rem;" role="status">
      <span  class="visually-hidden">Loading...</span>
    </div>
  </div>
  <nav class="d-flex justify-content-center mt-4" aria-label="Page navigation example">
    <ul class="pagination">
      <li class="page-item">
        <a class="page-link" href="#" aria-label="Previous">
          <span aria-hidden="true">&laquo;</span>
        </a>
      </li>
      <li class="page-item"><a class="page-link" href="#">1</a></li>
      <li class="page-item"><a class="page-link" href="#">2</a></li>
      <li class="page-item"><a class="page-link" href="#">3</a></li>
      <li class="page-item">
        <a class="page-link" href="#" aria-label="Next">
          <span aria-hidden="true">&raquo;</span>
        </a>
      </li>
    </ul>
  </nav>


</template>