<template>
  <div class="job-details" :selected="selected">
    <h2>{{ data.title }}</h2>
    <p>Index: {{ data.id - 1 }}</p>
    <p>{{ data.details }}</p>
    <button>Selected</button>
  </div>
</template>

<script>
export default {
  name: "JobDetails",
  props: ["data", "selected", "index"],
};
</script>

<style lang="scss" scoped>
.job-details {
  border: 1px solid black;
  border-radius: 25px;
  max-width: 320px;
  padding: 10px;
  margin: 10px;
  display: flex;
  flex-direction: column;
  white-space: break-spaces;
  background-color: #80808024;
}
.job-details[selected="true"] button {
  background-color: rgb(60, 137, 160);
}
.job-details button {
  height: 40px;
  width: 40%;
  margin: 20px auto;
}
</style>
