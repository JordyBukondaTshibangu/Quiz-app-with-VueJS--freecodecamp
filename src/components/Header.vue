<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Quiz App - Test Your Brain </b-nav-item>
      <b-nav-item disabled>Total Score : {{ numberCorrect}}/{{ numberTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  props : {
    numberCorrect : Number,
    numberTotal : Number
  }
}
</script>
