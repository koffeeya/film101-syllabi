<template>
  <div class="content-area">
    <div class="film-sidebar">Film area</div>
    <div class="syllabi-container">
      <SyllabusComponent
        v-for="syllabus in data"
        :key="syllabus.Syllabus_Id"
        :meta="syllabus.meta"
        :films="syllabus.films"
        :syllabi="syllabi"
      />
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
import {
  tidy,
  select,
  startsWith,
  groupBy,
  rename,
  distinct,
  summarize,
  fullJoin,
  mutate,
} from "@tidyjs/tidy";
import SyllabusComponent from "./components/SyllabusComponent.vue";

export default {
  name: "App",
  data() {
    return {
      raw: null,
      data: null,
      syllabi: null,
      films: null,
      tags: [
        "Film Theory & History",
        "Cinematography & Perspective",
        "Narrative & Story",
        "Experimental / Animation",
        "Minority Narratives & Spectatorship",
        "Style & Aesthetics",
        "Editing",
        "Mise-en-scene",
        "Film Form & Audience",
        "Sound & Music",
        "Genre",
        "Industry & Culture",
        "Hollywood & Classical Cinema",
        "Documentary & Nonfiction",
        "Methods & Analysis",
        "Filmmaking & Production",
        "Ideology",
        "Authorship",
        "The Idea of Film",
      ],
    };
  },
  components: {
    SyllabusComponent,
  },
  methods: {
    getItemsInObj(obj, name) {
      if (obj === undefined) {
        return [];
      } else if (obj.length === 0) {
        return [];
      } else {
        const len = obj.length;
        const arr = [];
        for (let row = 0; row < len; row++) {
          const value = Object.values(obj)[row][name];
          arr.push(value);
        }
        const unique = new Set(arr);
        return Array.from(unique);
      }
    },
    cleanData(data) {
      const results = tidy(
        data,
        select([
          "Syllabus_Id",
          "Section_Id",
          "Section_Name",
          "Syllabus_Number",
          "Syllabus_School",
          "Syllabus_Sem",
          "Syllabus_Year",
          startsWith("Film_"),
          "Tags",
        ]),
        groupBy(
          ["Syllabus_Id", "Film_Id"],
          [],
          groupBy.levels({
            levels: ["entries-object", "object"],
            single: true,
          })
        ),
        rename({
          key: "Syllabus_Id",
          values: "films",
        })
      );
      const syllabiData = tidy(
        data,
        select([startsWith("Syllabus_")]),
        distinct(["Syllabus_Id"]),
        groupBy(["Syllabus_Id"], [summarize({ meta: (meta) => meta })])
      );
      this.data = tidy(
        results,
        fullJoin(syllabiData, { by: ["Syllabus_Id", "Syllabus_Id"] }),
        mutate({
          meta: (d) => d.meta[0],
        })
      );
      this.syllabi = this.getItemsInObj(results, "Syllabus_Id");
      this.films = this.getItemsInObj(this.raw, "Film_Id");
    },
  },
  created() {
    // Load in the data
    Promise.all([d3.csv("./syllabi.csv", d3.autoType)]).then(([data]) => {
      this.raw = data;
      this.cleanData(this.raw);
    });
  },
};
</script>

<style>
html {
  background-color: black;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
}

.content-area {
  display: grid;
  grid-template-columns: 1fr 2fr;
}

.syllabi-container {
  display: grid;
  grid-template-columns: repeat(44, 1fr);
  column-gap: 0px;
}

.film-sidebar {
  background-color: rgb(22, 22, 22);
  margin: 10px;
}
</style>
