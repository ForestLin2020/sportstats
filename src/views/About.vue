<template>
  <div>
    <button class="btn btn-primary" @click="submit">Game</button>
    <h3 class="color-block">Career & Season Stats</h3>
    <div class="table-responsive" ng-if="ap.field_positions.indexOf('Quarterback') != -1">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead class='football-thead' ng-if="ap.field_positions.indexOf('Quarterback') != -1">
          <tr>
            <th colspan="2">Career
              <span ng-if="ap.field_positions.includes(',')"> - Offense</span>
            </th>
            <th colspan="9">Passing</th>
            <th colspan="5">Rushing</th>
            <th colspan="2">Sacked</th>
            <th colspan="2">Fumbles</th>
          </tr>
        </thead>
        <tbody ng-if="ap.field_positions.indexOf('Quarterback') != -1">
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
          <!-- <tr ng-repeat="S in games.totals_array track by $index">
            <td>{{S.sch}}</td>
            <td>{{S.gp }}</td>
            <td>{{S.pass.comp}}</td>
            <td>{{S.pass.att}}</td>
            <td>{{S.pass.yds}}</td>
            <td>{{(S.pass.yds / S.pass.att)}}</td>
            <td>{{(S.pass.comp / S.pass.att)}}</td>
            <td>{{S}}</td>
            <td>{{S.pass.int}}</td>
            <td>{{S.pass.td}}</td>
            <td>{{((8.4*S.pass.yds)+(330*S.pass.td)+(100*S.pass.comp)-(200*S.pass.int))/S.pass.att
               }}</td>
            <td>{{S.rush.att}}</td>
            <td>{{S.rush.yds}}</td>
            <td>{{S.rush.yds/S.rush.at}}</td>
            <td>{{S}}</td>
            <td>{{S.rush.td}}</td>
            <td>{{S.pass.sacks}}</td>
            <td>{{S.pass.sackyds}}</td>
            <td>{{S.fumbles.no }}</td>
            <td>{{S.fumbles.lost }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{games.career_totals.gp }}</th>
            <th>{{games.career_totals.pass.comp}}</th>
            <th>{{games.career_totals.pass.att}}</th>
            <th>{{games.career_totals.pass.yds}}</th>
            <th>{{(games.career_totals.pass.yds /
                games.career_totals.pass.att)}}</th>
            <th>{{(games.career_totals.pass.comp /
                games.career_totals.pass.att)}}</th>
            <th>{{games.career_totals}}</th>
            <th>{{games.career_totals.pass.int}}</th>
            <th>{{games.career_totals.pass.td}}</th>
            <th>{{((8.4*games.career_totals.pass.yds)+(330*games.career_totals.pass.td)+(100*games.career_totals.pass.comp)-(200*games.career_totals.pass.int))/games.career_totals.pass.att
               }}</th>
            <th>{{games.career_totals.rush.att}}</th>
            <th>{{games.career_totals.rush.yds}}</th>
            <th>{{games.career_totals.rush.yds/games.career_totals.rush.att
            }}</th>
            <th>{{games.career_totals}}</th>
            <th>{{games.career_totals.rush.td}}</th>
            <th>{{games.career_totals.pass.sacks}}</th>
            <th>{{games.career_totals.pass.sackyds}}</th>
            <th>{{games.career_totals.fumbles.no }}</th>
            <th>{{games.career_totals.fumbles.lost }}</th>
          </tr> -->
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead class='football-thead'>
            <tr>
              <!-- <th colspan="3">{{games[0]}} Games
                <span ng-if="ap.field_positions.includes(',')"> - Offense</span>
              </th> -->
              <th colspan="3">{{ year }} Games</th>
              <th colspan="9">Passing</th>
              <th colspan="5">Rushing</th>
              <th colspan="2">Sacked</th>
              <th colspan="2">Fumbles</th>
            </tr>
          </thead>
          <tbody ng-if="ap.field_positions.indexOf('Quarterback') != -1">
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
              <td nowrap style="color: green" v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b>W</b> {{game.byu_score}}-{{game.opp_score}} </td>
              <td nowrap style="color: blue" v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b>T</b> {{game.byu_score}}-{{game.opp_score}} </td>
              <td nowrap style="color: red" v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b>L</b> {{game.byu_score}}-{{game.opp_score}} </td>
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
              <th> <span style="color: green">{{ totals.result.win }}</span>-<span style="color: red">{{ totals.result.lose }}</span></th>
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
  props: [],
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
      gameYears: [],
      totalsByYear: []
    }
  },
  computed: {
    // do something
  },
  async mounted () {
    // ======= fetch from URL API =======
    // const athletesUrl = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/athlete/sportNid/athleteNid'
    const athletesUrl = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/athlete/1701/1285416'
    const res = await fetch(athletesUrl)
    const data = await res.json()

    for (var i = 0; i < data.length; i++) {
      const byuTeamIndex = data[i].fbgame.team.findIndex(x => x.name === 'BYU') // find index in array
      const oppoTeamIndex = data[i].fbgame.team.findIndex(x => x.name !== 'BYU') // find index in array
      const byuTeamStat = data[i].fbgame.team[byuTeamIndex]
      const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.playerNid)
      const playerStat = byuTeamStat.player[playerIndex]

      // Insert game's info into player's info -> Makes template much easier to render data
      // -> Makes data into same layer if they are stored in different place
      // -> response.data[i].fbgame.team[0].player[p].nid = response.data[i].nid
      playerStat.nid = data[i].nid
      playerStat.title = data[i].title
      playerStat.schedule_year = data[i].schedule_year
      playerStat.event_date = data[i].event_date
      playerStat.scores = data[i].fbgame.scores
      playerStat.venue = data[i].fbgame.venue
      playerStat.linescore = data[i].fbgame.team[byuTeamIndex].linescore
      playerStat.opp_score = data[i].fbgame.team[oppoTeamIndex].linescore.score
      playerStat.byu_score = data[i].fbgame.team[byuTeamIndex].linescore.score
      this.games.push(playerStat)

      // store different years for grouping stats table
      if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
        this.gameYears.push(playerStat.schedule_year)
      }
    }
    this.getTotals(this.games, this.gameYears)
  },
  methods: {
    submit () {
      console.log('Hi, Forest')
    },
    formDateStr (str) {
      var date = new Date(str)
      var dateStr = this.months[date.getMonth()] + ' ' + date.getDate()
      return dateStr
    },
    gamesFilterEventsByYear (year) {
      return this.games.filter(game => game.schedule_year === year)
    },
    getTotals (games, years) {
      for (let i = 0; i < years.length; i++) {
        const gamesByYear = this.gamesFilterEventsByYear(years[i])
        const gameTotalByYear = {
          schedule_year: years[i],
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          pass: {
            att: this.getTotalByYear(gamesByYear, 'pass', 'att'),
            comp: this.getTotalByYear(gamesByYear, 'pass', 'comp'),
            int: this.getTotalByYear(gamesByYear, 'pass', 'int'),
            long: this.getBiggestLongByYear(gamesByYear, 'pass', 'long'),
            sacks: this.getTotalByYear(gamesByYear, 'pass', 'sacks'),
            sackyds: this.getTotalByYear(gamesByYear, 'pass', 'sackyds'),
            td: this.getTotalByYear(gamesByYear, 'pass', 'td'),
            yds: this.getTotalByYear(gamesByYear, 'pass', 'yds'),
            ya: this.get_YA(this.getTotalByYear(gamesByYear, 'pass', 'yds'), this.getTotalByYear(gamesByYear, 'pass', 'att')),
            pct: this.get_PCT(this.getTotalByYear(gamesByYear, 'pass', 'comp'), this.getTotalByYear(gamesByYear, 'pass', 'att')),
            effic: this.get_EFFIC(
              this.getTotalByYear(gamesByYear, 'pass', 'yds'),
              this.getTotalByYear(gamesByYear, 'pass', 'td'),
              this.getTotalByYear(gamesByYear, 'pass', 'comp'),
              this.getTotalByYear(gamesByYear, 'pass', 'int'),
              this.getTotalByYear(gamesByYear, 'pass', 'att')
            )
          },
          rush: {
            att: this.getTotalByYear(gamesByYear, 'rush', 'att'),
            long: this.getBiggestLongByYear(gamesByYear, 'rush', 'long'),
            td: this.getTotalByYear(gamesByYear, 'rush', 'td'),
            yds: this.getTotalByYear(gamesByYear, 'rush', 'yds'),
            ya: this.get_YA(this.getTotalByYear(gamesByYear, 'rush', 'yds'), this.getTotalByYear(gamesByYear, 'rush', 'att'))
          },
          fumbles: {
            lost: this.getTotalByYear(gamesByYear, 'fumbles', 'lost'),
            no: this.getTotalByYear(gamesByYear, 'fumbles', 'no')
          }
        }
        this.totalsByYear.push(gameTotalByYear)
      }
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
    getTotalByYear (games, categoryKey, statsKey) {
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
