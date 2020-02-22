<template>
   
    <div class="header">
<el-row>
    <el-col :xs="24" :sm="22" :md="20" :lg="18" :xl="16" class="wrapper">
      <el-menu
        background-color="#f7f8f8"
        text-color="#fff"
        active-text-color="#ffd04b"
      >
 
  <br/>
            
        <form @submit.prevent="handleSearch">
          <el-input
            size="small"
            placeholder="search music..."
            :prefix-icon="
            !this.searchQuery && this.searchTracksLoading ? 'el-icon-loading' : 'el-icon-search'
            "
            class="searchInput"
            tabindex="-1"
            v-model="query"
            clearable
          />
        </form>
      </el-menu>
    </el-col>
    <div class="genresMenu" v-if="$route.path === '/'">
      <el-col
        :xs="24"
        :sm="22"
        :md="20"
        :lg="18"
        :xl="16"
        class="genresWrapper"
        v-if="!searchQuery"
      >
        <el-button-group size="small">
          <el-button
            v-for="(genre, i) in genres"
            :key="i"
            @click="getGenreItems(genre)"
            v-bind:class="{active: activeGenre === genre, preActive: getTracksLoading === genre}"
          >
            {{genre}}
          </el-button>
        </el-button-group>
      </el-col>
      <el-col
        :xs="24"
        :sm="22"
        :md="20"
        :lg="18"
        :xl="16"
        class="searchQueryWrapper"
        v-if="searchQuery"
      >
        <h3>{{ searchTracksLoading ? 'Loading...' : `Results of searched '${searchQuery}'`}}</h3>
         <el-button
           @click="handleClearSearch"
           class="clearSearchButton"
           icon="el-icon-close"
           v-if="!searchTracksLoading"
           circle
         />
      </el-col>
    </div>
  </el-row></div>
  
<br/><br/>


</template>



<script>
import { mapGetters } from 'vuex';

export default {
  data() {
    return {
      genres: [
        'house',
        'chill',
        'deep',
        'dubstep',
        'classical',
        'electronic',
        'trance',
        'pop',
        'rock',
      ],
      query: '',
    };
  },
  updated() {
    if (!this.query && this.searchQuery) {
      this.handleClearSearch();
    }
  },
  computed: {
    ...mapGetters({
      activeGenre: 'activeGenre',
      getTracksLoading: 'getTracksLoading',
      searchQuery: 'searchQuery',
      searchTracksLoading: 'searchTracksLoading',
    }),
  },
  methods: {
    getGenreItems(genre) {
      this.$store.dispatch('setActiveTeack', null);
      this.$store.dispatch('clearTracks');
      this.$store.dispatch('getTracks', {
        genre,
        page: 1,
      });
    },
    handleSearch() {
      if (this.query && (!this.searchQuery || (this.searchQuery !== this.query))) {
        this.$store.dispatch('search', {
          query: this.query,
          page: 1,
        });
      }
    },
    handleClearSearch() {
      this.$store.dispatch('clearSearch');
    },
  },
};
</script>

<style scoped>
  .wrapper {
    margin: 0 auto;
    float: none;
  }
  .el-row {
    background: #f7f8f8;
    padding: 0;
  }
  .el-menu {
    border: none;
  }
  .logo {
    width: 170px;
    float: left;
    margin: 0 20px 0 0;
  }
  .searchInput {
    float: right;
    width: 280px;
    margin: 13px 0;
  }
  .searchInput .el-input__inner {
    background: #2b2b2b;
    border: none;
    outline: none;
    color: #fff;
  }
  .genresMenu {
    width: 100%;
    background: #fff;
  }
  .searchQueryWrapper {
    margin: 0 auto;
    float: none;
    background: #fff;
    padding: 10px 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .searchQueryWrapper h3 {
    margin: 0;
  }
  .searchQueryWrapper .clearSearchButton {
    margin-left: 20px;
    padding: 8px;
  }
  .searchQueryWrapper .clearSearchButton:hover {
    border-color: #43b883;
    color: #fff;
    background: #43b883;
  }
  .genresWrapper {
    margin: 0 auto;
    float: none;
    background: #fff;
    display: block;
  }
  .genresWrapper .el-button {
    padding: 15px 35px;
    border: none;
    border-radius: 0;
    background: #fff;
    color: #999;
    border-bottom: 3px solid #fff;
  }
  .genresWrapper .el-button.active {
    background: #f1f1f1;
    color: #444;
    border-bottom: 3px solid #43b883;
  }
  .genresWrapper .el-button.preActive {
    background: #f1f1f1;
    color: #444;
    border-bottom: 3px solid #f1f1f1;
  }
  .genresWrapper .el-button:hover {
    background: #f1f1f1;
    color: #444;
    border-bottom: 3px solid #f1f1f1;
  }
  .el-button > span {
    font-size: 16px;
  }
  @media (max-width: 768px) {
    .el-button-group {
      overflow-x: scroll;
      overflow-y: hidden;
      white-space: nowrap;
      width: 100%;
    }
    .genresWrapper .el-button {
      float: none;
    }
    .logo {
      float: none;
    }
    .searchInput {
      width: calc(100% - 20px);
      margin: 0 10px 20px;
      box-sizing: border-box;
    }
  }
</style>



<style>
.header {
  position:fixed; /* fixing the position takes it out of html flow - knows
                   nothing about where to locate itself except by browser
                   coordinates */
  left:0;           /* top left corner should start at leftmost spot */
  top:0;            /* top left corner should start at topmost spot */
  width:100vw;      /* take up the full browser width */
  z-index:200;  /* high z index so other content scrolls underneath */
  height:100px;     /* define height for content */
}
</style>


  
