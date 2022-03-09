<template>
  <div class="wrapper recipe">
    <router-link to="/" class="back">&lt; Back</router-link>
    <h1>{{ recipe.title }}</h1>
    <p class="desc">{{ recipe.description }}</p>
    <hr />
    <div class="ingredients">
      <h3>Ingredientsss</h3>
      <ul>
        <li v-for="(ingredient, i) in recipe.ingredients" :key="i">
          {{ ingredient }}
        </li>
      </ul>
    </div>
    <div class="method">
      <h3>Method</h3>
      <ol>
        <li v-for="(step, i) in recipe.method" :key="i">
          <span v-html="cleanText(step)"></span>
        </li>
      </ol>
    </div>
  </div>
</template>
<script>
export default {
  name: "Recipe",
  components: {},
  computed: {
    recipe() {
      return this.$store.state.recipes.find(
        (recipe) => recipe.slug === this.$route.params.slug
      );
    },
  },
  methods: {
    cleanText(text) {
      return text.replace(/\n/g, "<br />");
    },
  },
};
</script>
<style lang="scss" >
.ingredients,
.method {
  max-width: 500px;
  margin: auto;
  text-align: start;
}

.back {
  text-align: start;
  display: inherit;
}
</style>
