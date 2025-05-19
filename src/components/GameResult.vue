<template>
  <div class="result">
    <h2 class="header">Score Board</h2>
    <span>Player <span class="score">{{ correctAnswerCount }}</span> </span> x
    <span>Computer <span class="score">{{ wrongAnswerCount }}</span> </span>
  </div>
  <hr />
</template>

<script>
  export default {
    props: ['correctAnswerCount', 'wrongAnswerCount']
  }
</script>

<style>
  .score {
    border: 1px solid #fff;
    padding: 10px;
    border-radius: 0.2rem;
  }
  .header {
    margin-bottom: 2.5rem;
    margin-top: 0;
  }
  .result {
    margin-bottom: 2rem;
    background-color: #655252;
    color: #fff;
    padding: 3rem;
    text-align: center;
    width: 40%;
    margin: auto;
    border-radius: 1rem;
  }
  @media screen and (max-width: 768px) {
    .result {
      width: 70%;
    }
  }
</style>