<template>
  <h1>Hi forest!!!!!!!!!!!!!!11213423412341</h1>
  <div class="expander">
    <div class="query mt-3 p-3 container">
      <svg
        @click="$emit('remove')"
        width="1em"
        height="1em"
        viewBox="0 0 16 16"
        id="close"
        class="bi bi-x btn-outline-danger x-button"
        fill="currentColor"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          fill-rule="evenodd"
          d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"
        />
      </svg>
      <h1>Hi Fores123123123123</h1>
      <div class="row mt-1 mb-1">
        <div class="col-sm">
          <div class="form-group">
            <label>Sport</label>
            <select
              class="form-select"
              v-model="selected.sport"
              @change="getYears(); changeSportClear();"
            >
              <option value="1698">Baseball</option>
              <option value="1699">Men's Basketball</option>
              <option value="1707">Women's Basketball</option>
              <!-- <option value="1700">Men's Cross Country</option> -->
              <!-- <option value="1708">Women's Cross Country</option> -->
              <option value="1701">Football</option>
              <!-- <option value="1702">Men's Golf</option> -->
              <!-- <option value="1709">Women's Golf</option> -->
              <!-- <option value="1710">Women's Gymnastics</option> -->
              <option value="1711">Soccer</option>
              <option value="1712">Softball</option>
              <!-- <option value="1703">Men's Swimming & Diving</option> -->
              <!-- <option value="1713">Women's Swimming & Diving</option> -->
              <!-- <option value="1704">Men's Tennis</option> -->
              <!-- <option value="1714">Women's Tennis</option> -->
              <!-- <option value="1705">Men's Track & Field</option> -->
              <!-- <option value="1715">Women's Track & Field</option> -->
              <option value="1706">Men's Volleyball</option>
              <option value="1716">Women's Volleyball</option>
            </select>
          </div>
        </div>

        <div class="col-sm">
          <div class="form-group">
            <label>Category</label>
            <select
              id="category"
              class="form-select"
              v-model="selected.category"
              @change="getAthleteOrGamesOrCoach(); getSeasonStats(); "
            >
              <option value="athlete">Athlete</option>
              <option value="coach">Coach</option>
              <option value="game">Game</option>
              <option value="season-stats">Season Stats</option>
            </select>
          </div>
        </div>

        <div class="col-sm">
          <div class="form-group">
            <label>Year</label>
            <select
              id="year"
              class="form-select"
              v-model="selected.year"
              @change="getAthleteOrGamesOrCoach()"
            >
              <option
                v-for="(y, index) in years"
                :key="index"
                v-bind:value="y.Year"
              >
                {{ y.Year }}
              </option>
            </select>
          </div>
        </div>

        <div
          v-if="selected.category === 'athlete'"
          @change="changeAthleteGameCoachClear(); getData();"
          class="col-sm"
        >
          <div class="form-group">
            <label>Athlete</label>
            <select class="form-select" v-model="selected.athleteNid">
              <option
                v-for="(ath, index) in athletes"
                :key="index"
                v-bind:value="ath.nid"
              >
                {{ ath.field_last_name }} {{ ath.field_first_name }}
              </option>
            </select>
          </div>
        </div>

        <div
          v-if="selected.category === 'game'"
          @change="changeAthleteGameCoachClear(); getData();"
          class="col-sm"
        >
          <div class="form-group">
            <label>Game</label>
            <select class="form-select" v-model="selected.gameNid">
              <option
                v-for="(game, index) in games"
                :key="index"
                v-bind:value="game.nid"
              >
                {{ game.title }}
              </option>
            </select>
          </div>
        </div>

        <div
          v-if="selected.category === 'coach'"
          @change="changeAthleteGameCoachClear(); getData();"
          class="col-sm"
        >
          <div class="form-group">
            <label>Coach</label>
            <select class="form-select" v-model="selected.coach">
              <option
                v-for="(coach, index) in coaches"
                :key="index"
                v-bind:value="coach.nid"
              >
                {{ coach.title }}
              </option>
            </select>
          </div>
        </div>

        <div class="col-sm">
          <div class="form-group">
            <label></label><br />
            <!-- :disabled="!(gamesRecordPlayerIn === null && gamesRecord === null)" -->
            <button
              id="submit"
              type="submit"
              :disabled="!isBoxScoreTableReady && gamesRecordPlayerIn === null"
              class="btn btn-outline-primary"
              @click="submit"
            >
              Submit
            </button>
          </div>
        </div>
      </div>
      <FootballAthlete
        ref="myFootball"
        v-if="selected.sport == '1701' && gamesRecordPlayerIn"
        :selected="selected"
        :gamesRecordPlayerIn="gamesRecordPlayerIn"
      />
      <VolleyballAthlete
        ref="myVolleyball"
        v-if="
          (selected.sport == '1706') | (selected.sport == '1716') &&
          gamesRecordPlayerIn
        "
        :selected="selected"
        :gamesRecordPlayerIn="gamesRecordPlayerIn"
      />
      <BaseballAthlete
        ref="myBaseball"
        v-if="selected.sport == '1698' || selected.sport == '1712'  && gamesRecordPlayerIn"
        :selected="selected"
        :gamesRecordPlayerIn="gamesRecordPlayerIn"
      />
      <BasketballAthlete
        ref="myBasketball"
        v-if="(selected.sport == '1699') || (selected.sport == '1707') && gamesRecordPlayerIn"
        :selected="selected"
        :gamesRecordPlayerIn="gamesRecordPlayerIn"
      />
      <SoccerAthlete
        ref="mySoccer"
        v-if="selected.sport == '1711' && gamesRecordPlayerIn"
        :selected="selected"
        :gamesRecordPlayerIn="gamesRecordPlayerIn"
      />
      <VolleyballBoxScore
        v-if="(selected.sport == '1706') | (selected.sport == '1716') && stats"
        :selected="selected"
        :stats="stats"
      />
      <SoccerBoxScore
        v-if="selected.sport == '1711' && stats"
        :selected="selected"
        :stats="stats"
      />
      <FootballBoxScore
        v-if="selected.sport == '1701' && stats"
        :selected="selected"
        :stats="stats"
      />
      <SoftballBoxScore
        v-if="selected.sport == '1712' && stats"
        :selected="selected"
        :stats="stats"
      />
      <BaseballBoxScore
        v-if="selected.sport == '1698' && stats"
        :selected="selected"
        :stats="stats"
      />
      <MensBasketballBoxScore
        v-if="selected.sport == '1699' && stats"
        :selected="selected"
        :stats="stats"
      />
      <WomensBasketballBoxScore
        v-if="selected.sport == '1707' && stats"
        :selected="selected"
        :stats="stats"
      />
      <h1 v-if="!isStatsExist">Sorry, there is no stats.</h1>
    </div>
  </div>
</template>

<script>
import FootballAthlete from '@/components/football_tables/FootballAthlete.vue'
import VolleyballAthlete from '@/components/volleyball_tables/VolleyballAthlete.vue'
import BaseballAthlete from '@/components/baseball_tables/BaseballAthlete.vue'
import BasketballAthlete from '@/components/basketball_tables/BasketballAthlete.vue'
import SoccerAthlete from '@/components/soccer_tables/SoccerAthlete.vue'
import VolleyballBoxScore from '@/components/boxscore_tables/VolleyballBoxScore.vue'
import SoccerBoxScore from '@/components/boxscore_tables/SoccerBoxScore.vue'
import FootballBoxScore from '@/components/boxscore_tables/FootballBoxScore.vue'
import SoftballBoxScore from '@/components/boxscore_tables/SoftballBoxScore.vue'
import BaseballBoxScore from '@/components/boxscore_tables/BaseballBoxScore.vue'
import MensBasketballBoxScore from '@/components/boxscore_tables/MensBasketballBoxScore.vue'
import WomensBasketballBoxScore from '@/components/boxscore_tables/WomensBasketballBoxScore.vue'

export default {
  name: 'Query',
  props: ['tempYearsUrl', 'tempAthletesUrl', 'tempGamesUrl', 'tempCoachUrl'],
  data () {
    return {
      selected: {
        sport: undefined, // undefined for no input
        category: undefined,
        year: undefined,
        athleteNid: undefined,
        gameNid: undefined,
        coachNid: undefined
      },
      athletes: [],
      gamesRecordPlayerIn: null, // disable and enable the submit button
      stats: null,
      isBoxScoreTableReady: false,
      games: [],
      gameRecords: null,
      coaches: [],
      years: [],
      isStatsExist: true
    }
  },
  components: {
    FootballAthlete,
    VolleyballAthlete,
    SoccerAthlete,
    BaseballAthlete,
    BasketballAthlete,
    FootballBoxScore,
    VolleyballBoxScore,
    SoccerBoxScore,
    SoftballBoxScore,
    BaseballBoxScore,
    MensBasketballBoxScore,
    WomensBasketballBoxScore
  },
  methods: {
    async getSeasonStats () {
      // if (this.selected.category === 'season-stats') {
      //   const dataURL = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/seasonstats/77638,107076'
      //   console.log('dataURL', dataURL)
      //   const res = await fetch(dataURL)
      //   const data = await res.json()
      //   // this.gameRecords = data[0]
      //   console.log('data', data)
      // }
      // console.log(this.selected)
    },
    changeAthleteGameCoachClear () {
      // clear old data and disable the submit button when data is not received.
      this.gamesRecordPlayerIn = null
      this.stats = null
      this.isStatsExist = true
    },
    async getData () {
      if (this.selected.athleteNid) {
        // ======= fetch from URL API =======
        const athletesUrl = `https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/athlete/${this.selected.sport}/${this.selected.athleteNid}`
        const res = await fetch(athletesUrl)
        const data = await res.json()
        this.gamesRecordPlayerIn = data
        console.log('gamesRecordPlayerIn', this.gamesRecordPlayerIn)
      }
      if (this.selected.gameNid) {
        // const gamesUrl = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/boxscore/1290213'
        const gamesUrl = `https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/boxscore/${this.selected.gameNid}`
        const res = await fetch(gamesUrl)
        const data = await res.json()
        this.gameRecords = data[0]
        console.log('gameRecords', this.gameRecords)
        this.isBoxScoreTableReady = true
      }
    },
    submit () {
      // call child function to clear and reorganize the data, and then show table
      if ((this.selected.sport === '1698' || this.selected.sport === '1712') && this.selected.athleteNid) {
        this.$refs.myBaseball.reorganizeGames()
      } else if ((this.selected.sport === '1699' || this.selected.sport === '1707') && this.selected.athleteNid) {
        this.$refs.myBasketball.reorganizeGames()
      } else if (this.selected.sport === '1701' && this.selected.athleteNid) {
        this.$refs.myFootball.reorganizeGames()
      } else if (this.selected.sport === '1711' && this.selected.athleteNid) {
        this.$refs.mySoccer.reorganizeGames()
      } else if ((this.selected.sport === '1706' || this.selected.sport === '1716') && this.selected.athleteNid) {
        this.$refs.myVolleyball.reorganizeGames()
      }
      // show box score table
      this.stats = this.gameRecords
      console.log(this.selected)

      // show info if there is no stats for player
      this.statsExist()
      // console.log('selected', this.selected)
    },
    statsExist () {
      let data = null
      if (this.gamesRecordPlayerIn) {
        data = this.gamesRecordPlayerIn
      } else if (this.stats) {
        data = this.stats
      }
      if (data === null || data.length === 0) {
        this.isStatsExist = false
      } else if (data.length !== 0) {
        this.isStatsExist = true
      }
    },
    changeSportClear () {
      // clear category option if sport change
      this.selected.category = undefined
      this.selected.athleteNid = undefined
      // clear gamesRecordPlayerIn array if sport change and disable the submit button
      this.gamesRecordPlayerIn = null
      // clear stats and gameRecords array if sport change and disable the submit button (isBoxScoreTableReady)
      this.stats = null
      this.gameRecords = null
      this.isBoxScoreTableReady = false
      // disable the stats checking table
      this.isStatsExist = true
    },
    async getYears () {
      const yearsUrl = `${this.tempYearsUrl}/${this.selected.sport}`
      // ======= fetch from URL API =======
      const res = await fetch(yearsUrl)
      const data = await res.json()
      this.years = data
    },
    async getAthleteOrGamesOrCoach () {
      if (this.selected.category === 'athlete') {
        this.selected.gameNid = undefined
        this.selected.coachNid = undefined
        if (this.selected.sport && this.selected.year) {
          const athletesUrl = `${this.tempAthletesUrl}/${this.selected.sport}/${this.selected.year}`
          // console.log('athletesUrl', athletesUrl)
          // ======= fetch from URL API =======
          const res = await fetch(athletesUrl)
          const data = await res.json()
          this.athletes = data
          // console.log('this.athletes', this.athletes)
        }
      } else if (this.selected.category === 'game') {
        this.selected.athleteNid = undefined
        this.selected.coachNid = undefined
        if (this.selected.sport && this.selected.year) {
          const gamesUrl = `${this.tempGamesUrl}/${this.selected.sport}/${this.selected.year}`
          // console.log('gamesUrl', gamesUrl)
          // ======= fetch from URL API =======
          const res = await fetch(gamesUrl)
          const data = await res.json()
          this.games = data
          // console.log('this.games', this.games)
        }
      } else if (this.selected.category === 'coach') {
        this.selected.athleteNid = undefined
        this.selected.gameNid = undefined
        if (this.selected.sport && this.selected.year) {
          const coachUrl = `${this.tempCoachUrl}/${this.selected.sport}/${this.selected.year}`
          // console.log('coachUrl', coachUrl)
          // ======= fetch from URL API =======
          const res = await fetch(coachUrl)
          const data = await res.json()
          this.coaches = data
          // console.log('this.coaches', this.coaches)
        }
      }
    }
  }
}
</script>

<style scoped>
.query {
  background-color: white;
  box-shadow: 0px 0px 5px 1px rgb(0 0 0 / 20%);
  border-radius: 5px;
  position: relative;
}

.x-button {
  position: absolute;
  top: 0;
  right: 0;
}

#submit:hover {
  color: #0d6efd;
  background-color: white;
}

#submit {
  color: white;
  background-color: #0d6efd;
}
</style>
