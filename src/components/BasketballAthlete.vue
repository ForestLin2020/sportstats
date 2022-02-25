<template>
 <h5>Basketball</h5>
  <div class="career-and-season">
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
        <tr>
            <th colspan="5">Career</th>
            <th colspan="8">Scoring</th>
            <th colspan="10">Misc</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>GS</th>
            <th>MIN</th>
            <th>AVG</th>
            <th>PTS</th>
            <th>AVG</th>
            <th>FG-FGA</th>
            <th>FG%</th>
            <th>3P-3PA</th>
            <th>3P%</th>
            <th>FT-FTA</th>
            <th>FT%</th>
            <th>OFF</th>
            <th>DEF</th>
            <th>REB</th>
            <th>AVG</th>
            <th>AST</th>
            <th>AVG</th>
            <th>BLK</th>
            <th>STL</th>
            <th>PF</th>
          </tr>
          <!-- <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.gs }}</td>
            <td>{{ totals.min }}</td>
            <td>{{ totals.avg }}</td>
            <td>{{ totals.scoring.pts }}</td>
            <td>{{ totals.scoring.avg }}</td>
            <td>{{ totals.scoring.fg-fga }}</td>
            <td>{{ totals.scoring.fgpercent }}</td>
            <td>{{ totals.scoring.threep-threepa}}</td>
            <td>{{ totals.scoring.threepercent }}</td>
            <td>{{ totals.scoring.ft-fta }}</td>
            <td>{{ totals.scoring.ftpercent }}</td>
            <td>{{ totals.misc.off }}</td>
            <td>{{ totals.misc.def }}</td>
            <td>{{ totals.misc.reb }}</td>
            <td>{{ totals.misc.avg }}</td>
            <td>{{ totals.misc.ast }}</td>
            <td>{{ totals.misc.avg }}</td>
            <td>{{ totals.misc.blk }}</td>
            <td>{{ totals.misc.stl }}</td>
            <td>{{ totals.misc.pf }}</td>
          </tr> -->
          <!-- <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
            <th>{{ careerTotals.gs }}</th>
            <th>{{ careerTotals.min }}</th>
            <th>{{ careerTotals.avg }}</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.pts }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.fg-fga }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.fgpercent }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.threep-threepa }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.threepercent }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.ft-fta }}</th><th v-else>0</th>
            <th v-if="careerTotals.scoring" nowrap>{{ careerTotals.scoring.ftpercent }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.off }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.def }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.reb }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.ast }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.blk }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.stl }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc" nowrap>{{ careerTotals.misc.pf }}</th><th v-else>0</th>
          </tr> -->
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th colspan="4">{{ year }} Games</th>
              <th colspan="7">Scoring</th>
              <th colspan="8">Misc</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>MIN</th>
              <th>PTS</th>
              <th>FG-FGA</th>
              <th>FG%</th>
              <th>3P-3PA</th>
              <th>3P%</th>
              <th>FT-FTA</th>
              <th>FT%</th>
              <th>OFF</th>
              <th>DEF</th>
              <th>REB</th>
              <th>AST</th>
              <th>BLK</th>
              <th>STL</th>
              <th>PF</th>
              <th>TO</th>
            </tr>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <!-- <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
              </td> -->
              <!-- <td nowrap>{{ formDateStr(game.event_date) }}</td> -->
              <!-- <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td> -->
              <!-- <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td> -->
              <!-- <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td> -->
              <!-- <td v-if="game.min && game.min" nowrap>{{ game.min }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.pts" nowrap>{{ game.misc.pts }}</td><td v-else>0</td> -->
                <!-- fg / fg att -->
              <!-- <td v-if="game.scoring && game.scoring.fg && game.scoring.fga" nowrap>{{ (game.scoring.fg / game.scoring.fga).toFixed(2) }}</td><td v-else>0</td> -->
                <!-- PCT: fg / fg att -->
              <!-- <td v-if="game.scoring && game.scoring.fg && game.pass.fga" nowrap>{{ (game.pass.fg / game.pass.fga * 100).toFixed(2) }}%</td><td v-else>0</td> -->
               <!-- 3p / 3p att -->
              <!-- <td v-if="game.scoring && game.scoring.threep && game.scoring.threepa" nowrap>{{ (game.scoring.threep / game.scoringthreepa).toFixed(2) }}</td><td v-else>0</td> -->
                <!-- PCT: 3p / 3p att -->
              <!-- <td v-if="game.scoring && game.scoring.threep && game.pass.threepa" nowrap>{{ (game.pass.threep / game.pass.threepa).toFixed(2) }}</td><td v-else>0</td> -->
               <!-- ft / ft att -->
              <!-- <td v-if="game.scoring && game.scoring.ft && game.scoring.fta" nowrap>{{ (game.scoring.ft / game.scoring.fta).toFixed(2) }}</td><td v-else>0</td> -->
                <!-- PCT: ft / ft att -->
              <!-- <td v-if="game.scoring && game.scoring.ft && game.pass.fta" nowrap>{{ (game.pass.ft / game.pass.fta).toFixed(2) }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.off" nowrap>{{ game.misc.off }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.def" nowrap>{{ game.misc.def }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.reb" nowrap>{{ game.misc.reb }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.ast" nowrap>{{ game.misc.ast }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.blk" nowrap>{{ game.misc.blk }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.stl" nowrap>{{ game.misc.stl }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.pf" nowrap>{{ game.misc.pf }}</td><td v-else>0</td> -->
              <!-- <td v-if="game.misc && game.misc.to" nowrap>{{ game.misc.to }}</td><td v-else>0</td> -->
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Basketball',
  props: ['selected', 'gamesRecordPlayerIn'],
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
      careerTotals: [],
      gamesRecordPlayerInCleared: [],
      gameYears: []
    }
  },
  computed: {
    // you can do some thing here
  },
  mounted () {
    // you can do some thing here
  },
  methods: {
    reorganizeGames () {
      // clear the old data
      if (this.gamesRecordPlayerInCleared) {
        this.gamesRecordPlayerInCleared = []
        this.gameYears = []
      }

      const data = this.gamesRecordPlayerIn
      for (var i = 0; i < data.length; i++) {
        // x.id === 'BYU' or x.name === 'BYU' or x.name === 'BY'
        const byuTeamIndex = data[i].bbgame.team.findIndex(x => (x.id === 'BYU' || x.name === 'BYU')) // find index in array
        const oppoTeamIndex = data[i].bbgame.team.findIndex(x => x.id !== 'BYU') // find index in array
        const byuTeamStat = data[i].bbgame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].bbgame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].bbgame.scores
        playerStat.venue = data[i].bbgame.venue
        playerStat.linescore = data[i].bbgame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].bbgame.team[oppoTeamIndex].linescore.score
        playerStat.byu_score = data[i].bbgame.team[byuTeamIndex].linescore.score
        this.gamesRecordPlayerInCleared.push(playerStat)

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()
      this.gameYears.reverse()
      this.calTotalsByYear(this.gameYears)

      console.log('selected', this.selected)
      console.log('gamesRecordPlayerInCleared', this.gamesRecordPlayerInCleared)
      console.log('gameYears', this.gameYears)
    },
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
          scoring: {
            pts: this.calTotal(gamesByYear, 'scoring', 'pts'),
            ptsavg: this.calTotal(gamesByYear, 'scoring', 'ptsavg'),
            fgfga: this.calTotal(gamesByYear, 'scoring', 'fgfga'),
            fgpercent: this.calTotal(gamesByYear, 'scoring', 'fgpercent'),
            threemadevsa: this.calTotal(gamesByYear, 'scoring', 'threemadevsa'),
            threepercent: this.calTotal(gamesByYear, 'scoring', 'threepercent'),
            ftmadevsatt: this.calTotal(gamesByYear, 'scoring', 'ftmadevsatt'),
            ftpercent: this.calTotal(gamesByYear, 'scoring', 'ftpercent')
          },
          misc: {
            minutes: this.calTotal(gamesByYear, 'misc', 'minutes'),
            minavg: this.calTotal(gamesByYear, 'misc', 'minavg'),
            off: this.calTotal(gamesByYear, 'misc', 'off'),
            def: this.calTotal(gamesByYear, 'misc', 'def'),
            reb: this.calTotal(gamesByYear, 'misc', 'reb'),
            rebavg: this.calTotal(gamesByYear, 'misc', 'rebavg'),
            ast: this.calTotal(gamesByYear, 'misc', 'ast'),
            astavg: this.calTotal(gamesByYear, 'misc', 'avg'),
            blk: this.calTotal(gamesByYear, 'misc', 'blk'),
            stl: this.calTotal(gamesByYear, 'misc', 'stl'),
            pf: this.calTotal(gamesByYear, 'misc', 'pf'),
            to: this.calTotal(gamesByYear, 'misc', 'to')
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
          scoring: {
            pts: this.calTotal(this.totalsByYear, 'scoring', 'pts'),
            ptsavg: this.calTotal(this.totalsByYear, 'scoring', 'avg'),
            fgfga: this.calTotal(this.totalsByYear, 'scoring', 'fgfga'),
            fgpercent: this.calTotal(this.totalsByYear, 'scoring', 'fgpercent'),
            threemadevsa: this.calTotal(this.totalsByYear, 'scoring', 'threemadevsa'),
            threepercent: this.calTotal(this.totalsByYear, 'scoring', 'threepercent'),
            ftmadevsatt: this.calTotal(this.totalsByYear, 'scoring', 'ftmadevsatt'),
            ftpercent: this.calTotal(this.totalsByYear, 'scoring', 'ftpercent')
          },
          misc: {
            minutes: this.calTotal(this.totalsByYear, 'misc', 'minutes'),
            minavg: this.calTotal(this.totalsByYear, 'misc', 'avg'),
            off: this.calTotal(this.totalsByYear, 'misc', 'off'),
            def: this.calTotal(this.totalsByYear, 'misc', 'def'),
            reb: this.calTotal(this.totalsByYear, 'misc', 'reb'),
            rebavg: this.calTotal(this.totalsByYear, 'misc', 'avg'),
            ast: this.calTotal(this.totalsByYear, 'misc', 'ast'),
            astavg: this.calTotal(this.totalsByYear, 'misc', 'avg'),
            blk: this.calTotal(this.totalsByYear, 'misc', 'blk'),
            stl: this.calTotal(this.totalsByYear, 'misc', 'stl'),
            pf: this.calTotal(this.totalsByYear, 'misc', 'pf'),
            to: this.calTotal(this.totalsByYear, 'misc', 'to')
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
