<template>
  <h2>Basketball Box Score</h2>
  <div class="game-description" style="text-align:left;" nowrap>
    <!-- 2017 Concordia Irvine -->
    <h6><strong v-if="stats.bbgame.venue.location">Location:</strong> {{ stats.bbgame.venue.location }} </h6>
    <h6><strong v-if="stats.bbgame.venue.time">Time:</strong> {{ stats.bbgame.venue.time }}</h6>
    <h6><strong v-if="stats.bbgame.venue.date">Date:</strong> {{ stats.bbgame.venue.date }} </h6>
    <h6><strong v-if="stats.bbgame.venue.attend">Attendance:</strong> {{ stats.bbgame.venue.attend }} </h6>
  </div>

  <div>
    <!-- <h4>Box Score</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th>Linescore</th>
            <th v-for="(inn, index) in stats.bbgame.plays.inning" :key="index">{{inn.number}}</th>
            <th>R</th>
            <th>H</th>
            <th>E</th>
            <th>LOB</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td>{{stats.bbgame.team[teamIndex].id}}</td>
            <td v-for="(s, index) in stats.bbgame.team[teamIndex].linescore.lineinn" :key="index">{{s.score}}</td>
            <td>{{stats.bbgame.team[teamIndex].linescore.runs}}</td>
            <td>{{stats.bbgame.team[teamIndex].linescore.hits}}</td>
            <td>{{stats.bbgame.team[teamIndex].linescore.errs}}</td>
            <td>{{stats.bbgame.team[teamIndex].linescore.lob}}</td>
          </tr>
        </tbody>
      </table>
    </div> -->

    <div class="mt-3" v-for="(i, teamIndex) in 2" :key="teamIndex">
      <h6 style="text-align:left;"><strong>{{stats.bbgame.team[teamIndex].name}} - {{stats.bbgame.team[teamIndex].linescore.score}}</strong> , <small style="text-align:left;">Record: {{stats.bbgame.team[teamIndex].record}}</small> </h6>
      <div class="table-responsive">
        <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th>NO.</th>
              <th style="text-align:left;">Starters</th>
              <th>MIN</th>
              <th>FG-FGA</th>
              <th>3P-3PA</th>
              <th>FT-FTA</th>
              <th>OFF</th>
              <th>DEF</th>
              <th>REB</th>
              <th>AST</th>
              <th>BLK</th>
              <th>STL</th>
              <th>PF</th>
              <th>TO</th>
              <th>PTS</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in startersInGameFilter(teamIndex)" :key="player.uni">
              <td v-if="player.uni">{{player.uni}}</td><td v-else>0</td>
              <td nowrap style="text-align:left;">
                {{player.name}}
                <span style="float:right; text-transform:uppercase;">{{player.pos}}</span>
              </td>
              <td>{{player.stats.min}}</td>
              <td>{{player.stats.fgm}}-{{player.stats.fga}} </td>
              <td>{{player.stats.fgm3}}-{{player.stats.fga3}}</td>
              <td>{{player.stats.ftm}}-{{player.stats.fta}}</td>
              <td>{{player.stats.oreb}}</td>
              <td>{{player.stats.dreb}}</td>
              <td>{{player.stats.treb}}</td>
              <td>{{player.stats.ast}}</td>
              <td>{{player.stats.blk}}</td>
              <td>{{player.stats.stl}}</td>
              <td>{{player.stats.pf}}</td>
              <td>{{player.stats.to}}</td>
              <td>{{player.stats.tp}}</td>
            </tr>
          </tbody>
          <thead>
            <tr>
              <th>NO.</th>
              <th style="text-align:left;">Starters</th>
              <th>MIN</th>
              <th nowrap>FG-FGA</th>
              <th nowrap>3P-3PA</th>
              <th nowrap>FT-FTA</th>
              <th>OFF</th>
              <th>DEF</th>
              <th>REB</th>
              <th>AST</th>
              <th>BLK</th>
              <th>STL</th>
              <th>PF</th>
              <th>TO</th>
              <th>PTS</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in benchsInGameFilter(teamIndex)" :key="player.uni">
              <td v-if="player.uni && player.uni !== 'TM'">{{player.uni}}</td><td v-else-if="player.uni === 'TM'"> - </td><td v-else>0</td>
              <td nowrap style="text-align:left;">
                {{player.name}}
              </td>
              <td>{{player.stats.min}}</td>
              <td>{{player.stats.fgm}}-{{player.stats.fga}} </td>
              <td>{{player.stats.fgm3}}-{{player.stats.fga3}}</td>
              <td>{{player.stats.ftm}}-{{player.stats.fta}}</td>
              <td>{{player.stats.oreb}}</td>
              <td>{{player.stats.dreb}}</td>
              <td>{{player.stats.treb}}</td>
              <td>{{player.stats.ast}}</td>
              <td>{{player.stats.blk}}</td>
              <td>{{player.stats.stl}}</td>
              <td>{{player.stats.pf}}</td>
              <td>{{player.stats.to}}</td>
              <td>{{player.stats.tp}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BasketballScoreBox',
  props: ['selected', 'stats'],
  data () {
    return {
      game: []
    }
  },
  mounted () {
  },
  methods: {
    startersInGameFilter (teamIndex) {
      this.game = this.stats
      const players = this.game.bbgame.team[teamIndex].player.filter(player => player.gs)
      // for (var i = 0; i < players.length; i++) {
      //   if (!players[i].hitting.h) players[i].hitting.h = 0
      //   if (!players[i].hitting.double) players[i].hitting.double = 0
      //   if (!players[i].hitting.triple) players[i].hitting.triple = 0
      //   if (!players[i].hitting.hr) players[i].hitting.hr = 0
      //   if (!players[i].hitting.ab) players[i].hitting.ab = 0
      //   if (!players[i].hitting.bb) players[i].hitting.bb = 0
      //   if (!players[i].hitting.hbp) players[i].hitting.hbp = 0
      //   if (!players[i].hitting.sf) players[i].hitting.sf = 0
      // }
      // this.reorganizeBattingTotals(teamIndex)
      return players
    },
    benchsInGameFilter (teamIndex) {
      this.game = this.stats
      const players = this.game.bbgame.team[teamIndex].player.filter(player => (!player.gs && player.gp === '1'))
      // for (var i = 0; i < players.length; i++) {
      //   if (!players[i].pitching.ip) players[i].pitching.ip = 0
      //   if (!players[i].pitching.h) players[i].pitching.h = 0
      //   if (!players[i].pitching.r) players[i].pitching.r = 0
      //   if (!players[i].pitching.er) players[i].pitching.er = 0
      //   if (!players[i].pitching.bb) players[i].pitching.bb = 0
      //   if (!players[i].pitching.so) players[i].pitching.so = 0
      //   if (!players[i].pitching.double) players[i].pitching.double = 0
      //   if (!players[i].pitching.triple) players[i].pitching.triple = 0
      //   if (!players[i].pitching.hr) players[i].pitching.hr = 0
      //   if (!players[i].pitching.bf) players[i].pitching.bf = 0
      //   if (!players[i].pitching.ab) players[i].pitching.ab = 0
      //   if (!players[i].pitching.wp) players[i].pitching.wp = 0
      //   if (!players[i].pitching.hbp) players[i].pitching.hbp = 0
      //   if (!players[i].pitching.bk) players[i].pitching.bk = 0
      //   if (!players[i].pitching.sfa) players[i].pitching.sfa = 0
      //   if (!players[i].pitching.sha) players[i].pitching.sha = 0
      //   players[i].pitching.innings_pitched = this.getInnings_pitched(players[i].pitching.ip)
      // }
      // this.reorganizePitchingTotals(teamIndex)
      return players
    },
    getInnings_pitched (ip) {
      if (ip.indexOf('.0') === -1) {
        var split = ip.split('.')
        split[1] = split[1] / 3
        var join = (split[0] - 0) + (split[1] - 0)
        return join
      } else {
        return ip
      }
    },
    reorganizeBattingTotals (teamIndex) {
      // console.log(this.game.bbgame.team[teamIndex].id)
      const totals = this.game.bbgame.team[teamIndex].totals
      if (!totals.hitting.h) totals.hitting.h = 0
      if (!totals.hitting.double) totals.hitting.double = 0
      if (!totals.hitting.triple) totals.hitting.triple = 0
      if (!totals.hitting.hr) totals.hitting.hr = 0
      // ===== Initial AVG and SLG value =====
      if (!totals.hitting.ab) {
        totals.hitting.ab = 0
        totals.hitting.avg = 'N/A'
        totals.hitting.slg = 'N/A'
      } else {
        totals.hitting.avg = ((totals.hitting.h - 0) / (totals.hitting.ab - 0)).toFixed(3)
        totals.hitting.slg = (((totals.hitting.h - 0) + (totals.hitting.double - 0) + (totals.hitting.triple - 0) * 2 + (totals.hitting.hr - 0) * 3) / (totals.hitting.ab - 0)).toFixed(3)
      }
      if (!totals.hitting.r) totals.hitting.r = 0
      if (!totals.hitting.rbi) totals.hitting.rbi = 0
      if (!totals.hitting.bb) totals.hitting.bb = 0
      if (!totals.hitting.so) totals.hitting.so = 0
      if (!totals.hitting.sb) totals.hitting.sb = 0
      if (!totals.hitting.cs) totals.hitting.cs = 0
      if (!totals.hitting.sf) totals.hitting.sf = 0
      if (!totals.hitting.sh) totals.hitting.sh = 0
      if (!totals.hitting.hbp) totals.hitting.hbp = 0
      if (!totals.fielding.po) totals.fielding.po = 0
      if (!totals.fielding.a) totals.fielding.a = 0
      if (!totals.fielding.e) totals.fielding.e = 0
      // ===== Initial OBP value =====
      if (((totals.hitting.ab - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0) + (totals.hitting.sf - 0)) === 0) {
        totals.hitting.obp = 'N/A'
      } else {
        totals.hitting.obp = (((totals.hitting.h - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0)) / ((totals.hitting.ab - 0) + (totals.hitting.bb - 0) + (totals.hitting.hbp - 0) + (totals.hitting.sf - 0))).toFixed(3)
      }
      // ===== Initial FLD value =====
      if (((totals.fielding.po - 0) + (totals.fielding.a - 0) + (totals.fielding.e - 0)) === 0) {
        totals.fielding.fld = 'N/A'
      } else {
        totals.fielding.fld = (((totals.fielding.po - 0) + (totals.fielding.a - 0)) / ((totals.fielding.po - 0) + (totals.fielding.a - 0) + (totals.fielding.e - 0))).toFixed(3)
      }
      this.battingTotals = totals
      // console.log('game', this.game) // -- fix >> should run only two times but run four times
    },
    reorganizePitchingTotals (teamIndex) {
      const pitching = this.game.bbgame.team[teamIndex].totals.pitching
      if (!pitching.ip) pitching.ip = 0
      if (!pitching.h) pitching.h = 0
      if (!pitching.r) pitching.r = 0
      if (!pitching.er) pitching.er = 0
      if (!pitching.bb) pitching.bb = 0
      if (!pitching.so) pitching.so = 0
      if (!pitching.double) pitching.double = 0
      if (!pitching.triple) pitching.triple = 0
      if (!pitching.hr) pitching.hr = 0
      if (!pitching.bf) pitching.bf = 0
      if (!pitching.wp) pitching.wp = 0
      if (!pitching.hbp) pitching.hbp = 0
      if (!pitching.bk) pitching.bk = 0
      if (!pitching.sfa) pitching.sfa = 0
      if (!pitching.sha) pitching.sha = 0
      pitching.innings_pitched = this.getInnings_pitched(pitching.ip)

      if (!pitching.ab) {
        pitching.ab = 0
        pitching.avg = 'N/A'
      } else {
        pitching.avg = ((pitching.h - 0) / (pitching.ab - 0)).toFixed(3)
      }
      if (!pitching.innings_pitched) {
        pitching.era = 'N/A'
      } else {
        pitching.era = (((pitching.er - 0) * 9) / pitching.innings_pitched).toFixed(3)
      }
      this.pitchingTotals = pitching
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
