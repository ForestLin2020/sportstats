<template>
  <h2>Volleyball Box Score</h2>
  <div class="game-description" nowrap>
    <!-- 2017 Concordia Irvine -->
    <h6 style="text-align:left;"><strong v-if="stats.vbgame.venue.location">Location:</strong> {{ stats.vbgame.venue.location }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.vbgame.venue.time">Time:</strong> {{ stats.vbgame.venue.time }} <strong>Date:</strong> {{ stats.vbgame.venue.date }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.vbgame.venue.attend">Attendance:</strong> {{ stats.vbgame.venue.attend }} </h6>
  </div>

  <div class="table-responsive">
    <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
      <thead>
        <tr>
          <th nowrap>Set Scores</th>
          <th>1</th>
          <th>2</th>
          <th>3</th>
          <th v-if="stats.vbgame.breakdowns[0].breakdown[3]">4</th>
          <th v-if="stats.vbgame.breakdowns[0].breakdown[4]">5</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td >{{stats.vbgame.team[0].id}} </td>
          <td >{{stats.vbgame.team[0].linescore.linegame[0].points}}</td>
          <td >{{stats.vbgame.team[0].linescore.linegame[1].points}}</td>
          <td >{{stats.vbgame.team[0].linescore.linegame[2].points}}</td>
          <td v-if="stats.vbgame.team[0].linescore.linegame[3]">{{stats.vbgame.team[0].linescore.linegame[3].points}}</td>
          <td v-if="stats.vbgame.team[0].linescore.linegame[4]">{{stats.vbgame.team[0].linescore.linegame[4].points}}</td>
        </tr>
        <tr>
          <td >{{stats.vbgame.team[1].id}} </td>
          <td >{{stats.vbgame.team[1].linescore.linegame[0].points}}</td>
          <td >{{stats.vbgame.team[1].linescore.linegame[1].points}}</td>
          <td >{{stats.vbgame.team[1].linescore.linegame[2].points}}</td>
          <td v-if="stats.vbgame.team[1].linescore.linegame[3]">{{stats.vbgame.team[1].linescore.linegame[3].points}}</td>
          <td v-if="stats.vbgame.team[1].linescore.linegame[4]">{{stats.vbgame.team[1].linescore.linegame[4].points}}</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div v-for="(i, teamIndex) in 2" :key="teamIndex">
    <h6 style="text-align:left;"><strong>{{stats.vbgame.team[teamIndex].name}}</strong> <small>{{stats.vbgame.team[teamIndex].record}}</small></h6>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th>NO.</th>
            <th style="text-align:left;">Athlete</th>
            <th title="Sets">S</th>
            <th title="Kills">K</th>
            <th title="Errors">E</th>
            <th title="Total Attemps">TA</th>
            <th title="Hitting Pecentage">Pct</th>
            <th title="Assists">Ast</th>
            <th title="Service Aces">SA</th>
            <th title="Service Errors">SE</th>
            <th title="Reception Errors">RE</th>
            <th title="Digs">Digs</th>
            <th title="Block Solos">BS</th>
            <th title="Block Assisted">BA</th>
            <th title="Block Errors">BE</th>
            <th title="Ball Handling Errors">BHE</th>
            <th title="Points">Pts</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="player in playersInGameFilter(teamIndex)" :key="player.uni">
            <td v-if="player.uni">{{player.uni}}</td><td v-else>0</td>
            <td nowrap style="text-align:left;">{{player.name}}</td>
            <td v-if="player.gp">{{player.gp}}</td><td v-else>0</td>
            <td v-if="player.attack && player.attack.k">{{player.attack.k}}</td><td v-else>0</td>
            <td v-if="player.attack && player.attack.e">{{player.attack.e}}</td><td v-else>0</td>
            <td v-if="player.attack && player.attack.ta">{{player.attack.ta}}</td><td v-else>0</td>
            <td v-if="player.attack && player.attack.pct">{{player.attack.pct}}</td><td v-else>0</td>
            <td v-if="player.set && player.set.a">{{player.set.a}}</td><td v-else>0</td>
            <td v-if="player.serve && player.serve.sa">{{player.serve.sa}}</td><td v-else>0</td>
            <td v-if="player.serve && player.serve.se">{{player.serve.se}}</td><td v-else>0</td>
            <td v-if="player.defense && player.defense.re">{{player.defense.re}}</td><td v-else>0</td>
            <td v-if="player.defense && player.defense.dig">{{player.defense.dig}}</td><td v-else>0</td>
            <td v-if="player.block && player.block.bs">{{player.block.bs}}</td><td v-else>0</td>
            <td v-if="player.block && player.block.ba">{{player.block.ba}}</td><td v-else>0</td>
            <td v-if="player.block && player.block.be">{{player.block.be}}</td><td v-else>0</td>
            <td v-if="player.misc && player.misc.bhe">{{player.misc.bhe}}</td><td v-else>0</td>
            <td v-if="player.misc && player.misc.pts">{{player.misc.pts}}</td><td v-else>0</td>
          </tr>
          <tr>
            <th></th>
            <th style="text-align:left">Totals</th>
            <th></th>
            <th>{{stats.vbgame.team[teamIndex].totals.attack.k}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.attack.e}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.attack.ta}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.attack.pct}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.set.a}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.serve.sa}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.serve.se}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.defense.re}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.defense.dig}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.block.bs}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.block.ba}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.block.be}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.misc.bhe}}</th>
            <th>{{stats.vbgame.team[teamIndex].totals.misc.pts}}</th>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <div class="mt-3">
    <h4>Team Attack Stats By Set</h4>
    <div v-for="(i, teamIndex) in 2" :key="teamIndex">
      <h6 style="text-align:left;"><strong>{{stats.vbgame.team[teamIndex].name}}</strong> <small>{{stats.vbgame.team[teamIndex].linescore.score}} sets {{stats.vbgame.breakdowns[teamIndex].block}} blocks</small> </h6>
      <div class="table-responsive mt-3">
        <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th style="text-align:left">Set</th>
              <th>K</th>
              <th>E</th>
              <th>TA</th>
              <th>Pct</th>
              <th>Pts</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(set ,index) in stats.vbgame.team[teamIndex].attackbygame.attackgame" :key="index">
              <td style="text-align:left">{{set.game}}</td>
              <td>{{set.k}}</td>
              <td>{{set.e}}</td>
              <td>{{set.ta}}</td>
              <td>{{set.pct}}</td>
              <td v-if="set.game == '1'">{{stats.vbgame.team[teamIndex].linescore.linegame[0].points}}</td>
              <td v-if="set.game == '2'">{{stats.vbgame.team[teamIndex].linescore.linegame[1].points}}</td>
              <td v-if="set.game == '3'">{{stats.vbgame.team[teamIndex].linescore.linegame[2].points}}</td>
              <td v-if="set.game == '4'">{{stats.vbgame.team[teamIndex].linescore.linegame[3].points}}</td>
              <td v-if="set.game == '5'">{{stats.vbgame.team[teamIndex].linescore.linegame[4].points}}</td>
              <td v-if="set.game == '6'">{{stats.vbgame.team[teamIndex].linescore.linegame[5].points}}</td>
            </tr>
            <tr>
              <th style="text-align:left">Totals</th>
              <th>{{stats.vbgame.team[teamIndex].totals.attack.k}}</th>
              <th>{{stats.vbgame.team[teamIndex].totals.attack.e}}</th>
              <th>{{stats.vbgame.team[teamIndex].totals.attack.ta}}</th>
              <th>{{stats.vbgame.team[teamIndex].totals.attack.pct}}</th>
              <th>{{stats.vbgame.breakdowns[teamIndex].pts}}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VolleyballScoreBox',
  props: ['selected', 'stats'],
  data () {
    return {
      totalsByYear: [],
      careerTotals: []
    }
  },
  methods: {
    playersInGameFilter (teamIndex) {
      return this.stats.vbgame.team[teamIndex].player.filter(player => (player.gp !== '0' && player.uni !== 'TM')).sort(this.dateSort)
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
