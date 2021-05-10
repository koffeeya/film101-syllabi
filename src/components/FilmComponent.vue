<template>
  <div
    class="film-section"
    @mouseover="
      highlightFilms(
        `${filmData.Film_Id}`,
        `${filmData.Syllabus_Id}`,
        `${filmData.Section_Id}`
      )
    "
    @mouseout="
      highlightFilmsReset(
        `${filmData.Film_Id}`,
        `${filmData.Syllabus_Id}`,
        `${filmData.Section_Id}`
      )
    "
  >
    <div v-if="hover" class="section-title">
      {{ filmData.Film_Title }}
      <br />
      <br />
      {{ filmData.Syllabus_School }}
      <br />
      <br />
      {{ filmData.Syllabus_Number }} ({{ filmData.Syllabus_Year }})
      <br />
      <br />
      {{ filmData.Section_Name }}
    </div>
    <div
      class="poster-wrapper"
      :class="[`${filmData.Syllabus_Id}`, `${filmData.Film_Id}`]"
    >
      <div class="image-wrapper">
        <img
          class="poster"
          :src="posterUrl"
          :style="{ height: imgHeight + 'px', width: imgWidth + 'px' }"
        />
      </div>
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "FilmComponent",
  props: ["msg", "filmData", "imgHeight", "imgWidth"],
  data() {
    return {
      hover: false,
    };
  },
  methods: {
    highlightFilms(className, syllabusName, sectionName) {
      this.hover = true;
      d3.selectAll(`.${className} > div > img`)
        .transition()
        .duration(100)
        .style("filter", "grayscale(0%) brightness(100%)");
      //d3.selectAll(`.${syllabusName}`).style("z-index", "200");
    },
    highlightFilmsReset(className, syllabusName, sectionName) {
      this.hover = false;
      d3.selectAll(`.${className} > div > img`)
        .transition()
        .duration(100)
        .style("filter", "grayscale(100%) brightness(45%)");
      //d3.selectAll(`.${syllabusName}`).style("z-index", 0);
    },
  },
  computed: {
    posterUrl() {
      if (
        this.filmData.Film_Poster === null ||
        this.filmData.Film_Poster === undefined
      ) {
      }
      return this.filmData.Film_Poster;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- clip-path: polygon(100% 0%, 100% 70%, 50% 100%, 0% 70%, 0% 0%, 50% 30%); margin-top: -14px; -->
<style scoped>
.poster-wrapper {
  overflow: hidden;
  display: block;
  margin: auto;
  position: relative;
  z-index: 50;
}

.poster-wrapper:hover {
  cursor: pointer;
  z-index: 50;
}

.section-title {
  position: absolute;
  z-index: 100;
  max-width: 200px;
  transform: translateY(90px);
  padding: 10px;
  background-color: black;
  opacity: 0.75;
}

img {
  object-fit: cover;
  display: block;
  filter: grayscale(100%) brightness(45%);
  margin: auto;
  border-bottom: 1px solid black;
}
</style>
