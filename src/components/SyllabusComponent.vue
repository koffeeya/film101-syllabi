<template>
  <div class="syllabus-section" :class="[`${meta.Syllabus_Id}`]">
    <SyllabusHeader :meta="meta" :width="imageWidth" />
    <div class="film-area">
      <FilmComponent
        v-for="film in films"
        :key="film.Film_Id"
        :filmData="film"
        :msg="film.Film_Title"
        :imgHeight="imageHeight"
        :imgWidth="imageWidth"
      />
    </div>
  </div>
</template>

<script>
import FilmComponent from "./FilmComponent.vue";
import SyllabusHeader from "./SyllabusHeader.vue";
export default {
  name: "SyllabusComponent",
  props: ["msg", "films", "meta", "syllabi"],
  data() {
    return {
      windowHeight: window.innerHeight * 0.7,
      windowWidth: window.innerWidth * 0.7,
    };
  },
  components: {
    FilmComponent,
    SyllabusHeader,
  },
  computed: {
    imageHeight() {
      const numFilms = Object.keys(this.films).length;
      const imgHeight = this.windowHeight / numFilms;
      return imgHeight;
    },
    imageWidth() {
      const numSyllabi = this.syllabi.length;
      const imgWidth = this.windowWidth / numSyllabi;
      return imgWidth;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.syllabus-section {
  display: grid;
  grid-template-rows: 1fr 6fr;
}
</style>
