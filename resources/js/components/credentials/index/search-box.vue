<style scoped>
.search__input {
  background-color: unset;
  border: none;
  border-bottom: 1px solid black;
}
.search__btn {
  background-color: black;
  color: white;
  border-radius: 0 0.5rem 0.5rem 0;
}
.search__clean-trigger {
  color: var(--app-blue);
  text-decoration: underline;
  cursor: pointer;
}
.search__loading {
  width: 20px;
  height: 20px;
}
</style>
<template>
  <div class="d-flex">
    <form class="px-2" v-on:submit.prevent="search">
      <input
        type="text"
        class="search__input"
        placeholder="Search credential..."
        ref="search"
      />
      <button class="search__btn px-2">
        <img
          width="22px"
          height="22px"
          src="/images/search-icon.svg"
          alt="trigger search for credential by title"
        />
      </button>
    </form>
    <AppLoading class="search__loading me-2" v-if="fetching"></AppLoading>
    <p class="search__clean-trigger me-2" v-if="needsClear" v-on:click="clear">Clear</p>
  </div>
</template>

<script>
import AppLoading from "../../app-loading-placeholder.vue";

export default {
  components: {
    AppLoading
  },
  data() {
    return {
      needsClear: false,
      fetching: false
    }
  },
  methods: {
    setToFetchingState: function () {
      this.fetching = true;
    },
    setToInitialState: function () {
      this.fetching = false;
    },
    clear: function () {
      this.needsClear = false;
      this.$refs.search.value = '';
      this.$emit("requireSearch", this, "/api/credentials");
    },
    search: function (formElement) {
      const titleParam = formElement.target.elements[0].value;
      let searchUrl = "/api/credentials";

      if (titleParam != "") {
        searchUrl = "/api/credentials/" + titleParam;
        this.needsClear = true;
      } else {
        this.needsClear = false;
      }

      this.$emit("requireSearch", this, searchUrl);
    },
  },
};
</script>
