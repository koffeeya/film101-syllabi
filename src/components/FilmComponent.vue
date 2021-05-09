<template>
  <div
    class="film-section"
    :class="[`${filmData.Syllabus_Id}`, `${filmData.Film_Id}`]"
    @mouseover="
      highlightFilms(`${filmData.Film_Id}`, `${filmData.Syllabus_Id}`)
    "
    @mouseout="
      highlightFilmsReset(`${filmData.Film_Id}`, `${filmData.Syllabus_Id}`)
    "
  >
    <div
      class="image-wrapper"
      :style="{ height: imgHeight + 'px', width: imgWidth + 'px' }"
    >
      <img
        class="poster"
        :src="posterUrl"
        :style="{ height: imgHeight + 'px', width: imgWidth + 'px' }"
      />
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "FilmComponent",
  props: ["msg", "filmData", "imgHeight", "imgWidth"],
  methods: {
    highlightFilms(className, syllabusName) {
      d3.selectAll(`.${className} > div > img`)
        .transition()
        .duration(100)
        .style(
          "filter",
          "grayscale(0%) brightness(100%) drop-shadow(0 0 0.75rem white)"
        );
      //d3.selectAll(`.${syllabusName}`).style("z-index", "200");
    },
    highlightFilmsReset(className, syllabusName) {
      d3.selectAll(`.${className} > div > img`)
        .transition()
        .duration(100)
        .style("filter", "grayscale(100%) brightness(35%)");
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
.image-wrapper {
  overflow: hidden;
  display: block;
  margin: auto;
  border: 1px solid black;
  position: relative;
}

/* filter: grayscale(90%); */

img {
  object-fit: cover;
  display: block;
  filter: grayscale(100%) brightness(35%);
  margin: auto;
}

.image-wrapper:hover {
  border: 1px solid white;
  cursor: pointer;
  transform-origin: center;
  transform: scale(2);
  z-index: 50;
  filter: drop-shadow(0 0 0.75rem white);
}
</style>
