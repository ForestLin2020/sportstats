<template>
 <h5>Returner Table</h5>
  <div>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
              <th colspan="2">Career</th>
              <th colspan="5">Kickoff Returns</th>
              <th colspan="5">Punt Returns</th>
              <th colspan="2">Fumbles</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>KR</th>
            <th>YDS</th>
            <th>AVG</th>
            <th>LNG</th>
            <th>TD</th>
            <th>PR</th>
            <th>YDS</th>
            <th>AVG</th>
            <th>LNG</th>
            <th>TD</th>
            <th>NO</th>
            <th>LOST</th>
        </tr>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.kr.no }}</td>
            <td>{{ totals.kr.yds }}</td>
            <td>{{ totals.kr.avg }}</td>
            <td>{{ totals.kr.long }}</td>
            <td>{{ totals.kr.tb }}</td>
            <td>{{ totals.pr.no }}</td>
            <td>{{ totals.pr.yds }}</td>
            <td>{{ totals.pr.avg }}</td>
            <td>{{ totals.pr.long }}</td>
            <td>{{ totals.pr.tb }}</td>
            <td>{{ totals.fumbles.no }}</td>
            <td>{{ totals.fumbles.lost }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
            <th v-if="careerTotals.kr && careerTotals.kr.no" nowrap>{{ careerTotals.kr.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.kr && careerTotals.kr.yds" nowrap>{{ careerTotals.kr.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.kr && careerTotals.kr.avg" nowrap>{{ careerTotals.kr.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.kr && careerTotals.kr.long" nowrap>{{ careerTotals.kr.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.kr && careerTotals.kr.td" nowrap>{{ careerTotals.kr.td }}</th><th v-else>0</th>
            <th v-if="careerTotals.pr && careerTotals.pr.no" nowrap>{{ careerTotals.pr.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.pr && careerTotals.pr.yds" nowrap>{{ careerTotals.pr.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.pr && careerTotals.pr.avg" nowrap>{{ careerTotals.pr.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.pr && careerTotals.pr.long" nowrap>{{ careerTotals.pr.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.pr && careerTotals.pr.td" nowrap>{{ careerTotals.pr.td }}</th><th v-else>0</th>
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
              <th colspan="5">Kickoff Returns</th>
              <th colspan="5">Punt Returns</th>
              <th colspan="2">Fumbles</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>KR</th>
              <th>YDS</th>
              <th>AVG</th>
              <th>LNG</th>
              <th>TD</th>
              <th>PR</th>
              <th>YDS</th>
              <th>AVG</th>
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
              <td v-if="game.kr && game.kr.no" nowrap>{{ game.kr.no }}</td><td v-else>-</td>
              <td v-if="game.kr && game.kr.yds" nowrap>{{ game.kr.yds }}</td><td v-else>-</td>
              <td v-if="game.kr && game.kr.no && game.kr.yds" nowrap>{{ (game.kr.yds / game.kr.no).toFixed(2) }}</td><td v-else>-</td>
              <td v-if="game.kr && game.kr.long" nowrap>{{ game.kr.long }}</td><td v-else>-</td>
              <td v-if="game.kr && game.kr.td" nowrap>{{ game.kr.td }}</td><td v-else>-</td>
              <td v-if="game.pr && game.pr.no" nowrap>{{ game.pr.no }}</td><td v-else>-</td>
              <td v-if="game.pr && game.pr.yds" nowrap>{{ game.pr.yds }}</td><td v-else>-</td>
              <td v-if="game.pr && game.pr.no && game.pr.yds" nowrap>{{ (game.pr.yds / game.pr.no).toFixed(2) }}</td><td v-else>-</td>
              <td v-if="game.pr && game.pr.long" nowrap>{{ game.pr.long }}</td><td v-else>-</td>
              <td v-if="game.pr && game.pr.td" nowrap>{{ game.pr.td }}</td><td v-else>-</td>
              <td v-if="game.fumbles && game.fumbles.no" nowrap>{{ game.fumbles.no }}</td><td v-else>-</td>
              <td v-if="game.fumbles && game.fumbles.lost" nowrap>{{ game.fumbles.lost }}</td><td v-else>-</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.kr.no }}</th>
              <th>{{ totals.kr.yds }}</th>
              <th>{{ totals.kr.avg }}</th>
              <th>{{ totals.kr.long }}</th>
              <th>{{ totals.kr.tb }}</th>
              <th>{{ totals.pr.no }}</th>
              <th>{{ totals.pr.yds }}</th>
              <th>{{ totals.pr.avg }}</th>
              <th>{{ totals.pr.long }}</th>
              <th>{{ totals.pr.tb }}</th>
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
  name: 'Returner',
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
      totalsByYear: [],
      careerTotals: []
    }
  },
  computed: {
    // do something
  },
  mounted () {
    this.calTotalsByYear()
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
      return this.gamesRecordPlayerInCleared.filter(game => game.schedule_year === year).sort(this.dateSort)
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
    calTotalsByYear () {
      for (let i = 0; i < this.gameYears.length; i++) {
        const gamesByYear = this.gamesFilterEventsByYear(this.gameYears[i])
        const gamesTotalByYear = {
          schedule_year: this.gameYears[i],
          gp: this.getGP(gamesByYear),
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          kr: {
            no: this.calTotal(gamesByYear, 'kr', 'no'),
            yds: this.calTotal(gamesByYear, 'kr', 'yds'),
            avg: this.getAVG(this.calTotal(gamesByYear, 'kr', 'yds'), this.calTotal(gamesByYear, 'kr', 'no')), // (game.punt.yds / game.punt.no).toFixed(2)
            long: this.getBiggestLongByYear(gamesByYear, 'kr', 'long'),
            tb: this.calTotal(gamesByYear, 'kr', 'tb')
          },
          pr: {
            no: this.calTotal(gamesByYear, 'pr', 'no'),
            yds: this.calTotal(gamesByYear, 'pr', 'yds'),
            avg: this.getAVG(this.calTotal(gamesByYear, 'pr', 'yds'), this.calTotal(gamesByYear, 'pr', 'no')), // (game.punt.yds / game.punt.no).toFixed(2)
            long: this.getBiggestLongByYear(gamesByYear, 'pr', 'long'),
            tb: this.calTotal(gamesByYear, 'pr', 'tb')
          },
          fumbles: {
            no: this.calTotal(gamesByYear, 'fumbles', 'no'),
            lost: this.calTotal(gamesByYear, 'fumbles', 'lost')
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
          kr: {
            no: this.calTotal(this.totalsByYear, 'kr', 'no'),
            yds: this.calTotal(this.totalsByYear, 'kr', 'yds'),
            avg: this.getAVG(this.calTotal(this.totalsByYear, 'kr', 'yds'), this.calTotal(this.totalsByYear, 'kr', 'no')), // (game.punt.yds / game.punt.no).toFixed(2)
            long: this.getBiggestLongByYear(this.totalsByYear, 'kr', 'long'),
            tb: this.calTotal(this.totalsByYear, 'kr', 'tb')
          },
          pr: {
            no: this.calTotal(this.totalsByYear, 'pr', 'no'),
            yds: this.calTotal(this.totalsByYear, 'pr', 'yds'),
            avg: this.getAVG(this.calTotal(this.totalsByYear, 'pr', 'yds'), this.calTotal(this.totalsByYear, 'pr', 'no')), // (game.punt.yds / game.punt.no).toFixed(2)
            long: this.getBiggestLongByYear(this.totalsByYear, 'pr', 'long'),
            tb: this.calTotal(this.totalsByYear, 'pr', 'tb')
          },
          fumbles: {
            made: this.calTotal(this.totalsByYear, 'fumbles', 'no'),
            att: this.calTotal(this.totalsByYear, 'fumbles', 'lost')
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
    getBiggestLongByYear (games, categoryKey, statsKey) {
      const longs = [0]
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i][categoryKey] && games[i][categoryKey][statsKey]) {
          longs.push(games[i][categoryKey][statsKey] - 0)
        }
      }

      return Math.max(...longs)
    },
    getAVG (yds, no) {
      if (no === 0) return 0
      const avg = (yds / no).toFixed(2)
      return avg
    },
    getPCT (made, att) {
      // (game.fg.made / game.fg.att * 100).toFixed(2)
      if (att === 0) return 0
      const pct = (made / att * 100).toFixed(2)
      return pct
    },
    getPTS (made, kickmade) {
      // game.fg.made * 3 + (game.pat.kickmade - 0)
      const pts = made * 3 + (kickmade - 0)
      return pts
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
