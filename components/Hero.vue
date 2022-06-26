<template>
  <div class="hero">
    <div class="hero__text">
      <h1 class="hero__text-headline">weWatch</h1>
      <a href="#movies" class="hero__text-button">View Movies</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style>
.hero {
  background: url("../assets/IMG_0006.JPG") no-repeat center;
  height: 400pt;
  display: flex;
  justify-content: center;
  align-items: center;
}
.hero__text {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.hero__text-headline {
  font-size: 7em;
  font-weight: 900;
}
.hero__text-button {
  margin-top: 10pt;
  font-size: 1.4em;
  font-weight: 900;
  padding: 7pt 10pt;
  background: crimson;
}
</style>
