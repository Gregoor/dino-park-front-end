<template>
  <form :class="'search-form' + ( modifier ? ' ' + modifier : '')" :action="this.$router.resolve({ name: 'Search', query: { query, who, }}).href" @submit="handleSubmit" method="GET">
    <fieldset>
      <legend class="visually-hidden">Search</legend>
      <div class="search-form__fields">
        <label for="search-query" class="visually-hidden">Search term</label>
        <input type="text" id="search-query" name="query" v-model="searchQuery" class="search-form__input" ref="searchQueryField" placeholder="Search for people by name">
        <button type="button" v-if="searchQuery && searchQuery.length > 0" @click="clearQuery" class="search-form__clear-button">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <line x1="18" y1="6" x2="6" y2="18" />
            <line x1="6" y1="6" x2="18" y2="18" />
          </svg>
          <span class="visually-hidden">Clear query</span>
        </button>
        <button type="submit" class="search-form__submit">
          <img src="@/assets/images/search.svg" alt="" role="presentation" aria-hidden="true" width="20" />
          <span class="visually-hidden">Search</span>
        </button>
      </div>
    </fieldset>
  </form>
</template>

<script>
export default {
  name: 'SearchForm',
  props: {
    modifier: String,
  },
  methods: {
    handleSubmit(event) {
      event.preventDefault();

      if (!this.searchQuery.length > 0) {
        this.$refs.searchQueryField.focus();
      } else {
        this.$router.push({
          name: 'Search',
          query: {
            query: this.searchQuery,
            who: this.who,
          },
        });
        this.$emit('close-search-form');
      }
    },
    clearQuery() {
      this.searchQuery = null;
    },
  },
  computed: {
    query: {
      get() {
        return this.$route.query.query || null;
      },
    },
    who() {
      return this.$route.query.who || 'all';
    },
  },
  data() {
    return {
      searchQuery: this.query,
    };
  },
  mounted() {
    this.$refs.searchQueryField.focus();
  },
};
</script>

<style>
.search-form {
  margin-right: auto;
  width: 100%;
  max-width: 25em;
}
  .search-form fieldset {
    border: 0;
    padding: 0;
  }
  .search-form__fields {
    display: flex;
    flex-direction: row;
    position: relative;
  }
     .search-form__fields input,
     .search-form__fields button[type="submit"] {
       background-color: var(--white);
       font-size: 1.15em;
       border: 1px solid var(--gray-30);
       padding: .5em;
     }
     .search-form__input {
       width: 100%;
       -webkit-appearance: none;
       appearance: none;
       border-radius: 0;
       padding-left: 3em;
       text-align: center;
     }
    .search-form__submit {
       border: 0;
       appearance: none;
       width: 2.5em;
       margin-right: -1px;
       position: absolute;
       left: 0;
       top: 0;
    }
      .search-form__submit img {
        vertical-align: middle;
      }
    .search-form__clear-button {
      background-color: var(--white);
      position: absolute;
      right: 0;
      border: 0;
      top: 1em;
      right: 1em;
      width: 2em;
      padding: 0;
      line-height: 1;
    }

.search-form--small {
  padding: 1em;
  max-width: none;
  background: var(--white);
  margin-bottom: 1em;
  position: fixed;
  left: 0;
  right: 0;
  top: 5em;
  z-index: var(--layerTopBar);
  box-shadow: var(--shadowCard);
}
</style>
