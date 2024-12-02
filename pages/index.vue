<script lang="ts">


</script>
<template>
  <div class="row">
    <div class="col-md-4">
      <select class="form-select" aria-label="Default select example">
        <option selected>Select ganre</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select>
    </div>
    <div class="col-md-4">
      <select class="form-select" aria-label="Default select example">
        <option selected>Select country</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select>
    </div>
    <div class="col-md-2">
      <select class="form-select" aria-label="Default select example">
        <option value="name">Name</option>
        <option value="year">Year</option>
        <option value="rating">rating</option>
      </select>
    </div>
    <div class="col-md-2">
      <button class="btn btn-outline-warning" type="submit">Reset</button>
    </div>
  </div>
  <div class="row row-cols-1 row-cols-md-3 g-4">
    <div class="col">
      <div class="card h-100">
        <img src="https://avatars.mds.yandex.net/i?id=7f54d3960a09f39fd654f28d45bde440b76dc5b6-5865446-images-thumbs&n=13" class="card-img-top" alt="...">
        <div class="card-body">
          <h5 class="card-title">Миссия невыполнима 2</h5>
          <p class="card-text">4.9</p>
          <p class="card-text">160 min.</p>
          <p class="card-text">Action, Fantastic</p>
          <button class="btn btn-outline-warning bg-dark" type="button">Смотреть</button>
        </div>
        <div class="card-footer">
          <small class="text-body-secondary">Last updated 3 mins ago</small>
        </div>
      </div>
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