<template>
 <h5>Volleyball</h5>
  <div class="career-and-season">
    <div class="table-responsive" v-if="totalsByYear.length != 0">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th>Year</th>
            <th>MS</th>
            <th>S</th>
            <th>K</th>
            <th>E</th>
            <th>TA</th>
            <th>Pct</th>
            <th>Ast</th>
            <th>SA</th>
            <th>SE</th>
            <th>RE</th>
            <th>Dig</th>
            <th>BS</th>
            <th>BA</th>
            <th>BE</th>
            <th>BHE</th>
            <th>Pts</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>MS</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.attack.k }}</td>
            <td>{{ totals.attack.e }}</td>
            <td>{{ totals.attack.ta }}</td>
            <td>{{ totals.attack.pct }}</td>
            <td>{{ totals.set.a }}</td>
            <td>{{ totals.serve.sa }}</td>
            <td>{{ totals.serve.se }}</td>
            <td>{{ totals.defense.re }}</td>
            <td>{{ totals.defense.dig }}</td>
            <td>{{ totals.block.bs }}</td>
            <td>{{ totals.block.ba }}</td>
            <td>{{ totals.block.be }}</td>
            <td>{{ totals.misc.bhe }}</td>
            <td>{{ totals.misc.pts }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>MS</th>
            <th>{{ careerTotals.gp }}</th>
            <th v-if="careerTotals.attack && careerTotals.attack.k" nowrap>{{ careerTotals.attack.k }}</th><th v-else>0</th>
            <th v-if="careerTotals.attack && careerTotals.attack.e" nowrap>{{ careerTotals.attack.e }}</th><th v-else>0</th>
            <th v-if="careerTotals.attack && careerTotals.attack.ta" nowrap>{{ careerTotals.attack.ta }}</th><th v-else>0</th>
            <th v-if="careerTotals.attack && careerTotals.attack.pct" nowrap>{{ careerTotals.attack.pct }}</th><th v-else>0</th>
            <th v-if="careerTotals.set && careerTotals.set.a" nowrap>{{ careerTotals.set.a }}</th><th v-else>0</th>
            <th v-if="careerTotals.serve && careerTotals.serve.sa" nowrap>{{ careerTotals.serve.sa }}</th><th v-else>0</th>
            <th v-if="careerTotals.serve && careerTotals.serve.se" nowrap>{{ careerTotals.serve.se }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.re" nowrap>{{ careerTotals.defense.re }}</th><th v-else>0</th>
            <th v-if="careerTotals.defense && careerTotals.defense.dig" nowrap>{{ careerTotals.defense.dig }}</th><th v-else>0</th>
            <th v-if="careerTotals.block && careerTotals.block.bs" nowrap>{{ careerTotals.block.bs }}</th><th v-else>0</th>
            <th v-if="careerTotals.block && careerTotals.block.ba" nowrap>{{ careerTotals.block.ba }}</th><th v-else>0</th>
            <th v-if="careerTotals.block && careerTotals.block.be" nowrap>{{ careerTotals.block.be }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc && careerTotals.misc.bhe" nowrap>{{ careerTotals.misc.bhe }}</th><th v-else>0</th>
            <th v-if="careerTotals.misc && careerTotals.misc.pts" nowrap>{{ careerTotals.misc.pts }}</th><th v-else>0</th>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <h1> {{ year }} </h1>
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th >S</th>
              <th >K</th>
              <th >E</th>
              <th >TA</th>
              <th >Pct</th>
              <th >Ast</th>
              <th >SA</th>
              <th >SE</th>
              <th >RE</th>
              <th >Dig</th>
              <th >BS</th>
              <th >BA</th>
              <th >BE</th>
              <th >BHE</th>
              <th >Pts</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <!-- <td nowrap ng-if="!S.player_game_stats.byu_score">-</td> -->
              <td v-if="game.gp" nowrap>{{ game.gp }}</td><td v-else>0</td>
              <td v-if="game.attack && game.attack.k" nowrap>{{ game.attack.k }}</td><td v-else>0</td>
              <td v-if="game.attack && game.attack.e" nowrap>{{ game.attack.e }}</td><td v-else>0</td>
              <td v-if="game.attack && game.attack.ta" nowrap>{{ game.attack.ta }}</td><td v-else>0</td>
              <td v-if="game.attack && game.attack.k && game.attack.e && game.attack.ta && game.attack.ta != '0' " nowrap>{{ ((game.attack.k - game.attack.e)/game.attack.ta).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.set && game.set.a" nowrap>{{ game.set.a }}</td><td v-else>0</td>
              <td v-if="game.serve && game.serve.sa" nowrap>{{ game.serve.sa }}</td><td v-else>0</td>
              <td v-if="game.serve && game.serve.se" nowrap>{{ game.serve.se }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.re" nowrap>{{ game.defense.re }}</td><td v-else>0</td>
              <td v-if="game.defense && game.defense.dig" nowrap>{{ game.defense.dig }}</td><td v-else>0</td>
              <td v-if="game.block && game.block.bs" nowrap>{{ game.block.bs }}</td><td v-else>0</td>
              <td v-if="game.block && game.block.ba" nowrap>{{ game.block.ba }}</td><td v-else>0</td>
              <td v-if="game.block && game.block.be" nowrap>{{ game.block.be }}</td><td v-else>0</td>
              <td v-if="game.misc && game.misc.bhe" nowrap>{{ game.misc.bhe }}</td><td v-else>0</td>
              <td v-if="game.misc && game.misc.pts" nowrap>{{ game.misc.pts }}</td><td v-else>0</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.gp }}</th>
              <th>{{ totals.attack.k }}</th>
              <th>{{ totals.attack.e }}</th>
              <th>{{ totals.attack.ta }}</th>
              <th>{{ totals.attack.pct }}</th>
              <th>{{ totals.set.a }}</th>
              <th>{{ totals.serve.sa }}</th>
              <th>{{ totals.serve.se }}</th>
              <th>{{ totals.defense.re }}</th>
              <th>{{ totals.defense.dig }}</th>
              <th>{{ totals.block.bs }}</th>
              <th>{{ totals.block.ba }}</th>
              <th>{{ totals.block.be }}</th>
              <th>{{ totals.misc.bhe }}</th>
              <th>{{ totals.misc.pts }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VolleyballAthlete',
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
      playerNid: 1285416,
      games: [],
      totalsByYear: [],
      careerTotals: [],
      gamesRecordPlayerInCleared: [],
      gameYears: []
    }
  },
  mounted () {
    // console.log('selected', this.selected)
    // console.log('games', this.games)
    // console.log('gameYears', this.gameYears)
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
        const byuTeamIndex = data[i].vbgame.team.findIndex(x => (x.name === 'BYU' | x.name === 'BY' | x.id === 'BYU')) // find index in array
        const oppoTeamIndex = data[i].vbgame.team.findIndex(x => x.name !== 'BYU') // find index in array
        const byuTeamStat = data[i].vbgame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].vbgame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].vbgame.scores
        playerStat.venue = data[i].vbgame.venue
        playerStat.linescore = data[i].vbgame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].vbgame.team[oppoTeamIndex].linescore.score
        playerStat.byu_score = data[i].vbgame.team[byuTeamIndex].linescore.score
        this.gamesRecordPlayerInCleared.push(playerStat)

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()
      this.games = this.gamesRecordPlayerInCleared
      this.calTotalsByYear(this.gameYears)
      console.log('selected', this.selected)
      console.log('games', this.games)
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
      return this.games.filter(game => game.schedule_year === year)
    },
    calTotalsByYear (years) {
      // clear old data
      this.totalsByYear = []
      for (let i = 0; i < years.length; i++) {
        const gamesByYear = this.gamesFilterEventsByYear(years[i])
        console.log('year', years[i])
        console.log('gamesByYear', gamesByYear)
        const gamesTotalByYear = {
          schedule_year: years[i],
          gp: this.getGP(gamesByYear),
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          attack: {
            k: this.calTotal(gamesByYear, 'attack', 'k'),
            e: this.calTotal(gamesByYear, 'attack', 'e'),
            ta: this.calTotal(gamesByYear, 'attack', 'ta'),
            pct: this.get_PCT(this.calTotal(gamesByYear, 'attack', 'k'), this.calTotal(gamesByYear, 'attack', 'e'), this.calTotal(gamesByYear, 'attack', 'ta'))
          },
          set: {
            a: this.calTotal(gamesByYear, 'set', 'a')
          },
          serve: {
            sa: this.calTotal(gamesByYear, 'serve', 'sa'),
            se: this.calTotal(gamesByYear, 'serve', 'se')
          },
          defense: {
            re: this.calTotal(gamesByYear, 'defense', 're'),
            dig: this.calTotal(gamesByYear, 'defense', 'dig')
          },
          block: {
            bs: this.calTotal(gamesByYear, 'block', 'bs'),
            ba: this.calTotal(gamesByYear, 'block', 'ba'),
            be: this.calTotal(gamesByYear, 'block', 'be')
          },
          misc: {
            bhe: this.calTotal(gamesByYear, 'misc', 'bhe'),
            pts: this.calTotal(gamesByYear, 'misc', 'pts')
          }
        }
        this.totalsByYear.push(gamesTotalByYear)
      }
      console.log('totalsByYear', this.totalsByYear)
      this.calCareerTotals()
    },
    calCareerTotals () {
      if (this.totalsByYear) {
        this.careerTotals = {
          gp: this.getGP(this.totalsByYear),
          attack: {
            k: this.calTotal(this.totalsByYear, 'attack', 'k'),
            e: this.calTotal(this.totalsByYear, 'attack', 'e'),
            ta: this.calTotal(this.totalsByYear, 'attack', 'ta'),
            pct: this.get_PCT(this.calTotal(this.totalsByYear, 'attack', 'k'), this.calTotal(this.totalsByYear, 'attack', 'e'), this.calTotal(this.totalsByYear, 'attack', 'ta'))
          },
          set: {
            a: this.calTotal(this.totalsByYear, 'set', 'a')
          },
          serve: {
            sa: this.calTotal(this.totalsByYear, 'serve', 'sa'),
            se: this.calTotal(this.totalsByYear, 'serve', 'se')
          },
          defense: {
            re: this.calTotal(this.totalsByYear, 'defense', 're'),
            dig: this.calTotal(this.totalsByYear, 'defense', 'dig')
          },
          block: {
            bs: this.calTotal(this.totalsByYear, 'block', 'bs'),
            ba: this.calTotal(this.totalsByYear, 'block', 'ba'),
            be: this.calTotal(this.totalsByYear, 'block', 'be')
          },
          misc: {
            bhe: this.calTotal(this.totalsByYear, 'misc', 'bhe'),
            pts: this.calTotal(this.totalsByYear, 'misc', 'pts')
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
    get_PCT (k, e, ta) {
      if (ta === 0) return 0
      const pct = ((k - e) / ta).toFixed(2)
      return pct
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
