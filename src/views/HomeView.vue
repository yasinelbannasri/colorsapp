<template>

  <div>
    <div id="top-bar-container">
      <div id="top-bar">
        <h2>色</h2>
      </div>
      <div id="top-bar-2">
        <h3>THE COLOR APP</h3>
      </div>
    </div>
    <div class="home">
      <pantone-card v-for="pantone of pantones[getCurrentIndex]" :key="pantone.hex" :pantone="pantone" />
      <section class="pagination">
        <router-link class="pagination-entry" v-if="this.$route.query['pageNumber'] > 0"
          :to="{path: '/', query: { pageNumber: getCurrentIndex - 1} }">&lt;&nbsp;</router-link>
        <pagination-list :amountOfPages="pantones.length"></pagination-list>
        <router-link class="pagination-entry" v-if="this.$route.query['pageNumber'] < pantones.length - 1"
          :to="{path: '/', query: { pageNumber: getCurrentIndex - -1} }">&gt;</router-link>
      </section>

    </div>
  </div>
</template>

<script>

import PantoneCard from "../components/PantoneCard.vue";
import PaginationList from "../components/PaginationList.vue";

export default {
  name: "HomeView",
  data() {
    return {
      pantones: [],
      currentIndex: 0,
      pageIndex: 0,
    }
  },
  computed: {
    getCurrentIndex() {
      if (this.$route.query["pageNumber"] !== undefined) {
        return this.$route.query["pageNumber"];
      }
      return 0;
    }
  },
  components: {
    PantoneCard,
    PaginationList
  },
  async mounted() {
    let fetchResult = await fetch(`http://localhost:3000/pantones`);
    fetchResult = await fetchResult.json();
    //This code takes out all the keys, returns a new object by accessing the value with the keys out of Object.keys()
    //The point of it is to return an array per property, instead of a single object.
    this.pantones = Object.keys(fetchResult[0]).map(key => {
      return fetchResult[0][key];
    });

    let counter = 1;
    let tempList = [];
    let tempFinalList = [];

    while (tempFinalList.length < this.pantones.length / 30) {
      tempList.push(this.pantones[counter - 1])
      if (counter % 30 === 0 && counter !== 0) {
        tempFinalList.push(tempList);
        tempList = [];
      }
      counter++;
    }

    this.pantones = tempFinalList;
    console.log(this.pantones)

  },
};
</script>

<style scoped>
#top-bar-container {
  display: flex;
  margin-bottom: 10vh;
  height: 6vh;
}

#top-bar {
  display: flex;
  align-content: center;
  flex-direction: column;
  justify-content: center;

  height: 100%;
  width: 5%;
  min-width: 70px;
  color: cyan;
  background-color: #272727;
}

#top-bar-2 {
  display: flex;
  align-content: center;
  flex-direction: row;
  height: 100%;
  width: 95%;
  background-color: #727272;
  flex-wrap: wrap;
}
#top-bar-2 h3 {
  color: #a1f3f4;
  margin-left: 25vw;
}

.pagination {
  display: flex;
  flex: 0 0 100%;
  justify-content: space-around;
}

.pagination-entry:hover {
  color: darkorange;
  text-decoration-line: none;

}

a {
  display: inline-block;
  font-weight: bold;
  color: #1f7edd;
}

.home {
  display: flex;
  grid-template-columns: repeat(10, 1fr);
  flex-wrap: wrap;
  justify-content: space-between;

}
</style>