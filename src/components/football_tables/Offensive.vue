<template>
 <h5>Offensive</h5>
  <div class="career-and-season">
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th colspan="2">Career</th>
            <th colspan="5">Receiving</th>
            <th colspan="5">Rushing</th>
            <th colspan="2">Fumbles</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>REC</th>
            <th>YDS</th>
            <th>Y/R</th>
            <th>LNG</th>
            <th>TD</th>
            <th>ATT</th>
            <th>YDS</th>
            <th>Y/A</th>
            <th>LNG</th>
            <th>TD</th>
            <th>NO</th>
            <th>LOST</th>
          </tr>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.rcv.no }}</td>
            <td>{{ totals.rcv.yds }}</td>
            <td>{{ totals.rcv.yr }}</td>
            <td>{{ totals.rcv.long }}</td>
            <td>{{ totals.rcv.td }}</td>
            <td>{{ totals.rush.att }}</td>
            <td>{{ totals.rush.yds }}</td>
            <td>{{ totals.rush.ya }}</td>
            <td>{{ totals.rush.long }}</td>
            <td>{{ totals.rush.td }}</td>
            <td>{{ totals.fumbles.no }}</td>
            <td>{{ totals.fumbles.lost }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
            <th v-if="careerTotals.rcv && careerTotals.rcv.no" nowrap>{{ careerTotals.rcv.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.rcv && careerTotals.rcv.yds" nowrap>{{ careerTotals.rcv.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.rcv && careerTotals.rcv.yr" nowrap>{{ careerTotals.rcv.yr }}</th><th v-else>0</th>
            <th v-if="careerTotals.rcv && careerTotals.rcv.long" nowrap>{{ careerTotals.rcv.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.rcv && careerTotals.rcv.td" nowrap>{{ careerTotals.rcv.td }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.att" nowrap>{{ careerTotals.rush.att }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.yds" nowrap>{{ careerTotals.rush.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.ya" nowrap>{{ careerTotals.rush.ya }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.long" nowrap>{{ careerTotals.rush.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.td" nowrap>{{ careerTotals.rush.td }}</th><th v-else>0</th>
            <th v-if="careerTotals.fumbles && careerTotals.fumbles.no" nowrap>{{ careerTotals.fumbles.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.fumbles && careerTotals.fumbles.lost" nowrap>{{ careerTotals.fumbles.lost }}</th><th v-else>0</th>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th colspan="3">{{ year }} Games</th>
              <th colspan="5">Receiving</th>
              <th colspan="5">Rushing</th>
              <th colspan="2">Fumbles</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>REC</th>
              <th>YDS</th>
              <th>Y/R</th>
              <th>LNG</th>
              <th>TD</th>
              <th>ATT</th>
              <th>YDS</th>
              <th>Y/A</th>
              <th>LNG</th>
              <th>TD</th>
              <th>NO</th>
              <th>LOST</th>
            </tr>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td v-if="game.rcv && game.rcv.no" nowrap>{{ game.rcv.no }}</td><td v-else>0</td>
              <td v-if="game.rcv && game.rcv.yds" nowrap>{{ game.rcv.yds }}</td><td v-else>0</td>
              <td v-if="game.rcv && game.rcv.yds && game.rcv.no && game.rcv.no!=0" nowrap>{{ (game.rcv.yds / game.rcv.no).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.rcv && game.rcv.long" nowrap>{{ game.rcv.long }}</td><td v-else>0</td>
              <td v-if="game.rcv && game.rcv.td" nowrap>{{ game.rcv.td }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.att" nowrap>{{ game.rush.att }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.yds" nowrap>{{ game.rush.yds }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.yds && game.rush.att && game.rush.att!=0" nowrap>{{ (game.rush.yds / game.rush.att).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.long" nowrap>{{ game.rush.long }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.td" nowrap>{{ game.rush.td }}</td><td v-else>0</td>
              <td v-if="game.fumbles && game.fumbles.no" nowrap>{{ game.fumbles.no }}</td><td v-else>0</td>
              <td v-if="game.fumbles && game.fumbles.lost" nowrap>{{ game.fumbles.lost }}</td><td v-else>0</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.rcv.no }}</th>
              <th>{{ totals.rcv.yds }}</th>
              <th>{{ totals.rcv.yr }}</th>
              <th>{{ totals.rcv.long }}</th>
              <th>{{ totals.rcv.td }}</th>
              <th>{{ totals.rush.att }}</th>
              <th>{{ totals.rush.yds }}</th>
              <th>{{ totals.rush.ya }}</th>
              <th>{{ totals.rush.long }}</th>
              <th>{{ totals.rush.td }}</th>
              <th>{{ totals.fumbles.no }}</th>
              <th>{{ totals.fumbles.lost }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Offensive',
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
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          rcv: {
            no: this.calTotal(gamesByYear, 'rcv', 'no'),
            yds: this.calTotal(gamesByYear, 'rcv', 'yds'),
            yr: this.getYR(this.calTotal(gamesByYear, 'rcv', 'yds'), this.calTotal(gamesByYear, 'rcv', 'no')), // (rcv.yds / rcv.no)
            long: this.getBiggestLongByYear(gamesByYear, 'rcv', 'long'),
            td: this.calTotal(gamesByYear, 'rcv', 'td')
          },
          rush: {
            att: this.calTotal(gamesByYear, 'rush', 'att'),
            yds: this.calTotal(gamesByYear, 'rush', 'yds'),
            ya: this.getYA(this.calTotal(gamesByYear, 'rush', 'yds'), this.calTotal(gamesByYear, 'rush', 'att')), // (rush.yds / rush.att)
            long: this.getBiggestLongByYear(gamesByYear, 'rush', 'long'),
            td: this.calTotal(gamesByYear, 'rush', 'td')
          },
          fumbles: {
            lost: this.calTotal(gamesByYear, 'fumbles', 'lost'),
            no: this.calTotal(gamesByYear, 'fumbles', 'no')
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
          rcv: {
            no: this.calTotal(this.totalsByYear, 'rcv', 'no'),
            yds: this.calTotal(this.totalsByYear, 'rcv', 'yds'),
            yr: this.getYR(this.calTotal(this.totalsByYear, 'rcv', 'yds'), this.calTotal(this.totalsByYear, 'rcv', 'no')), // (rcv.yds / rcv.no)
            long: this.getBiggestLongByYear(this.totalsByYear, 'rcv', 'long'),
            td: this.calTotal(this.totalsByYear, 'rcv', 'td')
          },
          rush: {
            att: this.calTotal(this.totalsByYear, 'rush', 'att'),
            yds: this.calTotal(this.totalsByYear, 'rush', 'yds'),
            ya: this.getYA(this.calTotal(this.totalsByYear, 'rush', 'yds'), this.calTotal(this.totalsByYear, 'rush', 'att')), // (rush.yds / rush.att)
            long: this.getBiggestLongByYear(this.totalsByYear, 'rush', 'long'),
            td: this.calTotal(this.totalsByYear, 'rush', 'td')
          },
          fumbles: {
            lost: this.calTotal(this.totalsByYear, 'fumbles', 'lost'),
            no: this.calTotal(this.totalsByYear, 'fumbles', 'no')
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
    getWinGame (games) {
      let counter = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i].byu_score > games[i].opp_score) counter += 1
      }
      return counter
    },
    getLoseGame (games) {
      let counter = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i].byu_score < games[i].opp_score) counter += 1
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
    getBiggestLongByYear (games, categoryKey, statsKey) {
      const longs = [0]
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i][categoryKey] && games[i][categoryKey][statsKey]) {
          longs.push(games[i][categoryKey][statsKey] - 0)
        }
      }

      return Math.max(...longs)
    },
    getYA (yds, att) {
      if (att === 0) return 0
      const ya = (yds / att).toFixed(2)
      return ya
    },
    getYR (yds, no) {
      // rcv.yds / rcv.no
      if (no === 0) return 0
      const yr = (yds / no).toFixed(2)
      return yr
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
