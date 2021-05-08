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
        :style="{ maxHeight: imgHeight * 1.8 + 'px' }"
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
        .style("filter", "grayscale(0%)")
        .style("cursor", "pointer");
      //d3.selectAll(`.${syllabusName}`).style("z-index", "200");
    },
    highlightFilmsReset(className, syllabusName) {
      d3.selectAll(`.${className} > div > img`)
        .transition()
        .duration(100)
        .style("filter", "grayscale(100%)");
      //d3.selectAll(`.${syllabusName}`).style("z-index", 0);
    },
  },
  computed: {
    posterUrl() {
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
}

img {
  object-fit: cover;
  filter: grayscale(90%);
  border-top: 3px solid black;
  border-bottom: 3px solid black;
  /* border: 5px solid white; */
}
</style>
