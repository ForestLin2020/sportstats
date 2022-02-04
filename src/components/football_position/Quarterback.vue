<template>
 <h5>Quarterback</h5>
  <div class="career-and-season">
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead class='football-thead'>
          <tr>
            <th colspan="2">Career</th>
            <th colspan="9">Passing</th>
            <th colspan="5">Rushing</th>
            <th colspan="2">Sacked</th>
            <th colspan="2">Fumbles</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>COMP</th>
            <th>ATT</th>
            <th>YDS</th>
            <th>Y/A</th>
            <th>PCT</th>
            <th>LNG</th>
            <th>INT</th>
            <th>TD</th>
            <th>EFFIC</th>
            <th>ATT</th>
            <th>YDS</th>
            <th>Y/A</th>
            <th>LNG</th>
            <th>TD</th>
            <th>NO</th>
            <th>YDS</th>
            <th>NO</th>
            <th>LOST</th>
          </tr>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.pass.comp }}</td>
            <td>{{ totals.pass.att }}</td>
            <td>{{ totals.pass.yds }}</td>
            <td>{{ totals.pass.ya }}</td>
            <td>{{ totals.pass.pct }}</td>
            <td>{{ totals.pass.long }}</td>
            <td>{{ totals.pass.int }}</td>
            <td>{{ totals.pass.td }}</td>
            <td>{{ totals.pass.effic }}</td>
            <td>{{ totals.rush.att }}</td>
            <td>{{ totals.rush.yds }}</td>
            <td>{{ totals.rush.ya }}</td>
            <td>{{ totals.rush.long }}</td>
            <td>{{ totals.rush.td }}</td>
            <td>{{ totals.pass.sacks }}</td>
            <td>{{ totals.pass.sackyds }}</td>
            <td>{{ totals.fumbles.no }}</td>
            <td>{{ totals.fumbles.lost }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
            <th v-if="careerTotals.pass && careerTotals.pass.comp" nowrap>{{ careerTotals.pass.comp }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.att" nowrap>{{ careerTotals.pass.att }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.yds" nowrap>{{ careerTotals.pass.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.ya" nowrap>{{ careerTotals.pass.ya }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.pct" nowrap>{{ careerTotals.pass.pct }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.long" nowrap>{{ careerTotals.pass.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.int" nowrap>{{ careerTotals.pass.int }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.td" nowrap>{{ careerTotals.pass.td }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.effic" nowrap>{{ careerTotals.pass.effic }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.att" nowrap>{{ careerTotals.rush.att }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.yds" nowrap>{{ careerTotals.rush.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.ya" nowrap>{{ careerTotals.rush.ya }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.long" nowrap>{{ careerTotals.rush.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.rush && careerTotals.rush.td" nowrap>{{ careerTotals.rush.td }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.sacks" nowrap>{{ careerTotals.pass.sacks }}</th><th v-else>0</th>
            <th v-if="careerTotals.pass && careerTotals.pass.sackyds" nowrap>{{ careerTotals.pass.sackyds }}</th><th v-else>0</th>
            <th v-if="careerTotals.fumbles && careerTotals.fumbles.no" nowrap>{{ careerTotals.fumbles.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.fumbles && careerTotals.fumbles.lost" nowrap>{{ careerTotals.fumbles.lost }}</th><th v-else>0</th>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead class='football-thead'>
            <tr>
              <th colspan="3">{{ year }} Games</th>
              <th colspan="9">Passing</th>
              <th colspan="5">Rushing</th>
              <th colspan="2">Sacked</th>
              <th colspan="2">Fumbles</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>COMP</th>
              <th>ATT</th>
              <th>YDS</th>
              <th>Y/A</th>
              <th>PCT</th>
              <th>LNG</th>
              <th>INT</th>
              <th>TD</th>
              <th>EFFIC</th>
              <th>ATT</th>
              <th>YDS</th>
              <th>Y/A</th>
              <th>LNG</th>
              <th>TD</th>
              <th>NO</th>
              <th>YDS</th>
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
              <td v-if="game.pass && game.pass.comp" nowrap>{{ game.pass.comp }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.att" nowrap>{{ game.pass.att }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.yds" nowrap>{{ game.pass.yds }}</td><td v-else>0</td>
              <!-- Y/A: yds / att -->
              <td v-if="game.pass && game.pass.yds && game.pass.att" nowrap>{{ (game.pass.yds / game.pass.att).toFixed(2) }}</td><td v-else>0</td>
              <!-- PCT: comp / att -->
              <td v-if="game.pass && game.pass.comp && game.pass.att" nowrap>{{ (game.pass.comp / game.pass.att).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.long" nowrap>{{ game.pass.long }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.int" nowrap>{{ game.pass.int }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.td" nowrap>{{ game.pass.td }}</td><td v-else>0</td>
              <!-- EFFIC: (8.4*yds + 330*td + 100*comp - 200*int)/ att -->
              <td v-if="game.pass && game.pass.yds && game.pass.td && game.pass.comp && game.pass.int && game.pass.att" nowrap>{{ (((8.4*game.pass.yds)+(330*game.pass.td)+(100*game.pass.comp)-(200*game.pass.int))/game.pass.att).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.att" nowrap>{{ game.rush.att }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.yds" nowrap>{{ game.rush.yds }}</td><td v-else>0</td>
              <!-- Y/A: yds / att -->
              <td v-if="game.rush && game.rush.yds && game.rush.att" nowrap>{{ (game.rush.yds/game.rush.att).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.long" nowrap>{{ game.rush.long }}</td><td v-else>0</td>
              <td v-if="game.rush && game.rush.td" nowrap>{{ game.rush.td }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.sacks" nowrap>{{ game.pass.sacks }}</td><td v-else>0</td>
              <td v-if="game.pass && game.pass.sackyds" nowrap>{{ game.pass.sackyds }}</td><td v-else>0</td>
              <td v-if="game.fumbles && game.fumbles.no" nowrap>{{ game.fumbles.no }}</td><td v-else>0</td>
              <td v-if="game.fumbles && game.fumbles.lost" nowrap>{{ game.fumbles.lost }}</td><td v-else>0</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.pass.comp }}</th>
              <th>{{ totals.pass.att }}</th>
              <th>{{ totals.pass.yds }}</th>
              <th>{{ totals.pass.ya }}</th>
              <th>{{ totals.pass.pct }}</th>
              <th>{{ totals.pass.long }}</th>
              <th>{{ totals.pass.int }}</th>
              <th>{{ totals.pass.td }}</th>
              <th>{{ totals.pass.effic }}</th>
              <th>{{ totals.rush.att }}</th>
              <th>{{ totals.rush.yds }}</th>
              <th>{{ totals.rush.ya }}</th>
              <th>{{ totals.rush.long }}</th>
              <th>{{ totals.rush.td }}</th>
              <th>{{ totals.pass.sacks }}</th>
              <th>{{ totals.pass.sackyds }}</th>
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
  name: 'Quarterback',
  props: ['selected', 'gamesRecordPlayerInCleared', 'gameYears'],
  data () {
    return {
      // For UI to format dates
      months: [
        'Jan.', // "January",
        'Feb.', // "February",
        'Mar.', // "March",
        'Apr.', // "April",
        'May"', // "May",
        'Jun.', // "June",
        'Jul.', // "July",
        'Aug.', // "August",
        'Sep.', // "September",
        'Oct.', // "October",
        'Nov.', // "November",
        'Dec.' // "December",
      ],
      playerNid: 1285416,
      games: [],
      // gameYears: [],
      totalsByYear: [],
      careerTotals: []
    }
  },
  computed: {
    // do something
  },
  mounted () {
    console.log('selected', this.selected)
    console.log('gamesRecordPlayerInCleared', this.gamesRecordPlayerInCleared)
    console.log('gameYears', this.gameYears)
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
      return this.games.filter(game => game.schedule_year === year)
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
          pass: {
            att: this.calTotal(gamesByYear, 'pass', 'att'),
            comp: this.calTotal(gamesByYear, 'pass', 'comp'),
            int: this.calTotal(gamesByYear, 'pass', 'int'),
            long: this.getBiggestLongByYear(gamesByYear, 'pass', 'long'),
            sacks: this.calTotal(gamesByYear, 'pass', 'sacks'),
            sackyds: this.calTotal(gamesByYear, 'pass', 'sackyds'),
            td: this.calTotal(gamesByYear, 'pass', 'td'),
            yds: this.calTotal(gamesByYear, 'pass', 'yds'),
            ya: this.get_YA(this.calTotal(gamesByYear, 'pass', 'yds'), this.calTotal(gamesByYear, 'pass', 'att')),
            pct: this.get_PCT(this.calTotal(gamesByYear, 'pass', 'comp'), this.calTotal(gamesByYear, 'pass', 'att')),
            effic: this.get_EFFIC(
              this.calTotal(gamesByYear, 'pass', 'yds'),
              this.calTotal(gamesByYear, 'pass', 'td'),
              this.calTotal(gamesByYear, 'pass', 'comp'),
              this.calTotal(gamesByYear, 'pass', 'int'),
              this.calTotal(gamesByYear, 'pass', 'att')
            )
          },
          rush: {
            att: this.calTotal(gamesByYear, 'rush', 'att'),
            long: this.getBiggestLongByYear(gamesByYear, 'rush', 'long'),
            td: this.calTotal(gamesByYear, 'rush', 'td'),
            yds: this.calTotal(gamesByYear, 'rush', 'yds'),
            ya: this.get_YA(this.calTotal(gamesByYear, 'rush', 'yds'), this.calTotal(gamesByYear, 'rush', 'att'))
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
          pass: {
            att: this.calTotal(this.totalsByYear, 'pass', 'att'),
            comp: this.calTotal(this.totalsByYear, 'pass', 'comp'),
            int: this.calTotal(this.totalsByYear, 'pass', 'int'),
            long: this.getBiggestLongByYear(this.totalsByYear, 'pass', 'long'),
            sacks: this.calTotal(this.totalsByYear, 'pass', 'sacks'),
            sackyds: this.calTotal(this.totalsByYear, 'pass', 'sackyds'),
            td: this.calTotal(this.totalsByYear, 'pass', 'td'),
            yds: this.calTotal(this.totalsByYear, 'pass', 'yds'),
            ya: this.get_YA(this.calTotal(this.totalsByYear, 'pass', 'yds'), this.calTotal(this.totalsByYear, 'pass', 'att')),
            pct: this.get_PCT(this.calTotal(this.totalsByYear, 'pass', 'comp'), this.calTotal(this.totalsByYear, 'pass', 'att')),
            effic: this.get_EFFIC(
              this.calTotal(this.totalsByYear, 'pass', 'yds'),
              this.calTotal(this.totalsByYear, 'pass', 'td'),
              this.calTotal(this.totalsByYear, 'pass', 'comp'),
              this.calTotal(this.totalsByYear, 'pass', 'int'),
              this.calTotal(this.totalsByYear, 'pass', 'att')
            )
          },
          rush: {
            att: this.calTotal(this.totalsByYear, 'rush', 'att'),
            long: this.getBiggestLongByYear(this.totalsByYear, 'rush', 'long'),
            td: this.calTotal(this.totalsByYear, 'rush', 'td'),
            yds: this.calTotal(this.totalsByYear, 'rush', 'yds'),
            ya: this.get_YA(this.calTotal(this.totalsByYear, 'rush', 'yds'), this.calTotal(this.totalsByYear, 'rush', 'att'))
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
    get_YA (yds, att) {
      if (att === 0) return 0
      const ya = (yds / att).toFixed(2)
      return ya
    },
    get_PCT (comp, att) {
      if (att === 0) return 0
      const pct = (comp / att).toFixed(2)
      return pct
    },
    get_EFFIC (yds, td, comp, int, att) {
      if (att === 0) return 0
      const effic = ((8.4 * yds + 330 * td + 100 * comp - 200 * int) / att).toFixed(2)
      return effic
    },
    getTotalsByYear (year) {
      return this.totalsByYear.filter(total => total.schedule_year === year)
    }
  }
}
</script>

<style scoped>

.football-thead {
  color: white;
  background: #0b5ed7;
}

</style>
