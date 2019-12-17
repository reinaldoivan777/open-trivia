<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Fancy Quiz</b></b-nav-item>
      <b-nav-item disabled
        ><b>{{ numberCorrect }}/{{ numberTotal }}</b></b-nav-item
      >
    </b-nav>
  </div>
</template>
<script>
export default {
  props: ["numberCorrect", "numberTotal"]
};
</script>
