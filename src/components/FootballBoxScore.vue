<template>
  <h2>Football Box Score</h2>
  <div class="game-description" nowrap>
    <!-- 2017 Concordia Irvine -->
    <h6 style="text-align:left;"><strong v-if="stats.fbgame.venue.location">Location:</strong> {{ stats.fbgame.venue.location }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.fbgame.venue.time">Time:</strong> {{ stats.fbgame.venue.time }} <strong>Date:</strong> {{ stats.fbgame.venue.date }} </h6>
    <h6 style="text-align:left;"><strong v-if="stats.fbgame.venue.attend">Attendance:</strong> {{ stats.fbgame.venue.attend }} </h6>
  </div>

  <h4>Line Score</h4>
  <div class="table-responsive">
    <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
      <thead>
        <tr>
          <th>Team</th>
          <th>1st</th>
          <th>2nd</th>
          <th>3rd</th>
          <th>4th</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[4]">OT</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[5]">2nd OT</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[6]">3rd OT</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[7]">4th OT</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[8]">5th OT</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[9]">6th OT</th>
          <th v-if="stats.fbgame.team[0].linescore.lineprd[10]">7th OT</th>
          <th>Final</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(i, teamIndex) in 2" :key="teamIndex">
          <td>{{stats.fbgame.team[teamIndex].name}}</td>
          <td>{{stats.fbgame.team[teamIndex].linescore.lineprd[0].score}}</td>
          <td>{{stats.fbgame.team[teamIndex].linescore.lineprd[1].score}}</td>
          <td>{{stats.fbgame.team[teamIndex].linescore.lineprd[2].score}}</td>
          <td>{{stats.fbgame.team[teamIndex].linescore.lineprd[3].score}}</td>
          <td v-if="stats.fbgame.team[teamIndex].linescore.lineprd[4]">{{stats.fbgame.team[teamIndex].linescore.lineprd[4].score}}</td>
          <td v-if="stats.fbgame.team[teamIndex].linescore.lineprd[5]">{{stats.fbgame.team[teamIndex].linescore.lineprd[5].score}}</td>
          <td v-if="stats.fbgame.team[teamIndex].linescore.lineprd[6]">{{stats.fbgame.team[teamIndex].linescore.lineprd[6].score}}</td>
          <td v-if="stats.fbgame.team[teamIndex].linescore.lineprd[7]">{{stats.fbgame.team[teamIndex].linescore.lineprd[7].score}}</td>
          <td>{{stats.fbgame.team[teamIndex].linescore.score}}</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div v-if="stats.fbgame.scores">
    <h4>Scoring Summary</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead v-if="stats.fbgame.scores.score && stats.fbgame.scores.score.length">
          <tr>
            <th>Team</th>
            <th>Type</th>
            <th>Time</th>
            <th></th>
            <th>{{stats.fbgame.venue.visid}}</th>
            <th>{{stats.fbgame.venue.homeid}}</th>
          </tr>
        </thead>
        <tbody v-if="stats.fbgame.scores.score && stats.fbgame.scores.score.length">
          <tr v-for="(ss, index) in stats.fbgame.scores.score" :key="index">
            <td v-if="ss.team">{{ss.team}}</td>
            <td v-if="ss.team">{{ss.type}}</td>
            <td v-if="ss.team">{{ss.clock}} {{ss.qtr}}<span v-if="ss.qtr">Q</span></td>
            <td style="text-align:left;" v-if="ss.team">{{ss.scorer}} {{ss.yds}}
              <span v-if="ss.yds">Yd</span>
              <span v-if="ss.how">{{ss.how}}</span>
              <span v-if="ss.passer">by {{ss.passer}}</span>
              <span v-if="ss.type == 'FG'">FIELD GOAL</span>
              <span v-if="ss.patby || ss.patype || ss.patres">({{ss.patby}} {{ss.patype}} {{ss.patres}})</span>
            </td>
            <td v-if="ss.team">{{ss.vscore}}</td>
            <td v-if="ss.team">{{ss.hscore}}</td>
            <td colspan="6" v-if="ss.text && !ss.patby && !ss.patype && !ss.patres && !ss.how&& !ss.passer && !ss.type && !ss.team && !ss.clock && !ss.qtr && !ss.scorer && !ss.yds">{{ss.text}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div>
    <h4>Team Stats</h4>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th></th>
            <th>{{stats.fbgame.venue.visid}}</th>
            <th>{{stats.fbgame.venue.homeid}}</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1st Downs</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.firstdowns && stats.fbgame.team[teamIndex].totals.firstdowns.no"
              >{{stats.fbgame.team[teamIndex].totals.firstdowns.no}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Rushing</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.firstdowns && stats.fbgame.team[teamIndex].totals.firstdowns.rush"
              >{{stats.fbgame.team[teamIndex].totals.firstdowns.rush}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Passing</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.firstdowns && stats.fbgame.team[teamIndex].totals.firstdowns.pass"
              >{{stats.fbgame.team[teamIndex].totals.firstdowns.pass}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Penalty</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.firstdowns && stats.fbgame.team[teamIndex].totals.firstdowns.penalty"
              >{{stats.fbgame.team[teamIndex].totals.firstdowns.penalty}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Rushing</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.rush && stats.fbgame.team[teamIndex].totals.rush.yds"
              >{{stats.fbgame.team[teamIndex].totals.rush.yds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Attempt</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.rush && stats.fbgame.team[teamIndex].totals.rush.att"
              >{{stats.fbgame.team[teamIndex].totals.rush.att}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Gain</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.rush && stats.fbgame.team[teamIndex].totals.rush.gain"
              >{{stats.fbgame.team[teamIndex].totals.rush.gain}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Loss</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.rush && stats.fbgame.team[teamIndex].totals.rush.loss"
              >{{stats.fbgame.team[teamIndex].totals.rush.loss}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>TDs Rushing</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.rush && stats.fbgame.team[teamIndex].totals.rush.td"
              >{{stats.fbgame.team[teamIndex].totals.rush.td}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Passing</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.pass && stats.fbgame.team[teamIndex].totals.pass.yds"
              >{{stats.fbgame.team[teamIndex].totals.pass.yds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Attempts</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.pass && stats.fbgame.team[teamIndex].totals.pass.att"
              >{{stats.fbgame.team[teamIndex].totals.pass.att}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Completions</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.pass && stats.fbgame.team[teamIndex].totals.pass.comp"
              >{{stats.fbgame.team[teamIndex].totals.pass.comp}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Interceptions</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.pass && stats.fbgame.team[teamIndex].totals.pass.int"
              >{{stats.fbgame.team[teamIndex].totals.pass.int}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>TDs Passing</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.pass && stats.fbgame.team[teamIndex].totals.pass.td"
              >{{stats.fbgame.team[teamIndex].totals.pass.td}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Total Offensive Plays</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.totoff_plays"
              >{{stats.fbgame.team[teamIndex].totals.totoff_plays}}</span>
              <span v-else>N/A</span>
            </td>
          </tr>
          <tr>
            <td>Total Net Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.totoff_yards"
              >{{stats.fbgame.team[teamIndex].totals.totoff_yards}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Average Gain per Play</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.totoff_avg"
              >{{stats.fbgame.team[teamIndex].totals.totoff_avg}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Fumbles - Lost</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.fumbles && stats.fbgame.team[teamIndex].totals.fumbles.no && stats.fbgame.team[teamIndex].totals.fumbles.lost"
              >{{stats.fbgame.team[teamIndex].totals.fumbles.no}} - {{stats.fbgame.team[teamIndex].totals.fumbles.lost}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Penalties - Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.penalties && stats.fbgame.team[teamIndex].totals.penalties.no && stats.fbgame.team[teamIndex].totals.penalties.yds"
              >{{stats.fbgame.team[teamIndex].totals.penalties.no}} - {{stats.fbgame.team[teamIndex].totals.penalties.yds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Interceptions - Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.defense && stats.fbgame.team[teamIndex].totals.defense.int && stats.fbgame.team[teamIndex].totals.defense.intyds"
              >{{stats.fbgame.team[teamIndex].totals.defense.int}} - {{stats.fbgame.team[teamIndex].totals.defense.intyds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Punts - Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.punt && stats.fbgame.team[teamIndex].totals.punt.no && stats.fbgame.team[teamIndex].totals.punt.yds"
              >{{stats.fbgame.team[teamIndex].totals.punt.no}} - {{stats.fbgame.team[teamIndex].totals.punt.yds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Average Per Punt</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.punt && stats.fbgame.team[teamIndex].totals.punt.avg"
              >{{stats.fbgame.team[teamIndex].totals.punt.avg}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Punt Returns - Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.pr && stats.fbgame.team[teamIndex].totals.pr.no && stats.fbgame.team[teamIndex].totals.pr.yds"
              >{{stats.fbgame.team[teamIndex].totals.pr.no}} - {{stats.fbgame.team[teamIndex].totals.pr.yds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Kickoff Returns - Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.kr && stats.fbgame.team[teamIndex].totals.kr.no && stats.fbgame.team[teamIndex].totals.kr.yds"
              >{{stats.fbgame.team[teamIndex].totals.kr.no}} - {{stats.fbgame.team[teamIndex].totals.kr.yds}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Time of Possession</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.misc && stats.fbgame.team[teamIndex].totals.misc.top"
              >{{stats.fbgame.team[teamIndex].totals.misc.top}}</span>
              <span v-else>0</span>
            </td>
          </tr>
          <tr>
            <td>Third Down Conversions</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.conversions && stats.fbgame.team[teamIndex].totals.conversions.thirdconv && stats.fbgame.team[teamIndex].totals.conversions.thirdatt"
              >{{stats.fbgame.team[teamIndex].totals.conversions.thirdconv}} of {{stats.fbgame.team[teamIndex].totals.conversions.thirdatt}}</span>
              <span v-else>0</span>
            </td>
          </tr>

          <tr>
            <td>Fourth Down Conversions</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.conversions && stats.fbgame.team[teamIndex].totals.conversions.fourthconv &&stats.fbgame.team[teamIndex].totals.conversions.fourthatt"
              >{{stats.fbgame.team[teamIndex].totals.conversions.fourthconv}} of {{stats.fbgame.team[teamIndex].totals.conversions.fourthatt}}</span>
              <span v-else>0</span>
            </td>
          </tr>

          <tr>
            <td>Sacks - Yards</td>
            <td v-for="(i, teamIndex) in 2" :key="teamIndex">
              <span
                v-if="stats.fbgame.team[teamIndex].totals.defense && stats.fbgame.team[teamIndex].totals.defense.sacks && stats.fbgame.team[teamIndex].totals.defense.sackyds"
              >{{stats.fbgame.team[teamIndex].totals.defense.sacks}} - {{stats.fbgame.team[teamIndex].totals.defense.sackyds}}</span>
              <span v-else>0</span>
            </td>
          </tr>

        </tbody>
      </table>
    </div>
  </div>

  <div v-for="(i, teamIndex) in 2" :key="teamIndex">
    <h6 style="text-align:left;"><strong>{{stats.fbgame.team[teamIndex].name}} Passing</strong></h6>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th style="text-align:left;">Athlete</th>
            <th>Comp</th>
            <th>Att</th>
            <th>Int</th>
            <th>Yds</th>
            <th>Y/A</th>
            <th>Lng</th>
            <th>TD</th>
            <th>Sk</th>
            <th>Effic</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="player in QBInGameFilter(teamIndex)" :key="player.uni">
            <td nowrap style="text-align:left;">{{player.name}}</td>
            <td>{{player.pass.comp}}</td>
            <td>{{player.pass.att}}</td>
            <td>{{player.pass.int}}</td>
            <td>{{player.pass.yds}}</td>
            <td>{{(player.pass.yds / player.pass.att).toFixed(2)}}</td>
            <td>{{player.pass.long}}</td>
            <td>{{player.pass.td}}</td>
            <td>{{player.pass.sacks}}</td>
            <!-- effic: ((8.4 * yds + 330 * td + 100 * comp - 200 * int) / att).toFixed(2) -->
            <td>{{((8.4 * player.pass.yds + 330 * player.pass.td + 100 * player.pass.comp - 200 * player.pass.int) / player.pass.att).toFixed(2)}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div v-for="(i, teamIndex) in 2" :key="teamIndex">
    <h6 style="text-align:left;"><strong>{{stats.fbgame.team[teamIndex].name}} Rushing</strong></h6>
    <div class="table-responsive">
      <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
        <thead>
          <tr>
            <th style="text-align:left;">Athlete</th>
            <th>No</th>
            <th>Gain</th>
            <th>Loss</th>
            <th>Net</th>
            <th>Avg</th>
            <th>Lng</th>
            <th>TD</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="player in RBInGameFilter(teamIndex)" :key="player.uni">
            <td nowrap style="text-align:left;">{{player.name}}</td>
            <td>{{player.rush.att}}</td>
            <td>{{player.rush.gain}}</td>
            <td>{{player.rush.loss}}</td>
            <td>{{(player.rush.gain - player.rush.loss)}}</td>
            <td>{{ ((player.rush.gain - player.rush.loss) / player.rush.att).toFixed(1) }}</td>
            <td>{{player.rush.long}}</td>
            <td>{{player.rush.td}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FootballScoreBox',
  props: ['selected', 'stats'],
  methods: {
    playersInGameFilter (teamIndex) {
      return this.stats.fbgame.team[teamIndex].player.filter(player => (player.gp !== '0' && player.uni !== 'TM'))
    },
    QBInGameFilter (teamIndex) {
      return this.stats.fbgame.team[teamIndex].player.filter(player => player.pass)
    },
    RBInGameFilter (teamIndex) {
      return this.stats.fbgame.team[teamIndex].player.filter(player => (player.rush && player.name !== 'TEAM')).sort(this.netSort)
    },
    netSort (a, b) {
      return (b.rush.gain - b.rush.loss) - (a.rush.gain - a.rush.loss)
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
