<template>
  <h2>Soccer Box Score</h2>
  <div class="game-description" nowrap>
    <!-- 2017 Concordia Irvine -->
    <h6 style="text-align:left;"><strong v-if="stats.sogame.venue.location">Location:</strong> {{ stats.sogame.venue.location }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.sogame.venue.time">Time:</strong> {{ stats.sogame.venue.time }} <strong>Date:</strong> {{ stats.sogame.venue.date }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.sogame.venue.attend">Attendance:</strong> {{ stats.sogame.venue.attend }} </h6>
  </div>

  <div>
    <h4>Goals By Period</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
              <th>University</th>
              <th v-for="(len, index) in stats.sogame.plays.period.length" :key="index">{{len}}</th>
              <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td> {{stats.sogame.team[teamIndex].id}}</td>
            <td v-for="(score, index) in stats.sogame.team[teamIndex].linescore.lineprd" :key="index">{{score.score}}</td>
            <td>{{stats.sogame.team[teamIndex].linescore.score}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="mt-3" v-for="(i, teamIndex) in 2" :key="teamIndex">
      <h6 style="text-align:left;"><strong>{{stats.sogame.team[teamIndex].name}}</strong> <small>{{stats.sogame.team[teamIndex].record}}</small></h6>
      <div class="table-responsive">
        <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th>NO.</th>
              <th style="text-align:left;">Athlete</th>
              <th title="Goals">G</th>
              <th title="Assists">A</th>
              <th title="Shots">SH</th>
              <th title="Shots On Goal">SOG</th>
              <th title="Fouls">Fouls</th>
              <th title="Minutes">MIN</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in playersInGameFilter(teamIndex)" :key="player.uni">
              <td v-if="player.uni">{{player.uni}}</td><td v-else>0</td>
              <td nowrap style="text-align:left;">{{player.name}}</td>
              <td v-if="player.shots && player.shots.g">{{player.shots.g}}</td><td v-else>0</td>
              <td v-if="player.shots && player.shots.a">{{player.shots.a}}</td><td v-else>0</td>
              <td v-if="player.shots && player.shots.sh">{{player.shots.sh}}</td><td v-else>0</td>
              <td v-if="player.shots && player.shots.sog">{{player.shots.sog}}</td><td v-else>0</td>
              <td v-if="player.penalty && player.penalty.fouls">{{player.penalty.fouls}}</td><td v-else>0</td>
              <td v-if="player.misc && player.misc.minutes">{{player.misc.minutes}}</td><td v-else>0</td>
            </tr>
            <tr>
              <th>Totals</th>
              <th></th>
              <th>{{stats.sogame.team[teamIndex].totals.shots.g}}</th>
              <th>{{stats.sogame.team[teamIndex].totals.shots.a}}</th>
              <th>{{stats.sogame.team[teamIndex].totals.shots.sh}}</th>
              <th>{{stats.sogame.team[teamIndex].totals.shots.sog}}</th>
              <th>{{stats.sogame.team[teamIndex].totals.penalty.fouls}}</th>
              <th></th>
            </tr>
          </tbody>
          <thead>
            <tr>
              <th>NO.</th>
              <th colspan="4" nowrap style="text-align:left;">Goalie</th>
              <th title="Goals Allowed">GA</th>
              <th>Saves</th>
              <th>Minutes</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="player in goalkeeperInGameFilter(teamIndex)" :key="player.uni">
              <td v-if="player.uni">{{player.uni}}</td><td v-else>0</td>
              <td colspan="4" nowrap style="text-align:left;">{{player.name}}</td>
              <td v-if="player.goalie && player.goalie.ga">{{player.goalie.ga}}</td><td v-else>0</td>
              <td v-if="player.goalie && player.goalie.saves">{{player.goalie.saves}}</td><td v-else>0</td>
              <td v-if="player.goalie && player.goalie.minutes">{{player.goalie.minutes}}</td><td v-else>0</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <div>
    <h4>Saves By Period</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
              <th>University</th>
              <th v-for="(len, index) in stats.sogame.plays.period.length" :key="index">{{len}}</th>
              <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td> {{stats.sogame.team[teamIndex].id}}</td>
            <td v-for="(saves, index) in stats.sogame.team[teamIndex].linescore.lineprd" :key="index">{{saves.saves}}</td>
            <td>{{stats.sogame.team[teamIndex].totals.goalie.saves}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div>
    <h4>Shots By Period</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
              <th>University</th>
              <th v-for="(len, index) in stats.sogame.plays.period.length" :key="index">{{len}}</th>
              <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td> {{stats.sogame.team[teamIndex].id}}</td>
            <td v-for="(shots, index) in stats.sogame.team[teamIndex].linescore.lineprd" :key="index">{{shots.shots}}</td>
            <td>{{stats.sogame.team[teamIndex].totals.shots.sh}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div>
    <h4>Corner Kicks By Period</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
              <th>University</th>
              <th v-for="(len, index) in stats.sogame.plays.period.length" :key="index">{{len}}</th>
              <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td> {{stats.sogame.team[teamIndex].id}}</td>
            <td v-for="(corners, index) in stats.sogame.team[teamIndex].linescore.lineprd" :key="index">{{corners.corners}}</td>
            <td>{{(stats.sogame.team[teamIndex].linescore.lineprd[0].corners * 1) + (stats.sogame.team[teamIndex].linescore.lineprd[1].corners * 1) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div>
    <h4>Fouls By Period</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
              <th>University</th>
              <th v-for="(len, index) in stats.sogame.plays.period.length" :key="index">{{len}}</th>
              <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
            <td> {{stats.sogame.team[teamIndex].id}}</td>
            <td v-for="(fouls, index) in stats.sogame.team[teamIndex].linescore.lineprd" :key="index">{{fouls.fouls}}</td>
            <td>{{ stats.sogame.team[teamIndex].totals.penalty.fouls }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div v-if="stats.sogame.scores">
    <h4>Scoring Summary</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <!-- scores come with an array - start -->
        <thead v-if="stats.sogame.scores.score && stats.sogame.scores.score.length">
          <tr>
            <th>No</th>
            <th>Time</th>
            <th style="text-align:left;">Goal</th>
            <th>Description</th>
          </tr>
        </thead>
        <tbody v-if="stats.sogame.scores.score && stats.sogame.scores.score.length">
          <tr v-for="(score, teamIndex) in stats.sogame.scores.score" :key="teamIndex">
            <td>{{score.number}}</td>
            <td>{{score.time}}</td>
            <td style="text-align:left;">Goal by {{score.name}} ({{score.id}})
                <span v-if="score.assist1">, Assist By {{score.assist1}}</span>
            </td>
            <td>{{score.desc}}</td>
          </tr>
        </tbody>
        <!-- scores come with an array - end -->
        <!-- scores come with one object - start -->
        <thead v-if="stats.sogame.scores.score && !stats.sogame.scores.score.length">
          <tr>
            <th>No</th>
            <th>Time</th>
            <th style="text-align:left;">Goal</th>
          </tr>
        </thead>
        <tbody v-if="stats.sogame.scores.score && !stats.sogame.scores.score.length">
          <tr>
            <td>{{stats.sogame.scores.score.number}}</td>
            <td>{{stats.sogame.scores.score.time}}</td>
            <td colspan="2" style="text-align:left;">Goal by {{stats.sogame.scores.score.name}} ({{stats.sogame.scores.score.id}})
                <span v-if="stats.sogame.scores.score.assist1">, Assist By {{stats.sogame.scores.score.assist1}}</span>
            </td>
          </tr>
        </tbody>
        <!-- scores come with one object - end -->
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SoccerScoreBox',
  props: ['selected', 'stats'],
  methods: {
    playersInGameFilter (teamIndex) {
      return this.stats.sogame.team[teamIndex].player.filter(player => (player.gp !== '0' && player.uni !== 'TM'))
    },
    goalkeeperInGameFilter (teamIndex) {
      return this.stats.sogame.team[teamIndex].player.filter(player => (player.pos === 'gk' || player.goalie))
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
