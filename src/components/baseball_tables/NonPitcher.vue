<template>
 <h5>NonPitcher</h5>
  <div class="career-and-season">
    <h4>Career & Season Stats</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>GS</th>
            <th>AB</th>
            <th>R</th>
            <th>H</th>
            <th>2B</th>
            <th>3B</th>
            <th>HR</th>
            <th>RBI</th>
            <th>BB</th>
            <th>SO</th>
            <th>SB</th>
            <th>CS</th>
            <th>SF</th>
            <th>SH</th>
            <th>HBP</th>
            <th>AVG</th>
            <th>SLG</th>
            <th>OBP</th>
            <th>PO</th>
            <th>A</th>
            <th>E</th>
            <th>FLD</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.gs }}</td>
            <td>{{ totals.hitting.ab }}</td>
            <td>{{ totals.hitting.r }}</td>
            <td>{{ totals.hitting.h }}</td>
            <td>{{ totals.hitting.double }}</td>
            <td>{{ totals.hitting.triple }}</td>
            <td>{{ totals.hitting.hr }}</td>
            <td>{{ totals.hitting.rbi }}</td>
            <td>{{ totals.hitting.bb }}</td>
            <td>{{ totals.hitting.so }}</td>
            <td>{{ totals.hitting.sb }}</td>
            <td>{{ totals.hitting.cs }}</td>
            <td>{{ totals.hitting.sf }}</td>
            <td>{{ totals.hitting.sh }}</td>
            <td>{{ totals.hitting.hbp }}</td>
            <td>{{ totals.hitting.avg }}</td>
            <td>{{ totals.hitting.slg }}</td>
            <td>{{ (((totals.hitting.h - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0)) / ((totals.hitting.ab - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0) + (totals.hitting.sf - 0))).toFixed(2) }}</td>
            <td>{{ totals.fielding.po }}</td>
            <td>{{ totals.fielding.a }}</td>
            <td>{{ totals.fielding.e }}</td>
            <td>{{ (((totals.fielding.po - 0) + (totals.fielding.a - 0)) / ((totals.fielding.po - 0) + (totals.fielding.a - 0) + (totals.fielding.e - 0))).toFixed(3) }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <!-- <th>{{ careerTotals.gp }}</th>
            <th>{{ careerTotals.gs }}</th>
            <th v-if="careerTotals.result && careerTotals.result.win">{{ careerTotals.result.win }}</th><th v-else>0</th>
            <th v-if="careerTotals.result && careerTotals.result.lose">{{ careerTotals.result.lose }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.cg">{{ careerTotals.pitching.cg }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.sho">{{ careerTotals.pitching.sho }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.save">{{ careerTotals.pitching.save }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.ip">{{ careerTotals.pitching.ip }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.h">{{ careerTotals.pitching.h }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.r">{{ careerTotals.pitching.r }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.er">{{ careerTotals.pitching.er }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.bb">{{ careerTotals.pitching.bb }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.so">{{ careerTotals.pitching.so }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.double">{{ careerTotals.pitching.double }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.triple">{{ careerTotals.pitching.triple }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.hr">{{ careerTotals.pitching.hr }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.ab">{{ careerTotals.pitching.ab }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.hbp">{{ careerTotals.pitching.hbp }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.h && careerTotals.pitching.ab">{{ (careerTotals.pitching.h / careerTotals.pitching.ab).toFixed(3) }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.er && careerTotals.pitching.innings_pitched">{{ (careerTotals.pitching.er / careerTotals.pitching.innings_pitched * 9).toFixed(2) }}</th><th v-else>0</th> -->
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th>{{ year }} Game - Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>AB</th>
              <th>R</th>
              <th>H</th>
              <th>2B</th>
              <th>3B</th>
              <th>HR</th>
              <th>RBI</th>
              <th>BB</th>
              <th>SO</th>
              <th>SB</th>
              <th>CS</th>
              <th>SF</th>
              <th>SH</th>
              <th>HBP</th>
              <th>AVG</th>
              <th>SLG</th>
              <th>OBP</th>
              <th>PO</th>
              <th>A</th>
              <th>E</th>
              <th>FLD</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium != 'Larry H.Miller Field' && game.venue.stadium != 'Larry H Miller Field' && game.venue.neutralgame != 'Y'">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td v-if="game.hitting && game.hitting.ab" nowrap>{{ game.hitting.ab }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.r" nowrap>{{ game.hitting.r }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.h" nowrap>{{ game.hitting.h }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.double" nowrap>{{ game.hitting.double }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.triple" nowrap>{{ game.hitting.triple }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.hr" nowrap>{{ game.hitting.hr }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.rbi" nowrap>{{ game.hitting.rbi }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.bb" nowrap>{{ game.hitting.bb }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.so" nowrap>{{ game.hitting.so }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.sb" nowrap>{{ game.hitting.sb }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.cs" nowrap>{{ game.hitting.cs }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.sf" nowrap>{{ game.hitting.sf }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.sh" nowrap>{{ game.hitting.sh }}</td><td v-else>-</td>
              <td v-if="game.hitting && game.hitting.hbp" nowrap>{{ game.hitting.hbp }}</td><td v-else>-</td>
              <!-- AVG -->
              <td v-if="game.hitting && game.hitting.h && game.hitting.ab && game.hitting.ab !=='0'" nowrap>{{ (game.hitting.h / game.hitting.ab).toFixed(2) }}</td>
              <td v-else-if="game.hitting && game.hitting.h && game.hitting.ab">0.00</td>
              <td v-else>-</td>
              <!-- SLG -->
              <td v-if="game.hitting && game.hitting.h && game.hitting.ab && game.hitting.double && game.hitting.triple && game.hitting.hr && game.hitting.ab !=='0'">
                {{ (((game.hitting.hr - 0) + (game.hitting.double - 0) * 2 + (game.hitting.triple - 0) * 3 + (game.hitting.hr - 0) * 4) / game.hitting.ab).toFixed(2) }}
              </td>
              <td v-else-if="game.hitting && game.hitting.h && game.hitting.ab && game.hitting.double && game.hitting.triple && game.hitting.hr">0.00</td>
              <td v-else>-</td>
              <!-- OBP -->
              <td v-if="game.hitting && game.hitting.h && game.hitting.ab && game.hitting.bb && game.hitting.hbp && game.hitting.sf && ((game.hitting.ab - 0) + (game.hitting.bb - 0) + (game.hitting.hbp - 0) + (game.hitting.sf - 0)) !== 0">
                {{ (((game.hitting.h - 0) + (game.hitting.bb - 0) + (game.hitting.hbp - 0)) / ((game.hitting.ab - 0) + (game.hitting.bb - 0) + (game.hitting.hbp - 0) + (game.hitting.sf - 0))).toFixed(2) }}
              </td>
              <td v-else-if="game.hitting && game.hitting.h && game.hitting.ab && game.hitting.bb && game.hitting.hbp && game.hitting.sf">0.00</td>
              <td v-else>-</td>
              <td v-if="game.fielding && game.fielding.po" nowrap>{{ game.fielding.po }}</td><td v-else>-</td>
              <td v-if="game.fielding && game.fielding.a" nowrap>{{ game.fielding.a }}</td><td v-else>-</td>
              <td v-if="game.fielding && game.fielding.e" nowrap>{{ game.fielding.e }}</td><td v-else>-</td>
              <!-- FLD% -->
              <td v-if="game.fielding && game.fielding.po && game.fielding.a && game.fielding.e && ((game.fielding.po - 0) + (game.fielding.a - 0) + (game.fielding.e - 0)) !== 0">
                {{ (((game.fielding.po - 0) + (game.fielding.a - 0)) / ((game.fielding.po - 0) + (game.fielding.a - 0) + (game.fielding.e - 0))).toFixed(3) }}
              </td>
              <td v-else-if="game.fielding && game.fielding.po && game.fielding.a && game.fielding.e">0.000</td>
              <td v-else>-</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.hitting.ab }}</th>
              <th>{{ totals.hitting.r }}</th>
              <th>{{ totals.hitting.h }}</th>
              <th>{{ totals.hitting.double }}</th>
              <th>{{ totals.hitting.triple }}</th>
              <th>{{ totals.hitting.hr }}</th>
              <th>{{ totals.hitting.rbi }}</th>
              <th>{{ totals.hitting.bb }}</th>
              <th>{{ totals.hitting.so }}</th>
              <th>{{ totals.hitting.sb }}</th>
              <th>{{ totals.hitting.cs }}</th>
              <th>{{ totals.hitting.sf }}</th>
              <th>{{ totals.hitting.sh }}</th>
              <th>{{ totals.hitting.hbp }}</th>
              <th>{{ totals.hitting.avg }}</th>
              <th>{{ totals.hitting.slg }}</th>
              <th>{{ (((totals.hitting.h - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0)) / ((totals.hitting.ab - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0) + (totals.hitting.sf - 0))).toFixed(2) }}</th>
              <th>{{ totals.fielding.po }}</th>
              <th>{{ totals.fielding.a }}</th>
              <th>{{ totals.fielding.e }}</th>
              <th>{{ (((totals.fielding.po - 0) + (totals.fielding.a - 0)) / ((totals.fielding.po - 0) + (totals.fielding.a - 0) + (totals.fielding.e - 0))).toFixed(3) }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Infielder',
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
      const games = this.gamesRecordPlayerInCleared.filter(game => game.schedule_year === year).sort(this.dateSort)
      for (var i = 0; i < games.length; i++) {
        // h, ab -> no double , triple, hr -> 0
        if (games[i].hitting && games[i].hitting.ab) {
          if (!games[i].hitting.double) games[i].hitting.double = '0'
          if (!games[i].hitting.triple) games[i].hitting.triple = '0'
          if (!games[i].hitting.hr) games[i].hitting.hr = '0'
          if (!games[i].hitting.bb) games[i].hitting.bb = '0'
          if (!games[i].hitting.hbp) games[i].hitting.hbp = '0'
          if (!games[i].hitting.sf) games[i].hitting.sf = '0'
        }
      }
      return games
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
          gs: this.getGS(gamesByYear),
          gp: this.getGP(gamesByYear),
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          hitting: {
            ab: this.calTotal(gamesByYear, 'hitting', 'ab'),
            h: this.calTotal(gamesByYear, 'hitting', 'h'),
            r: this.calTotal(gamesByYear, 'hitting', 'r'),
            double: this.calTotal(gamesByYear, 'hitting', 'double'),
            triple: this.calTotal(gamesByYear, 'hitting', 'triple'),
            hr: this.calTotal(gamesByYear, 'hitting', 'hr'),
            bb: this.calTotal(gamesByYear, 'hitting', 'bb'),
            so: this.calTotal(gamesByYear, 'hitting', 'so'),
            rbi: this.calTotal(gamesByYear, 'hitting', 'rbi'),
            sb: this.calTotal(gamesByYear, 'hitting', 'sb'),
            hbp: this.calTotal(gamesByYear, 'hitting', 'hbp'),
            cs: this.calTotal(gamesByYear, 'hitting', 'cs'),
            sf: this.calTotal(gamesByYear, 'hitting', 'sf'),
            sh: this.calTotal(gamesByYear, 'hitting', 'sh'),
            avg: this.getAVG(this.calTotal(gamesByYear, 'hitting', 'h'), this.calTotal(gamesByYear, 'hitting', 'ab')),
            slg: this.getSLG(
              this.calTotal(gamesByYear, 'hitting', 'h'),
              this.calTotal(gamesByYear, 'hitting', 'double'),
              this.calTotal(gamesByYear, 'hitting', 'triple'),
              this.calTotal(gamesByYear, 'hitting', 'hr'),
              this.calTotal(gamesByYear, 'hitting', 'ab'))
          },
          fielding: {
            po: this.calTotal(gamesByYear, 'fielding', 'po'),
            a: this.calTotal(gamesByYear, 'fielding', 'a'),
            e: this.calTotal(gamesByYear, 'fielding', 'e')
          }
        }
        console.log('gamesTotalByYear', gamesTotalByYear)
        this.totalsByYear.push(gamesTotalByYear)
      }
      this.calCareerTotals()
    },
    calCareerTotals () {
      if (this.totalsByYear) {
        this.careerTotals = {
          gs: this.getGS(this.totalsByYear),
          gp: this.getGP(this.totalsByYear),
          result: {
            win: this.getWinGame(this.totalsByYear),
            lose: this.getLoseGame(this.totalsByYear)
          },
          hitting: {
            ab: this.calTotal(this.totalsByYear, 'hitting', 'ab'),
            h: this.calTotal(this.totalsByYear, 'hitting', 'h'),
            r: this.calTotal(this.totalsByYear, 'hitting', 'r'),
            double: this.calTotal(this.totalsByYear, 'hitting', 'double'),
            triple: this.calTotal(this.totalsByYear, 'hitting', 'triple'),
            hr: this.calTotal(this.totalsByYear, 'hitting', 'hr'),
            bb: this.calTotal(this.totalsByYear, 'hitting', 'bb'),
            so: this.calTotal(this.totalsByYear, 'hitting', 'so'),
            rbi: this.calTotal(this.totalsByYear, 'hitting', 'rbi'),
            sb: this.calTotal(this.totalsByYear, 'hitting', 'sb'),
            hbp: this.calTotal(this.totalsByYear, 'hitting', 'hbp'),
            cs: this.calTotal(this.totalsByYear, 'hitting', 'cs'),
            sf: this.calTotal(this.totalsByYear, 'hitting', 'sf'),
            sh: this.calTotal(this.totalsByYear, 'hitting', 'sh'),
            avg: this.getAVG(this.calTotal(this.totalsByYear, 'hitting', 'h'), this.calTotal(this.totalsByYear, 'hitting', 'ab')),
            slg: this.getSLG(
              this.calTotal(this.totalsByYear, 'hitting', 'h'),
              this.calTotal(this.totalsByYear, 'hitting', 'double'),
              this.calTotal(this.totalsByYear, 'hitting', 'triple'),
              this.calTotal(this.totalsByYear, 'hitting', 'hr'),
              this.calTotal(this.totalsByYear, 'hitting', 'ab'))
          },
          fielding: {
            po: this.calTotal(this.totalsByYear, 'fielding', 'po'),
            a: this.calTotal(this.totalsByYear, 'fielding', 'a'),
            e: this.calTotal(this.totalsByYear, 'fielding', 'e')
          }
        }
      }
    },
    getGS (games) {
      let total = 0
      for (let i = 0; i < games.length; i++) {
        if (games[i] && games[i].gs) {
          total += games[i].gp - 0
        }
      }
      return total
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
    getTotalsByYear (year) {
      return this.totalsByYear.filter(total => total.schedule_year === year)
    },
    getAVG (numer, denom) {
      if (denom === 0) return 0
      const avg = (numer / denom).toFixed(2)
      return avg
    },
    getSLG (h, double, triple, hr, ab) {
      if (ab === 0) return 0
      const slg = ((h + double * 2 + triple * 3 + hr * 4) / ab).toFixed(2)
      return slg
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
