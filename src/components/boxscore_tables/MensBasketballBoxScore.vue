<template>
  <h2>Men's Basketball Box Score</h2>
  <div class="game-description" style="text-align:left;" nowrap>
    <h6><strong v-if="stats.bbgame.venue.location">Location:</strong> {{ stats.bbgame.venue.location }} </h6>
    <h6><strong v-if="stats.bbgame.venue.time">Time:</strong> {{ stats.bbgame.venue.time }}</h6>
    <h6><strong v-if="stats.bbgame.venue.date">Date:</strong> {{ stats.bbgame.venue.date }} </h6>
    <h6><strong v-if="stats.bbgame.venue.attend">Attendance:</strong> {{ stats.bbgame.venue.attend }} </h6>
  </div>

  <div>
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
            <tr v-for="player in startersIvameFilter(teamIndex)" :key="player.uni">
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
              <th style="text-align:left;">Bench</th>
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
            <tr v-for="player in benchsIvameFilter(teamIndex)" :key="player.uni">
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
            <tr>
              <th></th>
              <th style="text-align: left;">TOTALS</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.min}}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.fgm}}-{{stats.bbgame.team[teamIndex].totals.stats.fga}} </th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.fgm3}}-{{stats.bbgame.team[teamIndex].totals.stats.fga3}}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.ftm}}-{{stats.bbgame.team[teamIndex].totals.stats.fta}}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.oreb }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.dreb }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.treb }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.ast }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.blk }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.stl }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.pf }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.to }}</th>
              <th>{{stats.bbgame.team[teamIndex].totals.stats.tp }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <div class="mt-3">
    <h4>Team Stats By Period</h4>
    <div v-for="(i, teamIndex) in 2" :key="teamIndex">
      <h6 style="text-align:left;"><strong>{{stats.bbgame.team[teamIndex].name}}</strong></h6>
      <div class="table-responsive">
        <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
          <thead>
            <tr>
              <th>Period</th>
              <th>Score</th>
              <th>FG-FGA</th>
              <th>FG%</th>
              <th>3p-3pa</th>
              <th>3p%</th>
              <th>FT-FTA</th>
              <th>FT%</th>
              <th>In Paint</th>
              <th>Off T/0</th>
              <th>2nd Chance</th>
              <th>Fast Break</th>
              <th>Bench Pts</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>1st Half</td>
              <td>{{stats.bbgame.team[teamIndex].linescore.lineprd[0].score}}</td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.team[teamIndex].totals.statsbyprd[0].fgm}}-{{stats.bbgame.team[teamIndex].totals.statsbyprd[0].fga}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.plays.period[0].summary[teamIndex].fgm}}-{{stats.bbgame.plays.period[0].summary[teamIndex].fga}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.team[teamIndex].totals.statsbyprd[0].fgm / stats.bbgame.team[teamIndex].totals.statsbyprd[0].fga).toFixed(3)}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.plays.period[0].summary[teamIndex].fgm / stats.bbgame.plays.period[0].summary[teamIndex].fga).toFixed(3)}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.team[teamIndex].totals.statsbyprd[0].fgm3}}-{{stats.bbgame.team[teamIndex].totals.statsbyprd[0].fga3}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.plays.period[0].summary[teamIndex].fgm3}}-{{stats.bbgame.plays.period[0].summary[teamIndex].fga3}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.team[teamIndex].totals.statsbyprd[0].fgm3 / stats.bbgame.team[teamIndex].totals.statsbyprd[0].fga3).toFixed(3)}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.plays.period[0].summary[teamIndex].fgm3 / stats.bbgame.plays.period[0].summary[teamIndex].fga3).toFixed(3)}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.team[teamIndex].totals.statsbyprd[0].ftm}}-{{stats.bbgame.team[teamIndex].totals.statsbyprd[0].fta}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.plays.period[0].summary[teamIndex].ftm}}-{{stats.bbgame.plays.period[0].summary[teamIndex].fta}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.team[teamIndex].totals.statsbyprd[0].ftm / stats.bbgame.team[teamIndex].totals.statsbyprd[0].fta).toFixed(3)}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.plays.period[0].summary[teamIndex].ftm / stats.bbgame.plays.period[0].summary[teamIndex].fta).toFixed(3) }}</span>
              </td>
              <td>{{stats.bbgame.plays.period[0].special[teamIndex].pts_paint}}</td>
              <td>{{stats.bbgame.plays.period[0].special[teamIndex].pts_to}}</td>
              <td>{{stats.bbgame.plays.period[0].special[teamIndex].pts_ch2}}</td>
              <td>{{stats.bbgame.plays.period[0].special[teamIndex].pts_fastb}}</td>
              <td>{{stats.bbgame.plays.period[0].special[teamIndex].pts_bench}}</td>
            </tr>
            <tr>
              <td>2nd Half</td>
              <td>{{stats.bbgame.team[teamIndex].linescore.lineprd[1].score}}</td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.team[teamIndex].totals.statsbyprd[1].fgm}}-{{stats.bbgame.team[teamIndex].totals.statsbyprd[1].fga}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.plays.period[1].summary[teamIndex].fgm}}-{{stats.bbgame.plays.period[1].summary[teamIndex].fga}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.team[teamIndex].totals.statsbyprd[1].fgm / stats.bbgame.team[teamIndex].totals.statsbyprd[1].fga).toFixed(3) }}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.plays.period[1].summary[teamIndex].fgm / stats.bbgame.plays.period[1].summary[teamIndex].fga).toFixed(3)}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.team[teamIndex].totals.statsbyprd[1].fgm3}}-{{stats.bbgame.team[teamIndex].totals.statsbyprd[1].fga3}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.plays.period[1].summary[teamIndex].fgm3}}-{{stats.bbgame.plays.period[1].summary[teamIndex].fga3}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.team[teamIndex].totals.statsbyprd[1].fgm3 / stats.bbgame.team[teamIndex].totals.statsbyprd[1].fga3).toFixed(3)}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.plays.period[1].summary[teamIndex].fgm3 / stats.bbgame.plays.period[1].summary[teamIndex].fga3).toFixed(3)}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.team[teamIndex].totals.statsbyprd[1].ftm}}-{{stats.bbgame.team[teamIndex].totals.statsbyprd[1].fta}}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{stats.bbgame.plays.period[1].summary[teamIndex].ftm}}-{{stats.bbgame.plays.period[1].summary[teamIndex].fta}}</span>
              </td>
              <td>
                  <span v-if="stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.team[teamIndex].totals.statsbyprd[1].ftm / stats.bbgame.team[teamIndex].totals.statsbyprd[1].fta).toFixed(3) }}</span>
                  <span v-if="!stats.bbgame.team[teamIndex].totals.statsbyprd">{{(stats.bbgame.plays.period[1].summary[teamIndex].ftm / stats.bbgame.plays.period[1].summary[teamIndex].fta).toFixed(3) }}</span>
              </td>
              <td>{{stats.bbgame.plays.period[1].special[teamIndex].pts_paint }}</td>
              <td>{{stats.bbgame.plays.period[1].special[teamIndex].pts_to }}</td>
              <td>{{stats.bbgame.plays.period[1].special[teamIndex].pts_ch2 }}</td>
              <td>{{stats.bbgame.plays.period[1].special[teamIndex].pts_fastb }}</td>
              <td>{{stats.bbgame.plays.period[1].special[teamIndex].pts_bench }}</td>
            </tr>
            <tr>
              <th>Total</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.stats.tp }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.stats.fgm}}-{{stats.bbgame.team[teamIndex].totals.stats.fga }} </th>
              <th>{{ (stats.bbgame.team[teamIndex].totals.stats.fgm / stats.bbgame.team[teamIndex].totals.stats.fga).toFixed(3) }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.stats.fgm3}}-{{stats.bbgame.team[teamIndex].totals.stats.fga3}} </th>
              <th>{{ (stats.bbgame.team[teamIndex].totals.stats.fgm3 / stats.bbgame.team[teamIndex].totals.stats.fga3).toFixed(3) }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.stats.ftm}}-{{stats.bbgame.team[teamIndex].totals.stats.fta }}</th>
              <th>{{ (stats.bbgame.team[teamIndex].totals.stats.ftm / stats.bbgame.team[teamIndex].totals.stats.fta).toFixed(3) }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.special.pts_paint }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.special.pts_to }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.special.pts_ch2 }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.special.pts_fastb }}</th>
              <th>{{ stats.bbgame.team[teamIndex].totals.special.pts_bench }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <div class="mt-3">
    <div class="table-responsive-sm">
      <table class="table-sm table table-hover table-striped table-bordered">
        <thead>
          <tr>
            <th></th>
            <th>{{stats.bbgame.team[0].name}}</th>
            <th>{{stats.bbgame.team[1].name}}</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td nowrap style="text-align:left;">Largest Lead</td>
            <td v-if="stats.bbgame.team[0].totals.special.large_lead && stats.bbgame.team[0].totals.special.large_lead">{{stats.bbgame.team[0].totals.special.large_lead}} {{stats.bbgame.team[0].totals.special.large_lead_t}}</td>
            <td v-if="stats.bbgame.team[0].totals.special.large_lead == '0' || !stats.bbgame.team[0].totals.special.large_lead">None</td>
            <td v-if="stats.bbgame.team[1].totals.special.large_lead &&stats.bbgame.team[1].totals.special.large_lead != '0'">{{stats.bbgame.team[1].totals.special.large_lead}} {{stats.bbgame.team[1].totals.special.large_lead_t}}</td>
            <td v-if="stats.bbgame.team[1].totals.special.large_lead == '0' || !stats.bbgame.team[1].totals.special.large_lead">None</td>
          </tr>
          <tr>
            <td nowrap style="text-align:left;">Last FG</td>
            <td>{{lastFGTeam0}}</td>
            <td>{{lastFGTeam1}}</td>
          </tr>

          <tr>
            <td nowrap style="text-align:left;">Technical Fouls</td>
            <td>{{stats.bbgame.team[0].totals.stats.tf}}</td>
            <td>{{stats.bbgame.team[1].totals.stats.tf}}</td>
          </tr>
          <tr>
            <td nowrap style="text-align:left;">Deadball Rebounds</td>
            <td>{{stats.bbgame.team[0].totals.stats.deadball}}</td>
            <td>{{stats.bbgame.team[1].totals.stats.deadball}}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="table-responsive-sm">
      <table class="table-sm table table-hover table-striped table-bordered">
          <thead>
            <tr>
              <th>Score by periods</th>
              <th v-for="(period, index) in stats.bbgame.team[0].linescore.lineprd" :key="index">{{period.prd}}
                  <!-- <span v-if="(period.prd * 1) > (stats.bbgame.venue.rules.prds * 1)">
                  (OT)</th> -->
              </th>
              <th>Total</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td> {{stats.bbgame.team[0].name}}</td>
              <td v-for="(period, index) in stats.bbgame.team[0].linescore.lineprd" :key="index">{{ period.score }}</td>
              <td> {{stats.bbgame.team[0].linescore.score}}</td>
            </tr>
            <tr>
              <td>{{stats.bbgame.team[1].name}}</td>
              <td v-for="(period, index) in stats.bbgame.team[1].linescore.lineprd" :key="index">{{ period.score }}</td>
              <td> {{stats.bbgame.team[1].linescore.score}}</td>
            </tr>
          </tbody>
      </table>
    </div>

    <div class="table-responsive-sm">
      <table class="table-sm table  table-hover  table-striped table-bordered">
        <tbody>
          <tr>
            <th>Score Tied</th>
            <td>{{ stats.bbgame.team[0].totals.special.ties}} {{stats.bbgame.team[0].totals.special.ties_time }}</td>
          </tr>
          <tr>
            <th>Lead Chaves</th>
            <td>{{ (stats.bbgame.team[0].totals.special.leads * 1) + (stats.bbgame.team[1].totals.special.leads * 1) }} </td>
          </tr>
        </tbody>
      </table>
      </div>
  </div>
</template>

<script>
export default {
  name: 'BasketballScoreBox',
  props: ['selected', 'stats'],
  data () {
    return {
      game: [],
      lastFGTeam0: null,
      lastFGTeam1: null,
      TM: null
    }
  },
  mounted () {
    this.getLastFGMade()
    // this.reorganizePlayers()
  },
  methods: {
    reorganizePlayers () {
      console.log('Hi')
      for (var i; i < 2; i++) {
        console.log('Hi bsk mount')
        const TM = this.game.bbgame.team[i].player.filter(player => (player.uni === 'TM' || player.code === 'TM'))
        console.log('TM', TM)
        // const TMIndex = this.game.bbgame.team[teamIndex].player.findIndex(player => (player.uni === 'TM' || player.code === 'TM')) // find index in array
        // console.log('TMIndex', TMIndex)

        // this.game.bbgame.team[teamIndex].player.splice(TMIndex, 1)
        // this.game.bbgame.team[teamIndex].player.push(TM)
      }
    },
    startersIvameFilter (teamIndex) {
      this.game = this.stats
      const players = this.game.bbgame.team[teamIndex].player.filter(player => player.gs)
      // for (var i = 0; i < players.levth; i++) {
      //   if (!players[i].hittiv.h) players[i].hittiv.h = 0
      // }
      return players
    },
    benchsIvameFilter (teamIndex) {
      this.game = this.stats
      const players = this.game.bbgame.team[teamIndex].player.filter(player => (!player.gs && player.gp === '1' && (player.uni !== 'TM' || player.code !== 'TM')))
      // for (var i = 0; i < players.levth; i++) {
      //   if (!players[i].pitchiv.ip) players[i].pitchiv.ip = 0
      // }
      return players
    },
    getInnivs_pitched (ip) {
      if (ip.indexOf('.0') === -1) {
        var split = ip.split('.')
        split[1] = split[1] / 3
        var join = (split[0] - 0) + (split[1] - 0)
        return join
      } else {
        return ip
      }
    },
    getLastFGMade () {
      // find the last FG made
      for (var o = 0; o < this.stats.bbgame.plays.period.length; o++) {
        for (var x = 0; x < this.stats.bbgame.plays.period[o].play.length; x++) {
          if (this.stats.bbgame.plays.period[o].play[x].action === 'GOOD' && this.stats.bbgame.plays.period[o].play[x].type !== 'FT') {
            if (this.stats.bbgame.plays.period[o].play[x].vh === 'V') {
              this.lastFGTeam0 = this.stats.bbgame.plays.period[o].play[x].time
            } else {
              this.lastFGTeam1 = this.stats.bbgame.plays.period[o].play[x].time
            }
          }
        }
      }
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
