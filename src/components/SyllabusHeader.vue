<template>
  <!-- :style="{ minHeight: height + 'px' }" -->
  <div
    class="syllabus-header"
    :style="{ minWidth: width + 'px' }"
    :class="[`${meta.Syllabus_Id}`]"
    @mouseover="highlight(`${meta.Syllabus_Id}`)"
    @mouseout="highlightReset(`${meta.Syllabus_Id}`)"
  >
    <div class="syllabus-text">
      {{ meta.Syllabus_Initials }}
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "SyllabusHeader",
  props: ["meta", "width"],
  methods: {
    highlight(syllabusName) {
      d3.selectAll(`.${syllabusName} > div > img`)
        .transition()
        .duration(100)
        .style("filter", "grayscale(0%) brightness(100%)");
    },
    highlightReset(syllabusName) {
      d3.selectAll(`.${syllabusName} > div > img`)
        .transition()
        .duration(100)
        .style("filter", "grayscale(100%) brightness(45%)");
    },
  },
  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- clip-path: polygon(100% 0%, 100% 70%, 50% 100%, 0% 70%, 0% 0%, 50% 30%); margin-top: -14px; -->
<style scoped>
.syllabus-header {
  padding: 0px 0px 0px 5px;
  transform: rotate(-90deg);
  margin-top: auto;
  margin-bottom: 10px;
}

.syllabus-text {
  width: fit-content;
  padding: 5px;
}

.syllabus-text:hover {
  background-color: gray;
  cursor: pointer;
}
</style>
