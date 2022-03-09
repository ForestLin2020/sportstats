<template>
 <h5>Goalkeeper</h5>
  <div class="career-and-season">
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th colspan="6">Career & Season Stats</th>
          </tr>
          <tr>
            <th>Year</th>
            <th >GP</th>
            <th >GS</th>
            <th >GA</th>
            <th >Sav</th>
            <th >Min</th>
          </tr>
        </thead>
        <tbody>

          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td nowrap>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.gs }}</td>
            <td>{{ totals.goalie.ga }}</td>
            <td>{{ totals.goalie.saves }}</td>
            <td>{{ totals.misc.minutes }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th v-if="careerTotals.gp">{{ careerTotals.gp }}</th><th v-else>0</th>
            <th v-if="careerTotals.gs">{{ careerTotals.gs }}</th><th v-else>0</th>
            <th v-if="careerTotals.goalie && careerTotals.goalie.ga">{{ careerTotals.goalie.ga }}</th><th v-else>0</th>
            <th v-if="careerTotals.goalie && careerTotals.goalie.saves">{{ careerTotals.goalie.saves }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc && careerTotals.misc.minutes">{{ careerTotals.misc.minutes }}</th><th v-else>0</th>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th colspan="7">{{ year }} Games</th>
            </tr>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>GS</th>
              <th>GA</th>
              <th>Sav</th>
              <th>Min</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.homename != 'BYU' && game.venue.neutralgame != 'Y'">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td v-if="game.gs == '1'">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-check-lg" viewBox="0 0 16 16">
                  <path d="M12.736 3.97a.733.733 0 0 1 1.047 0c.286.289.29.756.01 1.05L7.88 12.01a.733.733 0 0 1-1.065.02L3.217 8.384a.757.757 0 0 1 0-1.06.733.733 0 0 1 1.047 0l3.052 3.093 5.4-6.425a.247.247 0 0 1 .02-.022Z"/>
                </svg>
              </td><td v-else></td>
              <td v-if="game.goalie && game.goalie.ga" nowrap>{{ game.goalie.ga }}</td><td v-else>0</td>
              <td v-if="game.goalie && game.goalie.saves" nowrap>{{ game.goalie.saves }}</td><td v-else>0</td>
              <td v-if="game.misc && game.misc.minutes" nowrap>{{ game.misc.minutes }}</td><td v-else>0</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.gs }}</th>
              <th>{{ totals.goalie.ga }}</th>
              <th>{{ totals.goalie.saves }}</th>
              <th>{{ totals.misc.minutes }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Goalkeeper',
  props: ['selected', 'gamesRecordPlayerInCleared', 'gameYears'],
  data () {
    return {
      // For UI to format dates
      months: [
        'Jan.', // "January",
        'Feb.', // "February",
        'Mar.', // "March",
        'Apr.', // "April",
        'May.', // "May",
        'Jun.', // "June",
        'Jul.', // "July",
        'Aug.', // "August",
        'Sep.', // "September",
        'Oct.', // "October",
        'Nov.', // "November",
        'Dec.' // "December",
      ],
      games: [],
      totalsByYear: [],
      careerTotals: []
    }
  },
  computed: {
    // do something
  },
  mounted () {
    this.games = this.gamesRecordPlayerInCleared
    this.calTotalsByYear(this.gameYears)
  },
  methods: {
    formDateStr (str) {
      var date = new Date(str)
      // need to double check if area different then show different data
      date.setHours(date.getHours() - 6)
      var dateStr = this.months[date.getMonth()] + ' ' + date.getDate()
      return dateStr
    },
    gamesFilterEventsByYear (year) {
      return this.games.filter(game => game.schedule_year === year).sort(this.dateSort)
    },
    dateSort (a, b) {
      if (a.event_date < b.event_date) {
        return -1
      }
      if (a.event_date > b.event_date) {
        return 1
      }
      return 0
    },
    calTotalsByYear (years) {
      for (let i = 0; i < years.length; i++) {
        const gamesByYear = this.gamesFilterEventsByYear(years[i])
        const gamesTotalByYear = {
          schedule_year: years[i],
          gp: this.getGP(gamesByYear),
          gs: this.getGS(gamesByYear),
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          goalie: {
            ga: this.calTotal(gamesByYear, 'goalie', 'ga'),
            saves: this.calTotal(gamesByYear, 'goalie', 'saves')
          },
          misc: {
            minutes: this.calTotal(gamesByYear, 'misc', 'minutes')
          }
        }
        this.totalsByYear.push(gamesTotalByYear)
      }
      this.calCareerTotals()
    },
    calCareerTotals () {
      if (this.totalsByYear) {
        this.careerTotals = {
          gp: this.getGP(this.totalsByYear),
          gs: this.getGS(this.totalsByYear),
          goalie: {
            ga: this.calTotal(this.totalsByYear, 'goalie', 'ga'),
            saves: this.calTotal(this.totalsByYear, 'goalie', 'saves')
          },
          misc: {
            minutes: this.calTotal(this.totalsByYear, 'misc', 'minutes')
          }
        }
      }
    },
    getGP (games) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i].gp) {
          total += games[i].gp - 0
        }
      }
      return total
    },
    getGS (games) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i].gs) {
          total += games[i].gs - 0
        }
      }
      return total
    },
    getWinGame (games) {
      let counter = 0
      for (let i = 0; i < games.length; i++) {
        if (parseInt(games[i].byu_score) > parseInt(games[i].opp_score)) counter += 1
      }
      return counter
    },
    getLoseGame (games) {
      let counter = 0
      for (let i = 0; i < games.length; i++) {
        if (parseInt(games[i].byu_score) < parseInt(games[i].opp_score)) counter += 1
      }
      return counter
    },
    calTotal (games, categoryKey, statsKey) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i][categoryKey] && games[i][categoryKey][statsKey]) {
          total += games[i][categoryKey][statsKey] - 0
        }
      }
      return total
    },
    getTotalsByYear (year) {
      return this.totalsByYear.filter(total => total.schedule_year === year)
    }
  }
}
</script>

<style scoped>

thead {
  color: white;
  background: #0b5ed7;
}

</style>
