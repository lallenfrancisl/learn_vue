<template>
  <div>
    <b-nav>
      <b-nav-item disabled><b>Learn Vue Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter {{correctCount}} / {{totalCount}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
  export default {
    props: {
      correctCount: Number,
      totalCount: Number
    }
  }
</script>
