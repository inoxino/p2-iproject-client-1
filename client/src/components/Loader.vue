<template>
  <div id="preloader" style="">
    <div class="loading">
      <lottie-player
        src="https://assets9.lottiefiles.com/packages/lf20_Tprkoc.json"
        background="transparent"
        speed="1"
        style="width: 300px; height: 300px"
        loop
        autoplay
      ></lottie-player>
    </div>
  </div>
</template>

<style>
#preloader {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
  background-color: #fff;
  opacity: 0.8;
}

#preloader .loading {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>
