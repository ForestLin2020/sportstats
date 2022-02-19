<template>
 <h5>Defensive</h5>
  <div>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th colspan="2">Career</th>
            <th colspan="4">Tackles</th>
            <th colspan="2">Sacks</th>
            <th colspan="3">Interceptions</th>
            <th colspan="5">Misc</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th title="Solo Tackles">SOLO</th>
            <th title="Assisted Tackles">AST</th>
            <th title="Total Tackles">TOT</th>
            <th title="Tackles For A Loss / Yards">TFL/YDS</th>
            <th title="Number Of Sacks">NO</th>
            <th title="Yards Lost On Sacks">YDS</th>
            <th title="Interceptions">INT</th>
            <th title="Yards After Interception">YDS</th>
            <th title="Touchdowns from Interceptions">TD</th>
            <th title="Forced Fumbles">FF</th>
            <th title="Fumble Recoveries">FR</th>
            <th title="Yards After Fumble Recovery">FRYDS</th>
            <th title="Quarterback Hurries">QBH</th>
            <th title="Pass Breakups">PBU</th>
          </tr>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.defense.tackua }}</td>
            <td>{{ totals.defense.tacka }}</td>
            <td>{{ totals.defense.tot_tack }}</td>
            <td>{{ totals.defense.tfla*0.5 + totals.defense.tflua }} / {{ totals.defense.tflyds }}</td>
            <td>{{ totals.defense.sackua }}</td>
            <td>{{ totals.defense.sackyds }}</td>
            <td>{{ totals.ir.no }}</td>
            <td>{{ totals.ir.yds }}</td>
            <td>{{ totals.ir.td }}</td>
            <td>{{ totals.defense.ff }}</td>
            <td>{{ totals.defense.fr }}</td>
            <td>{{ totals.defense.fryds }}</td>
            <td>{{ totals.defense.qbh }}</td>
            <td>{{ totals.defense.brup }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
            <th v-if="careerTotals.defense && careerTotals.defense.tackua" nowrap>{{ careerTotals.defense.tackua }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.tacka" nowrap>{{ careerTotals.defense.tacka }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.tot_tack" nowrap>{{ careerTotals.defense.tot_tack }}</th><th v-else>0</th>
            <th>
            <span v-if="careerTotals.defense && careerTotals.defense.tfla && careerTotals.defense.tflua" nowrap>{{ careerTotals.defense.tfla*(0.5) + careerTotals.defense.tflua }}</span><span v-else>0</span>
            <span> / </span>
            <span v-if="careerTotals.defense && careerTotals.defense.tflyds" nowrap>{{ careerTotals.defense.tflyds }}</span><span v-else>0</span>
            </th>
            <th v-if="careerTotals.defense && careerTotals.defense.sackua" nowrap>{{ careerTotals.defense.sackua }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.sackyds" nowrap>{{ careerTotals.defense.sackyds }}</th><th v-else>0</th>
            <th v-if="careerTotals.ir && careerTotals.ir.no" nowrap>{{ careerTotals.ir.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.ir && careerTotals.ir.yds" nowrap>{{ careerTotals.ir.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.ir && careerTotals.ir.td" nowrap>{{ careerTotals.ir.td }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.ff" nowrap>{{ careerTotals.defense.ff }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.fr" nowrap>{{ careerTotals.defense.fr }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.fryds" nowrap>{{ careerTotals.defense.fryds }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.qbh" nowrap>{{ careerTotals.defense.qbh }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.brup" nowrap>{{ careerTotals.defense.brup }}</th><th v-else>0</th>
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
              <th colspan="4">Tackles</th>
              <th colspan="2">Sacks</th>
              <th colspan="3">Interceptions</th>
              <th colspan="5">Misc</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th title="Solo Tackles">SOLO</th>
              <th title="Assisted Tackles">AST</th>
              <th title="Total Tackles">TOT</th>
              <th title="Tackles For A Loss / Yards">TFL/YDS</th>
              <th title="Number Of Sacks">NO</th>
              <th title="Yards Lost On Sacks">YDS</th>
              <th title="Interceptions">INT</th>
              <th title="Yards After Interception">YDS</th>
              <th title="Touchdowns from Interceptions">TD</th>
              <th title="Forced Fumbles">FF</th>
              <th title="Fumble Recoveries">FR</th>
              <th title="Yards After Fumble Recovery">FRYDS</th>
              <th title="Quarterback Hurries">QBH</th>
              <th title="Pass Breakups">PBU</th>
            </tr>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td v-if="game.defense && game.defense.tackua" nowrap>{{ game.defense.tackua }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.tacka" nowrap>{{ game.defense.tacka }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.tot_tack" nowrap>{{ game.defense.tot_tack }}</td><td v-else>0</td>
              <!-- (defense.tfla - 0) * .5 + (defense.tflua - 0) |ifEmpty:0 / (defense.tflyds) |ifEmpty:0 -->
              <td>
              <span v-if="game.defense && game.defense.tfla && game.defense.tflua" nowrap>{{ (game.defense.tfla-0)*(0.5) + (game.defense.tflua - 0) }}</span><span v-else>0</span>
              <span> / </span>
              <span v-if="game.defense && game.defense.tflyds" nowrap>{{ game.defense.tflyds }}</span><span v-else>0</span>
              </td>
              <td v-if="game.defense && game.defense.sackua" nowrap>{{ game.defense.sackua }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.sackyds" nowrap>{{ game.defense.sackyds }}</td><td v-else>0</td>
              <td v-if="game.ir && game.ir.no" nowrap>{{ game.ir.no }}</td><td v-else>0</td>
              <td v-if="game.ir && game.ir.yds" nowrap>{{ game.ir.yds }}</td><td v-else>0</td>
              <td v-if="game.ir && game.ir.td" nowrap>{{ game.ir.td }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.ff" nowrap>{{ game.defense.ff }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.fr" nowrap>{{ game.defense.fr }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.fryds" nowrap>{{ game.defense.fryds }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.qbh" nowrap>{{ game.defense.qbh }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.brup" nowrap>{{ game.defense.brup }}</td><td v-else>0</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.defense.tackua }}</th>
              <th>{{ totals.defense.tacka }}</th>
              <th>{{ totals.defense.tot_tack }}</th>
              <th>{{ totals.defense.tfla*0.5 + totals.defense.tflua }} / {{ totals.defense.tflyds }}</th>
              <th>{{ totals.defense.sackua }}</th>
              <th>{{ totals.defense.sackyds }}</th>
              <th>{{ totals.ir.no }}</th>
              <th>{{ totals.ir.yds }}</th>
              <th>{{ totals.ir.td }}</th>
              <th>{{ totals.defense.ff }}</th>
              <th>{{ totals.defense.fr }}</th>
              <th>{{ totals.defense.fryds }}</th>
              <th>{{ totals.defense.qbh }}</th>
              <th>{{ totals.defense.brup }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Defensive',
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
          defense: {
            tackua: this.calTotal(gamesByYear, 'defense', 'tackua'),
            tacka: this.calTotal(gamesByYear, 'defense', 'tacka'),
            tot_tack: this.calTotal(gamesByYear, 'defense', 'tot_tack'),
            tfla: this.calTotal(gamesByYear, 'defense', 'tfla'),
            tflua: this.calTotal(gamesByYear, 'defense', 'tflua'),
            tflyds: this.calTotal(gamesByYear, 'defense', 'tflyds'),
            sackua: this.calTotal(gamesByYear, 'defense', 'sackua'),
            sackyds: this.calTotal(gamesByYear, 'defense', 'sackyds'),
            ff: this.calTotal(gamesByYear, 'defense', 'ff'),
            fr: this.calTotal(gamesByYear, 'defense', 'fr'),
            fryds: this.calTotal(gamesByYear, 'defense', 'fryds'),
            qbh: this.calTotal(gamesByYear, 'defense', 'qbh'),
            brup: this.calTotal(gamesByYear, 'defense', 'brup')
          },
          ir: {
            no: this.calTotal(gamesByYear, 'ir', 'no'),
            yds: this.calTotal(gamesByYear, 'ir', 'yds'),
            td: this.calTotal(gamesByYear, 'ir', 'td')
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
          defense: {
            tackua: this.calTotal(this.totalsByYear, 'defense', 'tackua'),
            tacka: this.calTotal(this.totalsByYear, 'defense', 'tacka'),
            tot_tack: this.calTotal(this.totalsByYear, 'defense', 'tot_tack'),
            tfla: this.calTotal(this.totalsByYear, 'defense', 'tfla'),
            tflua: this.calTotal(this.totalsByYear, 'defense', 'tflua'),
            tflyds: this.calTotal(this.totalsByYear, 'defense', 'tflyds'),
            sackua: this.calTotal(this.totalsByYear, 'defense', 'sackua'),
            sackyds: this.calTotal(this.totalsByYear, 'defense', 'sackyds'),
            ff: this.calTotal(this.totalsByYear, 'defense', 'ff'),
            fr: this.calTotal(this.totalsByYear, 'defense', 'fr'),
            fryds: this.calTotal(this.totalsByYear, 'defense', 'fryds'),
            qbh: this.calTotal(this.totalsByYear, 'defense', 'qbh'),
            brup: this.calTotal(this.totalsByYear, 'defense', 'brup')
          },
          ir: {
            no: this.calTotal(this.totalsByYear, 'ir', 'no'),
            yds: this.calTotal(this.totalsByYear, 'ir', 'yds'),
            td: this.calTotal(this.totalsByYear, 'ir', 'td')
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
