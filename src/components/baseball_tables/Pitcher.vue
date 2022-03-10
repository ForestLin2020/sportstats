<template>
 <h5>Pitcher</h5>
  <div class="career-and-season">
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th colspan="22">Career & Season Stats</th>
          </tr>
          <tr>
            <th>Year</th>
            <th>GP</th>
            <th>GS</th>
            <th>W</th>
            <th>L</th>
            <th>CG</th>
            <th>SHO</th>
            <th>SV</th>
            <th>IP</th>
            <th>H</th>
            <th>R</th>
            <th>ER</th>
            <th>BB</th>
            <th>SO</th>
            <th>2B</th>
            <th>3B</th>
            <th>HR</th>
            <th>AB</th>
            <th>HBP</th>
            <th>AVG</th>
            <th>ERA</th>
            <th>WHIP</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(totals,index) in totalsByYear" :key="index">
            <td>{{ totals.schedule_year }}</td>
            <td>{{ totals.gp }}</td>
            <td>{{ totals.gs }}</td>
            <td>{{ totals.result.win }}</td>
            <td>{{ totals.result.lose }}</td>
            <td>{{ totals.pitching.cg }}</td>
            <td>{{ totals.pitching.sho }}</td>
            <td>{{ totals.pitching.save }}</td>
            <td>{{ totals.pitching.ip }}</td>
            <td>{{ totals.pitching.h }}</td>
            <td>{{ totals.pitching.r }}</td>
            <td>{{ totals.pitching.er }}</td>
            <td>{{ totals.pitching.bb }}</td>
            <td>{{ totals.pitching.so }}</td>
            <td>{{ totals.pitching.double }}</td>
            <td>{{ totals.pitching.triple }}</td>
            <td>{{ totals.pitching.hr }}</td>
            <td>{{ totals.pitching.ab }}</td>
            <td>{{ totals.pitching.hbp }}</td>
            <td>{{ (totals.pitching.h / totals.pitching.ab).toFixed(3) }}</td>
            <td>{{ (totals.pitching.er / totals.pitching.innings_pitched * 9).toFixed(2) }}</td>
            <td>{{ ((totals.pitching.h + totals.pitching.bb) / totals.pitching.innings_pitched).toFixed(2) }} </td>
          </tr>
          <tr>
            <th>TOTALS:</th>
            <th>{{ careerTotals.gp }}</th>
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
            <th v-if="careerTotals.pitching && careerTotals.pitching.er && careerTotals.pitching.innings_pitched">{{ (careerTotals.pitching.er / careerTotals.pitching.innings_pitched * 9).toFixed(2) }}</th><th v-else>0</th>
            <th v-if="careerTotals.pitching && careerTotals.pitching.h && careerTotals.pitching.bb && careerTotals.pitching.innings_pitched">
              {{ ((careerTotals.pitching.h + careerTotals.pitching.bb) / careerTotals.pitching.innings_pitched).toFixed(2) }}
            </th><th v-else>0</th>
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
              <th>IP</th>
              <th>H</th>
              <th>R</th>
              <th>ER</th>
              <th>BB</th>
              <th>SO</th>
              <th>2B</th>
              <th>3B</th>
              <th>HR</th>
              <th>AB</th>
              <th>HBP</th>
              <th>AVG</th>
              <th>ERA</th>
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
              <td v-if="game.pitching && game.pitching.ip" nowrap>{{ game.pitching.ip }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.h" nowrap>{{ game.pitching.h }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.r" nowrap>{{ game.pitching.r }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.er" nowrap>{{ game.pitching.er }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.bb" nowrap>{{ game.pitching.bb }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.so" nowrap>{{ game.pitching.so }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.double" nowrap>{{ game.pitching.double }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.triple" nowrap>{{ game.pitching.triple }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.hr" nowrap>{{ game.pitching.hr }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.ab" nowrap>{{ game.pitching.ab }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.hbp" nowrap>{{ game.pitching.hbp }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.h" nowrap>{{ ((game.pitching.h - 0) / (game.pitching.ab - 0)).toFixed(3)  }}</td><td v-else>-</td>
              <td v-if="game.pitching && game.pitching.er" nowrap>{{ ((game.pitching.er - 0) / (game.pitching.innings_pitched - 0) * 9).toFixed(2) }}</td><td v-else>-</td>
            </tr>
            <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
              <th>TOTALS:</th>
              <th>&nbsp;</th>
              <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
              <th>{{ totals.pitching.ip }}</th>
              <th>{{ totals.pitching.h }}</th>
              <th>{{ totals.pitching.r }}</th>
              <th>{{ totals.pitching.er }}</th>
              <th>{{ totals.pitching.bb }}</th>
              <th>{{ totals.pitching.so }}</th>
              <th>{{ totals.pitching.double }}</th>
              <th>{{ totals.pitching.triple }}</th>
              <th>{{ totals.pitching.hr }}</th>
              <th>{{ totals.pitching.ab }}</th>
              <th>{{ totals.pitching.hbp }}</th>
              <th>{{ (totals.pitching.h / totals.pitching.ab).toFixed(3) }}</th>
              <th>{{ (totals.pitching.er / totals.pitching.innings_pitched * 9).toFixed(2) }}</th>
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
        if (!games[i].pitching) games[i].pitching = {}
        if (!games[i].pitching.ip) {
          games[i].pitching.innings_pitched = this.getInnings_pitched(0)
        } else {
          games[i].pitching.innings_pitched = this.getInnings_pitched(games[i].pitching.ip)
        }
      }
      return games
    },
    getInnings_pitched (ip) {
      ip = ip.toString()
      if (ip.indexOf('.1') !== -1) {
        return ip.split('.')[0] + '.3334'
      } else if (ip.indexOf('.2') !== -1) {
        return ip.split('.')[0] + '.6667'
      } else {
        return ip
      }
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
          pitching: {
            ip: this.getIP(this.calTotal(gamesByYear, 'pitching', 'ip')),
            h: this.calTotal(gamesByYear, 'pitching', 'h'),
            r: this.calTotal(gamesByYear, 'pitching', 'r'),
            er: this.calTotal(gamesByYear, 'pitching', 'er'),
            bb: this.calTotal(gamesByYear, 'pitching', 'bb'),
            so: this.calTotal(gamesByYear, 'pitching', 'so'),
            double: this.calTotal(gamesByYear, 'pitching', 'double'),
            triple: this.calTotal(gamesByYear, 'pitching', 'triple'),
            hr: this.calTotal(gamesByYear, 'pitching', 'hr'),
            ab: this.calTotal(gamesByYear, 'pitching', 'ab'),
            hbp: this.calTotal(gamesByYear, 'pitching', 'hbp'),
            innings_pitched: this.getInnings_pitched(this.calTotal(gamesByYear, 'pitching', 'ip')),
            save: this.calTotal(gamesByYear, 'pitching', 'save'),
            sho: this.calTotal(gamesByYear, 'pitching', 'sho'),
            cg: this.calTotal(gamesByYear, 'pitching', 'cg')
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
          pitching: {
            ip: this.getIP(this.calTotal(this.totalsByYear, 'pitching', 'ip')),
            h: this.calTotal(this.totalsByYear, 'pitching', 'h'),
            r: this.calTotal(this.totalsByYear, 'pitching', 'r'),
            er: this.calTotal(this.totalsByYear, 'pitching', 'er'),
            bb: this.calTotal(this.totalsByYear, 'pitching', 'bb'),
            so: this.calTotal(this.totalsByYear, 'pitching', 'so'),
            double: this.calTotal(this.totalsByYear, 'pitching', 'double'),
            triple: this.calTotal(this.totalsByYear, 'pitching', 'triple'),
            hr: this.calTotal(this.totalsByYear, 'pitching', 'hr'),
            ab: this.calTotal(this.totalsByYear, 'pitching', 'ab'),
            hbp: this.calTotal(this.totalsByYear, 'pitching', 'hbp'),
            innings_pitched: this.getInnings_pitched(this.calTotal(this.totalsByYear, 'pitching', 'ip')),
            save: this.calTotal(this.totalsByYear, 'pitching', 'save'),
            sho: this.calTotal(this.totalsByYear, 'pitching', 'sho'),
            cg: this.calTotal(this.totalsByYear, 'pitching', 'cg')
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
          if (statsKey === 'ip') console.log('ip', games[i][categoryKey][statsKey] - 0)
          total += (games[i][categoryKey][statsKey] - 0)
          if (statsKey === 'ip') console.log('total', total)
        }
      }
      // Math.round((total + Number.EPSILON) * 100) / 100, fixing weird issue: 6.3 + 0.1 = 6.399999999995
      return Math.round((total + Number.EPSILON) * 100) / 100
    },
    getTotalsByYear (year) {
      return this.totalsByYear.filter(total => total.schedule_year === year)
    },
    getIP (ip) {
      ip = ip.toString()

      // string is not include '.0': string.indexOf('.0') === -1
      // string include interger and tenths: ip.split('.').length !== 1
      if (ip.indexOf('.0') === -1 && ip.split('.').length !== 1) {
        var split = ip.split('.')
        const quotient = Math.floor(parseInt(split[1]) / 3)
        const remainder = parseInt(split[1]) % 3
        split[0] = parseInt(split[0]) + quotient
        split[1] = remainder
        ip = split.join('.')
        return ip
      }
      return ip
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
