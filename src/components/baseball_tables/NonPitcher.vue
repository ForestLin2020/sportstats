<template>
 <h5>NonPitcher</h5>
  <div class="career-and-season">
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
          <tr>
            <!-- <td>{[{S.schedule_year}]}</td> -->
            <!-- <td>{[{S.gp | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.gs | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.ab | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.r | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.h | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.double | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.triple | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.hr | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.rbi | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.bb | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.so | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.sb | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.cs | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.sf | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.sh | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.hitting.hbp | ifEmpty:0}]}</td> -->
            <!-- AVG -->
            <!-- <td>{[{(S.hitting.h / S.hitting.ab) | number:3 | limitTo:4:1 | ifEmpty:0}]}</td> -->
            <!-- SLG -->
            <!-- <td nowrap>{[{((S.hitting.h - 0) - (S.hitting.double - 0) - (S.hitting.triple - 0) - (S.hitting.hr) + (S.hitting.double - 0)*2 + (S.hitting.triple - 0)*3 + (S.hitting.hr - 0)*4) / (S.hitting.ab) | number:3 | limitTo:4:1 | ifEmpty:0}]}</td> -->
            <!-- OBP -->
            <!-- <td>{[{((S.hitting.h *1) + (S.hitting.bb *1) + (S.hitting.hbp *1)) / ((S.hitting.ab *1) + (S.hitting.bb *1) + (S.hitting.hbp *1) + (S.hitting.sf *1)) | number:3 | limitTo:4:1 | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.fielding.po | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.fielding.a | ifEmpty:0}]}</td> -->
            <!-- <td>{[{S.fielding.e | ifEmpty:0}]}</td> -->
            <!-- FLD% -->
            <!-- <td>{[{((S.fielding.po - 0) + (S.fielding.a - 0)) / ((S.fielding.po - 0) + (S.fielding.a - 0) + (S.fielding.e - 0)) | number:3 | limitTo:4:1 |  ifEmpty:0}]}</td> -->
          </tr>
          <!-- <tr v-for="(totals,index) in totalsByYear" :key="index">
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
          </tr> -->
          <!-- <tr>
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
          </tr> -->
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
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
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
              <td v-if="game.hitting && game.hitting.ab" :v-model="game.hitting.ab" nowrap>{{ game.hitting.ab }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.r" :v-model="game.hitting.r" nowrap>{{ game.hitting.r }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.h" :v-model="game.hitting.h" nowrap>{{ game.hitting.h }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.double" :v-model="game.hitting.double" nowrap>{{ game.hitting.double }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.triple" :v-model="game.hitting.triple" nowrap>{{ game.hitting.triple }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.hr" :v-model="game.hitting.hr" nowrap>{{ game.hitting.hr }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.rbi" nowrap>{{ game.hitting.rbi }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.bb" nowrap>{{ game.hitting.bb }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.so" nowrap>{{ game.hitting.so }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.sb" nowrap>{{ game.hitting.sb }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.cs" nowrap>{{ game.hitting.cs }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.sf" nowrap>{{ game.hitting.sf }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.sh" nowrap>{{ game.hitting.sh }}</td><td v-else>0</td>
              <td v-if="game.hitting && game.hitting.hbp" nowrap>{{ game.hitting.hbp }}</td><td v-else>0</td>
              <!-- AVG: h / ab -->
              <td v-if="game.hitting && game.hitting.h && game.hitting.ab && game.hitting.ab!=0" nowrap>{{ (game.hitting.h / game.hitting.ab).toFixed(2) }}</td><td v-else>0</td>
              <!-- SLG: (h - double - triple - hr + 2*double + 3*triple + 4*hr) / ab -->
              <td>SLG</td>
              <!-- <td v-if="game.hitting && game.hitting.h && game.hitting.triple && game.hitting.ab && game.hitting.ab!=0 " nowrap>
                {{ ((game.hitting.h-0) - (game.hitting.double-0) - (game.hitting.triple-0) - (game.hitting.hr-0) + (game.hitting.double-0)*2 + (game.hitting.triple-0)*3 + (game.hitting.hr-0)*4) / game.hitting.ab }}
              </td><td v-else>0</td> -->
              <!-- <td>{{ game.hitting.triple }}</td> -->
              <!-- ((game.hitting.h-0) - (game.hitting.double-0) - (game.hitting.triple-0) - (game.hitting.hr-0) + (game.hitting.double-0)*2 + (game.hitting.triple-0)*3 + (game.hitting.hr-0)*4) / game.hitting.ab -->
              <!-- OBP: (h + bb + hbp) / (ab + bb + hbp + sf)-->
              <td>OBP</td>
              <td v-if="game.fielding && game.fielding.po" nowrap>{{ game.fielding.po }}</td><td v-else>0</td>
              <td v-if="game.fielding && game.fielding.a" nowrap>{{ game.fielding.a }}</td><td v-else>0</td>
              <td v-if="game.fielding && game.fielding.e" nowrap>{{ game.fielding.e }}</td><td v-else>0</td>
              <!-- FLD: (po + a) / (po + a + e) -->
              <td>FLD</td>
            </tr>
            <!-- ==================================== Next step: Making the Total works ==================================== -->
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.hitting.ab }}</th>
              <th>{{ totals.hitting.r }}</th>
              <th>{{ totals.hitting.h }}</th>
              <th>{{ totals.hitting.double }}</th>
              <th>{{ totals.hitting.triple }}</th>
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
              <th>{{ totals.hitting.obp }}</th>
              <th>{{ totals.fielding.po }}</th>
              <th>{{ totals.fielding.a }}</th>
              <th>{{ totals.fielding.e }}</th>
              <th>{{ totals.fielding.fld }}</th>
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
      games: [],
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
    this.calTotalsByYear(this.totalsByYear)
    // this.calTotalsByYear(this.gameYears)
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
    calTotalsByYear (years) {
      for (let i = 0; i < years.length; i++) {
        const gamesByYear = this.gamesFilterEventsByYear(years[i])
        const gamesTotalByYear = {
          schedule_year: years[i],
          gs: this.getGS(gamesByYear),
          gp: this.getGP(gamesByYear),
          result: {
            win: this.getWinGame(gamesByYear),
            lose: this.getLoseGame(gamesByYear)
          },
          hitting: {
            ab: this.calTotal(gamesByYear, 'hitting', 'ab'),
            r: this.calTotal(gamesByYear, 'hitting', 'r'),
            h: this.calTotal(gamesByYear, 'hitting', 'h'),
            double: this.calTotal(gamesByYear, 'hitting', 'double'),
            triple: this.calTotal(gamesByYear, 'hitting', 'triple'),
            hr: this.calTotal(gamesByYear, 'hitting', 'hr'),
            rbi: this.calTotal(gamesByYear, 'hitting', 'rbi'),
            bb: this.calTotal(gamesByYear, 'hitting', 'bb'),
            so: this.calTotal(gamesByYear, 'hitting', 'so'),
            sb: this.calTotal(gamesByYear, 'hitting', 'sb'),
            cs: this.calTotal(gamesByYear, 'hitting', 'cs'),
            sf: this.calTotal(gamesByYear, 'hitting', 'sf'),
            sh: this.calTotal(gamesByYear, 'hitting', 'sh'),
            avg: this.calTotal(gamesByYear, 'hitting', 'avg'),
            slg: this.calTotal(gamesByYear, 'hitting', 'slg'),
            obp: this.calTotal(gamesByYear, 'hitting', 'obp')
          },
          fielding: {
            po: this.calTotal(gamesByYear, 'fielding', 'po'),
            a: this.calTotal(gamesByYear, 'fielding', 'a'),
            e: this.calTotal(gamesByYear, 'fielding', 'e'),
            fld: this.calTotal(gamesByYear, 'fielding', 'fld')
          }
        }
        this.totalsByYear.push(gamesTotalByYear)
      }
      this.calCareerTotals()
    },
    calCareerTotals () {
      if (this.totalsByYear) {
        this.careerTotals = {
          gs: this.getGS(this.totalsByYear),
          gp: this.getGP(this.totalsByYear),
          hitting: {
            ab: this.calTotal(this.totalsByYear, 'hitting', 'ab'),
            r: this.calTotal(this.totalsByYear, 'hitting', 'r'),
            h: this.calTotal(this.totalsByYear, 'hitting', 'h'),
            double: this.calTotal(this.totalsByYear, 'hitting', 'double'),
            triple: this.calTotal(this.totalsByYear, 'hitting', 'triple'),
            hr: this.calTotal(this.totalsByYear, 'hitting', 'hr'),
            rbi: this.calTotal(this.totalsByYear, 'hitting', 'rbi'),
            bb: this.calTotal(this.totalsByYear, 'hitting', 'bb'),
            so: this.calTotal(this.totalsByYear, 'hitting', 'so'),
            sb: this.calTotal(this.totalsByYear, 'hitting', 'sb'),
            cs: this.calTotal(this.totalsByYear, 'hitting', 'cs'),
            sf: this.calTotal(this.totalsByYear, 'hitting', 'sf'),
            sh: this.calTotal(this.totalsByYear, 'hitting', 'sh')
            // avg: this.calTotal(gamesByYear, 'hitting', 'avg'),
            // slg: this.calTotal(gamesByYear, 'hitting', 'slg'),
            // obp: this.calTotal(gamesByYear, 'hitting', 'obp')
          },
          fielding: {
            po: this.calTotal(this.totalsByYear, 'fielding', 'po'),
            a: this.calTotal(this.totalsByYear, 'fielding', 'a'),
            e: this.calTotal(this.totalsByYear, 'fielding', 'e')
            // fld: this.calTotal(gamesByYear, 'fielding', 'fld')
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
