<template>
   <div id="search-container" ref=searchContainerRef>
      <div id="search-input">
        <input type='text'  placeholder='Search' ref=searchTerm />
        <button>
          <app-icon name="search" />
        </button>
      </div>
      <div>
        <span>
          <input type="checkbox" />
          Case sensitive
        </span>
        <span>
          <input type="checkbox" />
          Whole word
        </span>
      </div>
      <div class="divider" />
      <div id='search-buttons'>
        <span>
          <button>
           <app-icon name="searchClose" />
          </button>
        </span>
        <span id="search-iterators">
          <button>
            <app-icon name="chevronLeft" alt="Previous Search Result"/>
          </button>
          <button>
            <app-icon name="chevronRight" alt="Next Search Result" />
          </button>
        </span>
      </div>
    </div>
</template>

<script>

export default {
  name: 'Search',

  data() {
    return {
      searchResults: [],
      activeResultIndex: -1,
      toggledSearchModes: [],
      searchModes: {}
    }
  }
}
</script>

<style scoped>
#search-container {
  overflow: auto;
  flex: 1;
  padding: 1em;
  min-width: 293px;
}

#search-container button {
  border: none;
  outline: none;
  box-shadow: none;
}

#search-input * {
  vertical-align: middle;
  box-sizing: content-box;
  height: 28px;
  border: none;
}

#search-input input {
  border-right: none;
}

#search-input input:focus {
  outline-style: solid;
  outline-color: var(--border);
}

#search-input button {
  border: 1px solid var(--border);
}

#search-buttons {
  display: flex;
}

#search-buttons button {
  margin: 0.5em;
  border: 1px solid var(--border);
}

#search-buttons > span:first-of-type {
  flex: 1;
}

.search-result {
  margin: 1em;
  padding: 1em;
  border: 1px solid var(--border);
  cursor: pointer;
  max-width: 275px;
}

.search-value {
  background-color: rgba(250,206,0,.50196);
}
</style>