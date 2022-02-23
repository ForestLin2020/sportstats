<template>
  <h2>Softball Box Score</h2>
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
              <th title="Athlete">Athlete</th>
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
              <th title="Sacrifice Fly">SF</th>
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
              <td nowrap style="text-align:left;">{{player.name}}</td>
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
              <td v-if="player.hitting.fly">{{player.hitting.fly}}</td><td v-else>0</td>
              <td v-if="player.hitting.sh">{{player.hitting.sh}}</td><td v-else>0</td>
              <td v-if="player.hitting.hbp">{{player.hitting.hbp}}</td><td v-else>0</td>
              <!-- AVG -->
              <td v-if="player.hitting.ab != 0">{{(player.hitting.h / player.hitting.ab ).toFixed(3) }}</td>
              <td v-if="player.hitting.ab == 0">N/A</td>
              <!-- Slugging % (SLG) -->
              <td v-if="player.hitting.ab != 0">{{((player.hitting.h - 0) + (player.hitting.double - 0) + (player.hitting.triple - 0) * 2 + (player.hitting.hr - 0) * 3) / player.hitting.ab}}</td>
              <td v-if="player.hitting.ab == 0">N/A</td>
              <!-- On Base % (OBP) -->
              <td v-if="((player.hitting.ab - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0) + (player.hitting.fly - 0)) !== 0">{{((player.hitting.h - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0) ) / ((player.hitting.ab - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0) + (player.hitting.fly - 0))}}</td>
              <td v-if="((player.hitting.ab - 0) + (player.hitting.bb - 0) + (player.hitting.hbp - 0) + (player.hitting.fly - 0)) == 0">N/A</td>
              <td>{{player.fielding.po}}</td>
              <td>{{player.fielding.a}}</td>
              <td>{{player.fielding.e}}</td>
              <td>{{ (((player.fielding.po - 0) + (player.fielding.a - 0)) / ((player.fielding.po - 0) + (player.fielding.a - 0) + (player.fielding.e - 0))) }}</td>
            </tr>
            <tr>
              <th>Totals</th>
              <!-- <th></th>
              <th>{{stats.bsgame.team[teamIndex].totals.shots.g}}</th>
              <th>{{stats.bsgame.team[teamIndex].totals.shots.a}}</th>
              <th>{{stats.bsgame.team[teamIndex].totals.shots.sh}}</th>
              <th>{{stats.bsgame.team[teamIndex].totals.shots.sog}}</th>
              <th>{{stats.bsgame.team[teamIndex].totals.penalty.fouls}}</th>
              <th></th> -->
            </tr>
          </tbody>
          <thead>
            <tr>
              <th title="Pitcher">Pitcher</th>
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
              <th title="Sacrifice Hits Allowed">SHA</th>
              <th title="Average">AVG</th>
              <th title="Earned Run Average">ERA</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in goalkeeperInGameFilter(teamIndex)" :key="player.uni">
              <!-- <td v-if="player.uni">{{player.uni}}</td><td v-else>0</td>
              <td colspan="4" nowrap style="text-align:left;">{{player.name}}</td>
              <td v-if="player.goalie && player.goalie.ga">{{player.goalie.ga}}</td><td v-else>0</td>
              <td v-if="player.goalie && player.goalie.saves">{{player.goalie.saves}}</td><td v-else>0</td>
              <td v-if="player.goalie && player.goalie.minutes">{{player.goalie.minutes}}</td><td v-else>0</td> -->
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SoftballScoreBox',
  props: ['selected', 'stats'],
  methods: {
    playersInGameFilter (teamIndex) {
      return this.stats.bsgame.team[teamIndex].player.filter(player => (player.gp !== '0' && player.uni !== 'TM'))
    },
    goalkeeperInGameFilter (teamIndex) {
      return this.stats.bsgame.team[teamIndex].player.filter(player => (player.pos === 'gk' || player.goalie))
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
