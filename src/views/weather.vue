<template>
  <div>
    <div class="weather">
      <video autoplay muted loop class="weather__video">
        <source src="../assets/video/rain.mp4" type="video/mp4" />
        Sorry, your browser doesn't support embedded videos.
      </video>
      <div class="info">
        <div class="info__script">
          <div class="info__sort">
            <div class="info__sort-city">
              New Taipei City
            </div>
            <div class="info__sort-degree">
              5°
            </div>
          </div>
          <div class="info__script-description">
              微微小雨
          </div>
        </div>
        <div class="info__week">
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
        </div>
      </div>
    </div>
  </div>
</template>
