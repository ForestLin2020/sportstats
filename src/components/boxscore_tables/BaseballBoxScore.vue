<template>
  <h2>Baseball Box Score</h2>
  <div class="game-description" nowrap>
    <!-- 2017 Concordia Irvine -->
    <h6 style="text-align:left;"><strong v-if="stats.bsgame.venue.location">Location:</strong> {{ stats.bsgame.venue.location }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.bsgame.venue.time">Time:</strong> {{ stats.bsgame.venue.time }} <strong>Date:</strong> {{ stats.bsgame.venue.date }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.bsgame.venue.attend">Attendance:</strong> {{ stats.bsgame.venue.attend }} </h6>
  </div>

  <div>
    <h4>Box Score</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th>Linescore</th>
            <th v-for="(inn, index) in stats.bsgame.plays.inning" :key="index">{{inn.number}}</th>
            <th>R</th>
            <th>H</th>
            <th>E</th>
            <th>LOB</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td>{{stats.bsgame.team[teamIndex].id}}</td>
            <td v-for="(s, index) in stats.bsgame.team[teamIndex].linescore.lineinn" :key="index">{{s.score}}</td>
            <td>{{stats.bsgame.team[teamIndex].linescore.runs}}</td>
            <td>{{stats.bsgame.team[teamIndex].linescore.hits}}</td>
            <td>{{stats.bsgame.team[teamIndex].linescore.errs}}</td>
            <td>{{stats.bsgame.team[teamIndex].linescore.lob}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="mt-3" v-for="(i, teamIndex) in 2" :key="teamIndex">
      <h6 style="text-align:left;"><strong>{{stats.bsgame.team[teamIndex].name}}</strong> <small>{{stats.bsgame.team[teamIndex].record}}</small></h6>
      <div class="table-responsive">
        <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th nowrap style="text-align:left;" title="Athlete">Athlete</th>
              <th title="At Bats">AB</th>
              <th title="Runs">Runs</th>
              <th title="Hits">Hits</th>
              <th title="Doubles">2B</th>
              <th title="Triples">3B</th>
              <th title="Home Runs">HR</th>
              <th title="Runs Batted In">RBI</th>
              <th title="Base On Balls">BB</th>
              <th title="Strike Outs">SO</th>
              <th title="Stolen Bases">SB</th>
              <th title="Caught Stealing">CS</th>
              <th title="Sacrifice sf">SF</th>
              <th title="Sacrifice Hit">SH</th>
              <th title="Hit By Pinch">HBP</th>
              <th title="Average">AVG</th>
              <th title="Slugging Average">SLG</th>
              <th title="On Base Percentage">OBP</th>
              <th title="Put Outs">PO</th>
              <th title="Assists">A</th>
              <th title="Errors">E</th>
              <th title="Fielding Percentage">FLD</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in playersInGameFilter(teamIndex)" :key="player.uni">
              <td nowrap style="text-align:left;">
                {{player.name}}
                <span style="float:right; text-transform:uppercase;">{{player.pos}}</span>
              </td>
              <td v-if="player.hitting.ab">{{player.hitting.ab}}</td><td v-else>0</td>
              <td v-if="player.hitting.r">{{player.hitting.r}}</td><td v-else>0</td>
              <td v-if="player.hitting.h">{{player.hitting.h}}</td><td v-else>0</td>
              <td v-if="player.hitting.double">{{player.hitting.double}}</td><td v-else>0</td>
              <td v-if="player.hitting.triple">{{player.hitting.triple}}</td><td v-else>0</td>
              <td v-if="player.hitting.hr">{{player.hitting.hr}}</td><td v-else>0</td>
              <td v-if="player.hitting.rbi">{{player.hitting.rbi}}</td><td v-else>0</td>
              <td v-if="player.hitting.bb">{{player.hitting.bb}}</td><td v-else>0</td>
              <td v-if="player.hitting.so">{{player.hitting.so}}</td><td v-else>0</td>
              <td v-if="player.hitting.sb">{{player.hitting.sb}}</td><td v-else>0</td>
              <td v-if="player.hitting.cs">{{player.hitting.cs}}</td><td v-else>0</td>
              <td v-if="player.hitting.sf">{{player.hitting.sf}}</td><td v-else>0</td>
              <td v-if="player.hitting.sh">{{player.hitting.sh}}</td><td v-else>0</td>
              <td v-if="player.hitting.hbp">{{player.hitting.hbp}}</td><td v-else>0</td>
              <!-- AVG -->
              <td v-if="player.hitting.ab !== '0'">{{(player.hitting.h / player.hitting.ab).toFixed(3) }}</td>
              <td v-else>N/A</td>
              <!-- Slugging % (SLG) -->
              <td v-if="player.hitting.ab !== '0'">{{ (((player.hitting.h - 0) + (player.hitting.double - 0) + (player.hitting.triple - 0) * 2 + (player.hitting.hr - 0) * 3) / player.hitting.ab).toFixed(3) }}</td>
              <td v-else>N/A</td>
              <!-- On Base % (OBP) -->
              <td v-if="((player.hitting.ab - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0) + (player.hitting.sf - 0)) !== 0">{{(((player.hitting.h - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0)) / ((player.hitting.ab - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0) + (player.hitting.sf - 0))).toFixed(3)}}</td>
              <td v-else>N/A</td>
              <td>{{player.fielding.po}}</td>
              <td>{{player.fielding.a}}</td>
              <td>{{player.fielding.e}}</td>
              <td v-if="((player.fielding.po - 0) + (player.fielding.a - 0) + (player.fielding.e - 0)) !== 0">{{ (((player.fielding.po - 0) + (player.fielding.a - 0)) / ((player.fielding.po - 0) + (player.fielding.a - 0) + (player.fielding.e - 0))).toFixed(3) }}</td>
              <td v-else>0</td>
            </tr>
            <tr>
                <th style="text-align:left">Batting Totals</th>
                <th>{{battingTotals.hitting.ab}}</th>
                <th>{{battingTotals.hitting.r}}</th>
                <th>{{battingTotals.hitting.h}}</th>
                <th>{{battingTotals.hitting.double}}</th>
                <th>{{battingTotals.hitting.triple}}</th>
                <th>{{battingTotals.hitting.hr}}</th>
                <th>{{battingTotals.hitting.rbi}}</th>
                <th>{{battingTotals.hitting.bb}}</th>
                <th>{{battingTotals.hitting.so}}</th>
                <th>{{battingTotals.hitting.sb}}</th>
                <th>{{battingTotals.hitting.cs}}</th>
                <th>{{battingTotals.hitting.sf}}</th>
                <th>{{battingTotals.hitting.sh}}</th>
                <th>{{battingTotals.hitting.hbp}}</th>
                <th>{{battingTotals.hitting.avg}}</th>
                <th>{{battingTotals.hitting.slg}}</th>
                <th>{{battingTotals.hitting.obp}}</th>
                <th>{{battingTotals.fielding.po }}</th>
                <th>{{battingTotals.fielding.a }}</th>
                <th>{{battingTotals.fielding.e }}</th>
                <th>{{battingTotals.fielding.fld }}</th>
            </tr>
          </tbody>
          <thead>
            <tr>
              <th nowrap style="text-align:left;" title="Pitcher">Pitcher</th>
              <th title="Innings Pitch">IP</th>
              <th title="Hits Allowed">H</th>
              <th title="Runs Allowed">R</th>
              <th title="Errors">ER</th>
              <th title="Based On Balls Surrendered">BB</th>
              <th title="Strike Outs">SO</th>
              <th title="Doubles Surrendered">2B</th>
              <th title="Triples Surrendered">3B</th>
              <th title="Home Runs Surrendered">HR</th>
              <th title="Batters Faced">BF</th>
              <th title="At Bats">AB</th>
              <th title="Wild Pitches">WP</th>
              <th title="Hit By Pitch">HBP</th>
              <th title="Balk">BK</th>
              <th title="Sacrifice Flies Allowed">SFA</th>
              <th colspan="2" title="Sacrifice Hits Allowed">SHA</th>
              <th colspan="2" title="Average">AVG</th>
              <th colspan="2" title="Earned Run Average">ERA</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in pitchersInGameFilter(teamIndex)" :key="player.uni">
              <td nowrap style="text-align:left;">
                {{player.name}}
                <span style="float:right; text-transform:uppercase;">{{player.pos}}</span>
              </td>
              <td>{{player.pitching.ip}}</td>
              <td>{{player.pitching.h}}</td>
              <td>{{player.pitching.r}}</td>
              <td>{{player.pitching.er}}</td>
              <td>{{player.pitching.bb}}</td>
              <td>{{player.pitching.so}}</td>
              <td>{{player.pitching.double}}</td>
              <td>{{player.pitching.triple}}</td>
              <td>{{player.pitching.hr}}</td>
              <td>{{player.pitching.bf}}</td>
              <td>{{player.pitching.ab}}</td>
              <td>{{player.pitching.wp}}</td>
              <td>{{player.pitching.hbp}}</td>
              <td>{{player.pitching.bk}}</td>
              <td>{{player.pitching.sfa}}</td>
              <td colspan="2">{{player.pitching.sha}}</td>
              <td colspan="2">{{ (player.pitching.h / player.pitching.ab).toFixed(3) }}</td>
              <td colspan="2">{{ ((player.pitching.er * 9) / player.pitching.innings_pitched).toFixed(3) }}</td>
            </tr>
            <tr>
              <th style="text-align:left">Pitching Totals</th>
              <th>{{ pitchingTotals.ip }}</th>
              <th>{{ pitchingTotals.h }}</th>
              <th>{{ pitchingTotals.r }}</th>
              <th>{{ pitchingTotals.er }}</th>
              <th>{{ pitchingTotals.bb }}</th>
              <th>{{ pitchingTotals.so }}</th>
              <th>{{ pitchingTotals.double }}</th>
              <th>{{ pitchingTotals.triple }}</th>
              <th>{{ pitchingTotals.hr }}</th>
              <th>{{ pitchingTotals.bf }}</th>
              <th>{{ pitchingTotals.ab }}</th>
              <th>{{ pitchingTotals.wp }}</th>
              <th>{{ pitchingTotals.hbp }}</th>
              <th>{{ pitchingTotals.bk }}</th>
              <th>{{ pitchingTotals.sfa }}</th>
              <th colspan="2">{{ pitchingTotals.sha }}</th>
              <th colspan="2">{{ pitchingTotals.avg }}</th>
              <th colspan="2">{{ pitchingTotals.era }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BaseballScoreBox',
  props: ['selected', 'stats'],
  data () {
    return {
      game: [],
      battingTotals: [],
      pitchingTotals: []
    }
  },
  mounted () {
  },
  methods: {
    playersInGameFilter (teamIndex) {
      this.game = this.stats
      const players = this.game.bsgame.team[teamIndex].player.filter(player => (player.gp !== '0' && player.uni !== 'TM' && player.pos !== 'p'))
      for (var i = 0; i < players.length; i++) {
        if (!players[i].hitting.h) players[i].hitting.h = 0
        if (!players[i].hitting.double) players[i].hitting.double = 0
        if (!players[i].hitting.triple) players[i].hitting.triple = 0
        if (!players[i].hitting.hr) players[i].hitting.hr = 0
        if (!players[i].hitting.ab) players[i].hitting.ab = 0
        if (!players[i].hitting.bb) players[i].hitting.bb = 0
        if (!players[i].hitting.hbp) players[i].hitting.hbp = 0
        if (!players[i].hitting.sf) players[i].hitting.sf = 0
      }
      this.reorganizeBattingTotals(teamIndex)
      return players
    },
    pitchersInGameFilter (teamIndex) {
      this.game = this.stats
      const players = this.game.bsgame.team[teamIndex].player.filter(player => (player.pos === 'p') || ((player.pos.indexOf('p/') !== -1) && (player.pos.indexOf('dp/') === -1)) || ((player.pos.indexOf('/p') !== -1) && (player.pos.indexOf('/pr') === -1) && (player.pos.indexOf('/ph') === -1)))
      for (var i = 0; i < players.length; i++) {
        if (!players[i].pitching.ip) players[i].pitching.ip = 0
        if (!players[i].pitching.h) players[i].pitching.h = 0
        if (!players[i].pitching.r) players[i].pitching.r = 0
        if (!players[i].pitching.er) players[i].pitching.er = 0
        if (!players[i].pitching.bb) players[i].pitching.bb = 0
        if (!players[i].pitching.so) players[i].pitching.so = 0
        if (!players[i].pitching.double) players[i].pitching.double = 0
        if (!players[i].pitching.triple) players[i].pitching.triple = 0
        if (!players[i].pitching.hr) players[i].pitching.hr = 0
        if (!players[i].pitching.bf) players[i].pitching.bf = 0
        if (!players[i].pitching.ab) players[i].pitching.ab = 0
        if (!players[i].pitching.wp) players[i].pitching.wp = 0
        if (!players[i].pitching.hbp) players[i].pitching.hbp = 0
        if (!players[i].pitching.bk) players[i].pitching.bk = 0
        if (!players[i].pitching.sfa) players[i].pitching.sfa = 0
        if (!players[i].pitching.sha) players[i].pitching.sha = 0
        players[i].pitching.innings_pitched = this.getInnings_pitched(players[i].pitching.ip)
      }
      this.reorganizePitchingTotals(teamIndex)
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
      // console.log(this.game.bsgame.team[teamIndex].id)
      const totals = this.game.bsgame.team[teamIndex].totals
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
      const pitching = this.game.bsgame.team[teamIndex].totals.pitching
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
