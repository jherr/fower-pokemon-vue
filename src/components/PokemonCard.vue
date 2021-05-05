<template>
  <div p-10 border-1 borderGray500 roundedXL grid gap-10 class="card">
    <img :src="image" w="100%" />
    <div>
      <div grid gridTemplateColumns-2 gap-10>
        <div>
          <div textXL fontBold>
            {{name.english}}
          </div>
          <div textXL fontBold>
            {{name.japanese}}
          </div>
        </div>
        <div>
          <PillButton
            @click="onSelect"
            :selected="selected"
          >
            Select
          </PillButton>
        </div>
      </div>
      <div textLG mt-10>
        {{type}}
      </div>
      <div grid gridTemplateColumns-2 gap-10 ml-20 mt-10>
        <template v-for="item in base">
          <div textSM fontBold :key="item.nameKey">
            {{item.name}}
          </div>
          <div textSM fontBold :key="item.valueKey">
            {{item.value}}
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import PillButton from "./PillButton";

export default {
  components: {
    PillButton,
  },
  name: "PokemonCard",
  props: ["pokemon"],
  computed: {
    selected() {
      return this.pokemon.selected;
    },
    name() {
      return this.pokemon.name || {
        english: "",
        japanese: ""
      };
    },
    type() {
      return (this.pokemon.type || []).join(", ")
    },
    image() {
      return this.pokemon.name ? `/pokemon/${this.pokemon.name.english.toLowerCase()}.jpg` : "";
    },
    base() {
      return Object.keys(this.pokemon.base)
        .reduce((a, key) => ([
          ...a,
          {
            name: key,
            value: this.pokemon.base[key],
            nameKey: `name:${key}`,
            valueKey: `value:${key}`,
          }
        ]), [])
    }
  },
  methods: {
    onSelect() {
      this.$emit("select", this.pokemon.name.english)
    }
  }
}
</script>

<style scoped>
.card {
  grid-template-columns: 30% 70%;
}
</style>