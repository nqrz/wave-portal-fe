<script setup>

</script>

<template>
  <div class="tooltip">
    <p>
      Learn Web 3 here
    </p>
    <a class="link" href="http://buildspace.so" target="_blank" rel="noopener noreferrer">
      <button class="btn">
        🦄 Buildspace
      </button>
    </a>
  </div>
  <div class="container">
    <figure>
      <img class="image" src="./assets/ijay.jpg" alt="Nizar Baihaqi photo">
    </figure>
    <h1>
      <a title="Go to my website" class="link" href="http://nizarbaihaqi.com" target="_blank" rel="noopener noreferrer">
        Nizar Baihaqi &#8599;
      </a>
    </h1>
    <p>
      Hello folks, I'm Nizar Baihaqi. A Fullstack web dev that always confused which framework to use. 
    </p>
    <p>
      Wave at me and share your thoughts
    </p>
    <form @submit.prevent="handleWave" class="form-container">
      <input class="input" type="text" name="message" id="message" placeholder="Message here">
      <button class="btn" type="submit">
        Wave 👋
      </button>
    </form>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,400;0,700;1,400;1,700&display=swap');

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Noto Sans', sans-serif;
  margin: 0;
  padding: 0;
}

.tooltip {
  position: fixed;
  top: 0;
  right: 0;
  display: flex;
  gap: 10px;
  margin-right: 10px;
}

.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(to bottom left, whitesmoke, lightblue);
}

figure {
  background-image: linear-gradient(to top right, blue, gray, red, yellow);
  width: 150px;
  height: 150px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.image {
  max-width: 140px;
  border-radius: 50%;
}

.link {
  text-decoration: none;
}

.link:hover {
  text-decoration: underline;
}

.form-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 345px;
}

.input {
  box-sizing: border-box;
  padding: 10px;
  border-radius: 4px;
  border: none;
  width: 100%;
}

.btn {
  align-self: end;
  border: none;
  margin-top: 10px;
  padding: 10px;
  padding-top: 8px;
  padding-bottom: 8px;
  border-radius: 4px;
}
</style>
