<template>
 <h5>KickerPunter</h5>
  <div>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
            <tr>
              <th colspan="2" rowspan="2" style="padding-bottom: 20px;">{{ year }} Games</th>
              <th colspan="9" rowspan="2" style="padding-bottom: 20px;">Punting</th>
              <th colspan="14">Kicking</th>
            </tr>
            <tr>
                <!-- <th colspan="3">{{ year }} Games</th> -->
                <!-- <th colspan="9">Punting</th> -->
                <th colspan="5">Kickoffs</th>
                <th colspan="5">Field Goals</th>
                <th colspan="3">PAT</th>
                <th colspan="1">PTS</th>
            </tr>
        </thead>
        <tbody>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>Punt</th>
            <th>YDS</th>
            <th>AVG</th>
            <th>LNG</th>
            <th>IN20</th>
            <th>50+</th>
            <th>FC</th>
            <th>TB</th>
            <th>BLK</th>
            <th>NO</th>
            <th>YDS</th>
            <th>AVG</th>
            <th>TB</th>
            <th>OB</th>
            <th>FGM</th>
            <th>FGA</th>
            <th>PCT</th>
            <th>LNG</th>
            <th>BLK</th>
            <th>XPM</th>
            <th>XPA</th>
            <th>PCT</th>
            <th>PTS</th>
          </tr>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.punt.no }}</td>
            <td>{{ totals.punt.yds }}</td>
            <td>{{ totals.punt.avg }}</td>
            <td>{{ totals.punt.long }}</td>
            <td>{{ totals.punt.inside20 }}</td>
            <td>{{ totals.punt.plus50 }}</td>
            <td>{{ totals.punt.fc }}</td>
            <td>{{ totals.punt.tb }}</td>
            <td>{{ totals.punt.blkd }}</td>
            <td>{{ totals.ko.no }}</td>
            <td>{{ totals.ko.yds }}</td>
            <td>{{ totals.ko.avg }}</td>
            <td>{{ totals.ko.tb }}</td>
            <td>{{ totals.ko.ob }}</td>
            <td>{{ totals.fg.made }}</td>
            <td>{{ totals.fg.att }}</td>
            <td>{{ totals.fg.pct }}</td>
            <td>{{ totals.fg.long }}</td>
            <td>{{ totals.fg.blkd }}</td>
            <td>{{ totals.pat.kickmade }}</td>
            <td>{{ totals.pat.kickatt }}</td>
            <td>{{ totals.pat.pct }}</td>
            <td>{{ totals.pts }}</td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
            <th v-if="careerTotals.punt && careerTotals.punt.no" nowrap>{{ careerTotals.punt.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.yds" nowrap>{{ careerTotals.punt.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.avg" nowrap>{{ careerTotals.punt.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.long" nowrap>{{ careerTotals.punt.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.inside20" nowrap>{{ careerTotals.punt.inside20 }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.plus50" nowrap>{{ careerTotals.punt.plus50 }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.fc" nowrap>{{ careerTotals.punt.fc }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.tb" nowrap>{{ careerTotals.punt.tb }}</th><th v-else>0</th>
            <th v-if="careerTotals.punt && careerTotals.punt.blkd" nowrap>{{ careerTotals.punt.blkd }}</th><th v-else>0</th>
            <th v-if="careerTotals.ko && careerTotals.ko.no" nowrap>{{ careerTotals.ko.no }}</th><th v-else>0</th>
            <th v-if="careerTotals.ko && careerTotals.ko.yds" nowrap>{{ careerTotals.ko.yds }}</th><th v-else>0</th>
            <th v-if="careerTotals.ko && careerTotals.ko.avg" nowrap>{{ careerTotals.ko.avg }}</th><th v-else>0</th>
            <th v-if="careerTotals.ko && careerTotals.ko.tb" nowrap>{{ careerTotals.ko.tb }}</th><th v-else>0</th>
            <th v-if="careerTotals.ko && careerTotals.ko.ob" nowrap>{{ careerTotals.ko.ob }}</th><th v-else>0</th>
            <th v-if="careerTotals.fg && careerTotals.fg.made" nowrap>{{ careerTotals.fg.made }}</th><th v-else>0</th>
            <th v-if="careerTotals.fg && careerTotals.fg.att" nowrap>{{ careerTotals.fg.att }}</th><th v-else>0</th>
            <th v-if="careerTotals.fg && careerTotals.fg.pct" nowrap>{{ careerTotals.fg.pct }}</th><th v-else>0</th>
            <th v-if="careerTotals.fg && careerTotals.fg.long" nowrap>{{ careerTotals.fg.long }}</th><th v-else>0</th>
            <th v-if="careerTotals.fg && careerTotals.fg.blkd" nowrap>{{ careerTotals.fg.blkd }}</th><th v-else>0</th>
            <th v-if="careerTotals.pat && careerTotals.pat.kickmade" nowrap>{{ careerTotals.pat.kickmade }}</th><th v-else>0</th>
            <th v-if="careerTotals.pat && careerTotals.pat.kickatt" nowrap>{{ careerTotals.pat.kickatt }}</th><th v-else>0</th>
            <th v-if="careerTotals.pat && careerTotals.pat.pct" nowrap>{{ careerTotals.pat.pct }}</th><th v-else>0</th>
            <th v-if="careerTotals.pts" nowrap>{{ careerTotals.pts }}</th><th v-else>0</th>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-for="(year, index) in gameYears" :key="index">
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th colspan="3" rowspan="2" style="padding-bottom: 20px;">{{ year }} Games</th>
              <th colspan="9" rowspan="2" style="padding-bottom: 20px;">Punting</th>
              <th colspan="14">Kicking</th>
            </tr>
            <tr>
                <!-- <th colspan="3">{{ year }} Games</th> -->
                <!-- <th colspan="9">Punting</th> -->
                <th colspan="5">Kickoffs</th>
                <th colspan="5">Field Goals</th>
                <th colspan="3">PAT</th>
                <th colspan="1">PTS</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>Opponent</th>
              <th>Date</th>
              <th>Result</th>
              <th>Punt</th>
              <th>YDS</th>
              <th>AVG</th>
              <th>LNG</th>
              <th>IN20</th>
              <th>50+</th>
              <th>FC</th>
              <th>TB</th>
              <th>BLK</th>
              <th>NO</th>
              <th>YDS</th>
              <th>AVG</th>
              <th>TB</th>
              <th>OB</th>
              <th>FGM</th>
              <th>FGA</th>
              <th>PCT</th>
              <th>LNG</th>
              <th>BLK</th>
              <th>XPM</th>
              <th>XPA</th>
              <th>PCT</th>
              <th>PTS</th>
            </tr>
            <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
              <td nowrap style="text-align: left">
                <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
              </td>
              <td nowrap>{{ formDateStr(game.event_date) }}</td>
              <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
              <td v-if="game.punt && game.punt.no" nowrap>{{ game.punt.no }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.yds" nowrap>{{ game.punt.yds }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.no && game.punt.yds" nowrap>{{ (game.punt.yds / game.punt.no).toFixed(2)  }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.long" nowrap>{{ game.punt.long }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.inside20" nowrap>{{ game.punt.inside20 }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.plus50" nowrap>{{ game.punt.plus50 }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.fc" nowrap>{{ game.punt.fc }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.tb" nowrap>{{ game.punt.tb }}</td><td v-else>0</td>
              <td v-if="game.punt && game.punt.blkd" nowrap>{{ game.punt.blkd }}</td><td v-else>0</td>
              <td v-if="game.ko && game.ko.no" nowrap>{{ game.ko.no }}</td><td v-else>0</td>
              <td v-if="game.ko && game.ko.yds" nowrap>{{ game.ko.yds }}</td><td v-else>0</td>
              <td v-if="game.ko && game.ko.yds && game.ko.no" nowrap>{{ (game.ko.yds/game.ko.no).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.ko && game.ko.tb" nowrap>{{ game.ko.tb }}</td><td v-else>0</td>
              <td v-if="game.ko && game.ko.ob" nowrap>{{ game.ko.ob }}</td><td v-else>0</td>
              <td v-if="game.fg && game.fg.made" nowrap>{{ game.fg.made }}</td><td v-else>0</td>
              <td v-if="game.fg && game.fg.att" nowrap>{{ game.fg.att }}</td><td v-else>0</td>
              <td v-if="game.fg && game.fg.made && game.fg.att" nowrap>{{ (game.fg.made / game.fg.att * 100).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.fg && game.fg.long" nowrap>{{ game.fg.long }}</td><td v-else>0</td>
              <td v-if="game.fg && game.fg.blkd" nowrap>{{ game.fg.blkd }}</td><td v-else>0</td>
              <td v-if="game.pat && game.pat.kickmade" nowrap>{{ game.pat.kickmade }}</td><td v-else>0</td>
              <td v-if="game.pat && game.pat.kickatt" nowrap>{{ game.pat.kickatt }}</td><td v-else>0</td>
              <td v-if="game.pat && game.pat.kickmade && game.pat.kickatt" nowrap>{{ (game.pat.kickmade / game.pat.kickatt * 100).toFixed(2) }}</td><td v-else>0</td>
              <td v-if="game.fg && game.fg.made && game.pat && game.pat.kickmade" nowrap>{{ game.fg.made * 3 + (game.pat.kickmade - 0) }}</td><td v-else>0</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.punt.no }}</th>
              <th>{{ totals.punt.yds }}</th>
              <th>{{ totals.punt.avg }}</th>
              <th>{{ totals.punt.long }}</th>
              <th>{{ totals.punt.inside20 }}</th>
              <th>{{ totals.punt.plus50 }}</th>
              <th>{{ totals.punt.fc }}</th>
              <th>{{ totals.punt.tb }}</th>
              <th>{{ totals.punt.blkd }}</th>
              <th>{{ totals.ko.no }}</th>
              <th>{{ totals.ko.yds }}</th>
              <th>{{ totals.ko.avg }}</th>
              <th>{{ totals.ko.tb }}</th>
              <th>{{ totals.ko.ob }}</th>
              <th>{{ totals.fg.made }}</th>
              <th>{{ totals.fg.att }}</th>
              <th>{{ totals.fg.pct }}</th>
              <th>{{ totals.fg.long }}</th>
              <th>{{ totals.fg.blkd }}</th>
              <th>{{ totals.pat.kickmade }}</th>
              <th>{{ totals.pat.kickatt }}</th>
              <th>{{ totals.pat.pct }}</th>
              <th>{{ totals.pts }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'KickerPunter',
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
          punt: {
            no: this.calTotal(gamesByYear, 'punt', 'no'),
            yds: this.calTotal(gamesByYear, 'punt', 'yds'),
            avg: this.getAVG(this.calTotal(gamesByYear, 'punt', 'yds'), this.calTotal(gamesByYear, 'punt', 'no')), // (game.punt.yds / game.punt.no).toFixed(2)
            long: this.getBiggestLongByYear(gamesByYear, 'punt', 'long'),
            inside20: this.calTotal(gamesByYear, 'punt', 'inside20'),
            plus50: this.calTotal(gamesByYear, 'punt', 'plus50'),
            fc: this.calTotal(gamesByYear, 'punt', 'fc'),
            tb: this.calTotal(gamesByYear, 'punt', 'tb'),
            blkd: this.calTotal(gamesByYear, 'punt', 'blkd')
          },
          ko: {
            no: this.calTotal(gamesByYear, 'ko', 'no'),
            yds: this.calTotal(gamesByYear, 'ko', 'yds'),
            avg: this.getAVG(this.calTotal(gamesByYear, 'ko', 'yds'), this.calTotal(gamesByYear, 'ko', 'no')), // (game.ko.yds/game.ko.no).toFixed(2)
            tb: this.calTotal(gamesByYear, 'ko', 'tb'),
            ob: this.calTotal(gamesByYear, 'ko', 'ob')
          },
          fg: {
            made: this.calTotal(gamesByYear, 'fg', 'made'),
            att: this.calTotal(gamesByYear, 'fg', 'att'),
            pct: this.getPCT(this.calTotal(gamesByYear, 'fg', 'made'), this.calTotal(gamesByYear, 'fg', 'att')), // (game.fg.made / game.fg.att * 100).toFixed(2)
            long: this.getBiggestLongByYear(gamesByYear, 'fg', 'long'),
            blkd: this.calTotal(gamesByYear, 'fg', 'blkd')
          },
          pat: {
            kickmade: this.calTotal(gamesByYear, 'pat', 'kickmade'),
            kickatt: this.calTotal(gamesByYear, 'pat', 'kickatt'),
            pct: this.getPCT(this.calTotal(gamesByYear, 'pat', 'kickmade'), this.calTotal(gamesByYear, 'pat', 'kickatt')) // (game.pat.kickmade / game.pat.kickatt * 100).toFixed(2)
          },
          pts: this.getPTS(this.calTotal(gamesByYear, 'fg', 'made'), this.calTotal(gamesByYear, 'pat', 'kickmade')) // game.fg.made * 3 + (game.pat.kickmade - 0)
        }
        this.totalsByYear.push(gamesTotalByYear)
      }
      this.calCareerTotals()
    },
    calCareerTotals () {
      if (this.totalsByYear) {
        this.careerTotals = {
          gp: this.getGP(this.totalsByYear),
          punt: {
            no: this.calTotal(this.totalsByYear, 'punt', 'no'),
            yds: this.calTotal(this.totalsByYear, 'punt', 'yds'),
            avg: this.getAVG(this.calTotal(this.totalsByYear, 'punt', 'yds'), this.calTotal(this.totalsByYear, 'punt', 'no')), // (game.punt.yds / game.punt.no).toFixed(2)
            long: this.getBiggestLongByYear(this.totalsByYear, 'punt', 'long'),
            inside20: this.calTotal(this.totalsByYear, 'punt', 'inside20'),
            plus50: this.calTotal(this.totalsByYear, 'punt', 'plus50'),
            fc: this.calTotal(this.totalsByYear, 'punt', 'fc'),
            tb: this.calTotal(this.totalsByYear, 'punt', 'tb'),
            blkd: this.calTotal(this.totalsByYear, 'punt', 'blkd')
          },
          ko: {
            no: this.calTotal(this.totalsByYear, 'ko', 'no'),
            yds: this.calTotal(this.totalsByYear, 'ko', 'yds'),
            avg: this.getAVG(this.calTotal(this.totalsByYear, 'ko', 'yds'), this.calTotal(this.totalsByYear, 'ko', 'no')), // (game.ko.yds/game.ko.no).toFixed(2)
            tb: this.calTotal(this.totalsByYear, 'ko', 'tb'),
            ob: this.calTotal(this.totalsByYear, 'ko', 'ob')
          },
          fg: {
            made: this.calTotal(this.totalsByYear, 'fg', 'made'),
            att: this.calTotal(this.totalsByYear, 'fg', 'att'),
            pct: this.getPCT(this.calTotal(this.totalsByYear, 'fg', 'made'), this.calTotal(this.totalsByYear, 'fg', 'att')), // (game.fg.made / game.fg.att * 100).toFixed(2)
            long: this.getBiggestLongByYear(this.totalsByYear, 'fg', 'long'),
            blkd: this.calTotal(this.totalsByYear, 'fg', 'blkd')
          },
          pat: {
            kickmade: this.calTotal(this.totalsByYear, 'pat', 'kickmade'),
            kickatt: this.calTotal(this.totalsByYear, 'pat', 'kickatt'),
            pct: this.getPCT(this.calTotal(this.totalsByYear, 'pat', 'kickmade'), this.calTotal(this.totalsByYear, 'pat', 'kickatt')) // (game.pat.kickmade / game.pat.kickatt * 100).toFixed(2)
          },
          pts: this.getPTS(this.calTotal(this.totalsByYear, 'fg', 'made'), this.calTotal(this.totalsByYear, 'pat', 'kickmade')) // game.fg.made * 3 + (game.pat.kickmade - 0)
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
    getAVG (yds, no) {
      if (no === 0) return 0
      const ya = (yds / no).toFixed(2)
      return ya
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
