<template>
  <div class="syllabus-section" :class="[`${meta.Syllabus_Id}`]">
    <p>{{ msg }}</p>
    <FilmComponent
      v-for="film in films"
      :key="film.Film_Id"
      :filmData="film"
      :msg="film.Film_Title"
    />
  </div>
</template>

<script>
import FilmComponent from "./FilmComponent.vue";
export default {
  name: "SyllabusComponent",
  props: ["msg", "films", "meta"],
  components: {
    FilmComponent,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.syllabus-section {
}
</style>
