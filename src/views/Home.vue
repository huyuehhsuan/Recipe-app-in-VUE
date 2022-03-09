<template>
  <div class="wrapper home">
    <h1>My Recipes</h1>
    <button type="button" class="popBtn">Add New Recipe</button>

    <div class="recipes">
      <div
        class="card"
        v-for="recipe in this.$store.state.recipes"
        :key="recipe.title"
      >
        <h2>{{ recipe.title }}</h2>
        <p>{{ recipe.description }}</p>
        <router-link :to="`/recipe/${recipe.slug}`">
          <button>View Recipe</button>
        </router-link>
      </div>
    </div>

    <div class="addPopup">
      <h2>Add new recipes</h2>
      <form action="">
        <div>
          <label for="title">Title</label><br />
          <input type="text" id="title" v-model="newRecipe.title" />
        </div>
        <div>
          <label for="description">Description</label><br />
          <textarea
            id="description"
            rows="10"
            v-model="newRecipe.description"
          />
        </div>
        <div>
          <label for="ingredients">Ingredients</label>
          <div v-for="i in newRecipe.ingredientRows" :key="i">
            <input
              type="text"
              id="ingredients"
              v-model="newRecipe.ingredients[i - 1]"
            />
          </div>

          <button type="button" @click.prevent="addIngredient()">
            Add Ingredients
          </button>
        </div>
        <div>
          <label for="method">Method</label>
          <div v-for="i in newRecipe.methodRows" :key="i">
            <textarea id="method" rows="2" v-model="newRecipe.method[i - 1]" />
          </div>

          <button type="button" @click.prevent="addStep()">Add Step</button>
        </div>
        <button
          type="submit"
          class="addNewRecipe"
          @click.prevent="addNewRecipe()"
        >
          Add Recipe
        </button>
        <button type="button" class="popBtnClose">Close</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      newRecipe: {
        title: "",
        description: "",
        ingredients: [],
        method: [],
        ingredientRows: 1,
        methodRows: 1,
      },
    };
  },
  components: {},
  mounted() {
    const popBtn = document.querySelector(".popBtn");
    const popBtnClose = document.querySelector(".popBtnClose");
    const addPopup = document.querySelector(".addPopup");
    popBtn.addEventListener("click", function () {
      addPopup.classList.add("active");
    });
    popBtnClose.addEventListener("click", function () {
      addPopup.classList.remove("active");
    });
  },
  methods: {
    addIngredient() {
      this.newRecipe.ingredientRows++;
    },
    addStep() {
      this.newRecipe.methodRows++;
    },
    addNewRecipe() {
      this.newRecipe.slug = this.newRecipe.title
        .toLowerCase()
        .replace(/\s/g, "-");
      if (!this.newRecipe.slug) {
        alert("Please enter a title");
        return;
      }
      this.$store.commit("ADD_RECIPE", this.newRecipe);
      this.newRecipe = {
        title: "",
        description: "",
        ingredients: [],
        method: [],
        ingredientRows: 1,
        methodRows: 1,
      };
      const addPopup = document.querySelector(".addPopup");
      const addNewRecipe = document.querySelector(".addNewRecipe");
      addNewRecipe.addEventListener("click", function () {
        addPopup.classList.remove("active");
      });
    },
  },
};
</script>
<style lang="scss" scoped>
label {
  display: inline-block;
  margin: 0.8rem 0;
}
input,
textarea {
  width: 100%;
}
.addPopup {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  text-align: start;
  width: 500px;
  border: 1px solid #999;
  padding: 1.25rem;
  background-color: #fff;
  z-index: 10;
  opacity: 0;
  visibility: hidden;
  transition: 0.5s;
}
.active {
  top: 2rem;
  opacity: 1;
  visibility: visible;
}
.recipes {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  .card {
    border: 1px solid #999;
    border-radius: 0.5rem;
    background-color: #fff;
    padding: 0.5rem;
  }
}
</style>
