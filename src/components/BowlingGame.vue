<template>
  <div class="hello">
    <h1 style="color: white; font-family: 'Shizuru', cursive">{{ title }}</h1>

    <input type="text" class="name-input" placeholder="Your Name..." />
    <router-link to="/started_" class="start-btn"
      ><b>Let's get started!!!</b></router-link
    >
    <div class="stars-board">
      <h5
        style="
          background-color: brown;
          color: white;
          border-radius: 5px 5px 0 0;
          height: 3vh;
          font-family: 'Rubik', sans-serif;
          padding-bottom: 5px; 
        "
      >
        Stars Board
      </h5>
      <div class="the-white-board"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Title",
  data() {
    return {
      title: "The Bowling Game",
    };
  },
};
</script>
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.start-btn {
  padding: 10px;
  outline: none !important;
  border: none;
  font-size: smaller;
  border-radius: 25px;
  background-color: rgb(148, 212, 214);
  text-decoration: none;
  color: rgb(45, 60, 66);
  font-family: 'Rubik', sans-serif;
}
.start-btn:hover {
  background-color: #5f8675;
  color: white;
}
.stars-board {
  background-color: rgb(207, 207, 207);
  border-radius: 5px;
  height: 60vh;
  max-height: 400px;
  margin-right: 35vw;
  margin-left: 35vw;
  margin-bottom: 3vh;
  display:block;
}
.the-white-board {
  background-color: white;
  min-height: 51vh;
  margin-left: 10px;
  margin-right: 10px;
  border-radius: 5px;
}
.name-input {
  outline: none;
  background-color: rgb(200, 236, 250);
  padding: 3px;
  border-radius: 25px;
  font-family: 'Oswald', sans-serif;
}
@media only screen and (max-width: 1000px) {
  .stars-board {
    background-color: rgb(179, 180, 180);
    border-radius: 5px;
    min-height: 60vh;
    margin-right: 20vw;
    margin-left: 20vw;
    margin-bottom: 4vh;
  }
}
@media only screen and (max-width: 800px) {
  .stars-board {
    background-color: rgb(179, 180, 180);
    border-radius: 5px;
    min-height: 60vh;
    margin-right: 5vw;
    margin-left: 5vw;
    margin-bottom: 4vh;
  }
}
</style>
