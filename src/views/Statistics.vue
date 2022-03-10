<template>
  <div class="stats mb-5">
    <query
      :tempAthletesUrl="tempAthletesUrl"
      :tempGamesUrl="tempGamesUrl"
      :tempYearsUrl="tempYearsUrl"
      :tempCoachUrl="tempCoachUrl"
      v-for="(query, index) in queries"
      v-bind:key="query.key"
      @remove="removeQuery(index)"
    />
    <div class="text-center mt-5">
      <button id="qButton" type="button" class="btn btn-outline-primary" @click="add(); getIP();">Query <svg width="25px" height="25px" viewBox="0 0 16 16" class="bi bi-plus-square-fill" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
        <path fill-rule="evenodd" d="M2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2H2zm6.5 4.5a.5.5 0 0 0-1 0v3h-3a.5.5 0 0 0 0 1h3v3a.5.5 0 0 0 1 0v-3h3a.5.5 0 0 0 0-1h-3v-3z"/></svg>
      </button>
    </div>
  </div>
</template>

<script>
import Query from '@/components/Query.vue'
// https://byucougars.byu-dept-athletics-dev.amazon.byu.edu/feeds/sport-years/sport_nid
// Ex: https://byucougars.byu-dept-athletics-dev.amazon.byu.edu/feeds/sport-years/1701
const tempYearsUrl = 'https://byucougars.byu-dept-athletics-dev.amazon.byu.edu/feeds/sport-years'

// https://byucougars.com/feeds/athlete/year/sport_nid/year
// Ex: https://byucougars.com/feeds/athlete/year/1698/2014
const tempAthletesUrl = 'https://byucougars.com/feeds/athlete/year'

// https://byucougars.com/feeds/game/year/sport_nid/year
// Ex: https://byucougars.com/feeds/game/year/1698/2014
const tempGamesUrl = 'https://byucougars.com/feeds/game/year'
const tempCoachUrl = 'https://byucougars.com/feeds/coach/year'

export default {

  name: 'Statistics',
  data () {
    return {
      queries: [],
      uniqueKey: 0,
      tempAthletesUrl: tempAthletesUrl,
      tempGamesUrl: tempGamesUrl,
      tempYearsUrl: tempYearsUrl,
      tempCoachUrl: tempCoachUrl,
      athletes: []
    }
  },
  components: {
    Query
  },
  methods: {
    add () {
      // let quantity = this.events.length
      this.queries.push({
        key: this.uniqueKey++,
        query: Query
      })
    },
    removeQuery (index) {
      this.queries.splice(index, 1)
    }
  }
}
</script>

<style scoped>
#qButton {
  color: #0d6efd;
  background-color: white;
}

#qButton:hover {
  color: white;
  background-color: #0d6efd;
}
</style>
