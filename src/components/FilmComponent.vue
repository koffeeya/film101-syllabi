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
    <div class="image-wrapper"><img class="poster" :src="posterUrl" /></div>
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "FilmComponent",
  props: ["msg", "filmData"],
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
<style scoped>
.image-wrapper {
  clip-path: polygon(100% 0%, 100% 70%, 50% 100%, 0% 70%, 0% 0%, 50% 30%);
  height: 50px;
  margin-top: -14px;
}

img {
  width: 100%;
  filter: grayscale(100%) url("#teal-lightgreen");
  /* border: 5px solid white; */
}
</style>
