<template>
  <h5>Defensive</h5>
      <div class="career-and-season">
        <h3 class="color-block">Career & Season Stats</h3>
          <div class="table-responsive">
            <table class="table-striped table-sm table-condensed table table-hover table-bordered total_up">
              <thead class='football-thead'>
                <tr>
                    <th colspan="2">Career</th>
                    <th colspan="9">Tackles</th>
                    <th colspan="5">Sacks</th>
                    <th colspan="2">Interceptions</th>
                    <th colspan="2">Misc</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                    <th>Year</th>
                    <th>GP</th>
                    <th>SOLO</th>
                    <th>AST</th>
                    <th>TOT</th>
                    <th>TFL/YDS</th>
                    <th>NO</th>
                    <th>YDS</th>
                    <th>INT</th>
                    <th>YDS</th>
                    <th>TD</th>
                    <th>FF</th>
                    <th>FR</th>
                    <th>FRYDS</th>
                    <th>QBH</th>
                    <th>PBU</th>
                </tr>
                <!-- <tr v-for="(totals,index) in totalsByYear" :key="index">
                    <td>{{ totals.schedule_year }}</td>
                    <td>{{ totals.gp }}</td>
                    <td>{{ totals.tackles.solo }}</td>
                    <td>{{ totals.tackles.ast }}</td>
                    <td>{{ totals.tackles.tot }}</td>
                    <td>{{ totals.tackles.tfl/yds }}</td>
                    <td>{{ totals.sacks.no }}</td>
                    <td>{{ totals.sacks.yds }}</td>
                    <td>{{ totals.interceptions.int }}</td>
                    <td>{{ totals.interceptions.yds }}</td>
                    <td>{{ totals.interceptions.td }}</td>
                    <td>{{ totals.misc.ff }}</td>
                    <td>{{ totals.misc.fr }}</td>
                    <td>{{ totals.misc.fryds }}</td>
                    <td>{{ totals.misc.qbh }}</td>
                    <td>{{ totals.misc.pbu }}</td>
                </tr> -->
                <!-- <tr>
                    <th>TOTALS:</th>
                    <th>{{ careerTotals.gp }}</th>
                    <th v-if="careerTotals.tackles && careerTotals.tackles.solo" nowrap>{{ careerTotals.tackles.solo }}</th><th v-else>0</th>
                    <th v-if="careerTotals.tackles && careerTotals.tackles.ast" nowrap>{{ careerTotals.tackles.ast }}</th><th v-else>0</th>
                    <th v-if="careerTotals.tackles && careerTotals.tackles.tot" nowrap>{{ careerTotals.tackles.tot }}</th><th v-else>0</th>
                    <th v-if="careerTotals.tackles && careerTotals.tackles.tfl/yds" nowrap>{{ careerTotals.tackles.tfl/yds }}</th><th v-else>0</th>
                    <th v-if="careerTotals.sacks && careerTotals.sacks.no " nowrap>{{ careerTotals.sacks.no }}</th><th v-else>0</th>
                    <th v-if="careerTotals.sacks && careerTotals.sacks.yds" nowrap>{{ careerTotals.sacks.yds }}</th><th v-else>0</th>
                    <th v-if="careerTotals.interceptions && careerTotals.interceptions.int" nowrap>{{ careerTotals.interceptions.int }}</th><th v-else>0</th>
                    <th v-if="careerTotals.interceptions && careerTotals.interceptions.yds" nowrap>{{ careerTotals.interceptions.yds }}</th><th v-else>0</th>
                    <th v-if="careerTotals.interceptions && careerTotals.interceptions.td" nowrap>{{ careerTotals.interceptions.td }}</th><th v-else>0</th>
                    <th v-if="careerTotals.misc && careerTotals.misc.ff" nowrap>{{ careerTotals.misc.ff }}</th><th v-else>0</th>
                    <th v-if="careerTotals.misc && careerTotals.misc.fr" nowrap>{{ careerTotals.misc.fr }}</th><th v-else>0</th>
                    <th v-if="careerTotals.misc && careerTotals.misc.fryds" nowrap>{{ careerTotals.misc.fryds }}</th><th v-else>0</th>
                    <th v-if="careerTotals.misc && careerTotals.misc.qbh" nowrap>{{ careerTotals.misc.qbh }}</th><th v-else>0</th>
                    <th v-if="careerTotals.misc && careerTotals.misc.pbu" nowrap>{{ careerTotals.misc.pbu }}</th><th v-else>0</th>
                </tr> -->
              </tbody>
            </table>
          </div>

          <div v-for="(year, index) in gameYears" :key="index">
            <div class="table-responsive mt-3">
              <table class=" table-striped table-sm table-condensed table table-hover table-bordered total_up">
                <thead class='football-thead'>
                  <tr>
                    <th colspan="3">{{ year }} Games</th>
                    <th colspan="4">Tackles</th>
                    <th colspan="2">Sacks</th>
                    <th colspan="3">Interceptions</th>
                    <th colspan="5">Misc</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <th>Opponent</th>
                    <th>Date</th>
                    <th>Result</th>
                    <th>SOLO</th>
                    <th>AST</th>
                    <th>TOT</th>
                    <th>TFL/YDS</th>
                    <th>NO</th>
                    <th>YDS</th>
                    <th>INT</th>
                    <th>YDS</th>
                    <th>TD</th>
                    <th>FF</th>
                    <th>FR</th>
                    <th>FRYDS</th>
                    <th>QBH</th>
                    <th>PBU</th>
                  </tr>
                  <!-- <tr v-for="(game, _id) in gamesFilterEventsByYear(year)" :key="_id">
                    <td nowrap style="text-align: left">
                        <span v-if="game.venue.stadium  != 'LaVell Edwards' && game.venue.neutralgame != 'Y' ">@</span>{{game.title}}
                    </td>
                    <td nowrap>{{ formDateStr(game.event_date) }}</td>
                    <td nowrap v-if="(game.byu_score - 0) > (game.opp_score - 0)"><b style="color: green">W </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
                    <td nowrap v-if="(game.byu_score - 0) == (game.opp_score - 0)"><b style="color: blue">T </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
                    <td nowrap v-if="(game.byu_score - 0) < (game.opp_score - 0)"><b style="color: red">L </b> {{ game.byu_score }}-{{ game.opp_score }}</td>
                    <td v-if="game.tackles && game.tackles.solo" nowrap>{{ game.tackles.solo }}</td><td v-else>0</td>
                    <td v-if="game.tackles && game.tackles.ast" nowrap>{{ game.tackles.ast }}</td><td v-else>0</td>
                    <td v-if="game.tackles && game.tackles.tot" nowrap>{{ game.tackles.tot }}</td><td v-else>0</td>
                    <td v-if="game.tackles && game.tackles.tfl && game.tackles.yds" nowrap>{{ (game.pass.tfl / game.pass.yds).toFixed(2) }}</td><td v-else>0</td>
                    <td v-if="game.sacks && game.sacks.no" nowrap>{{ game.sacks.no }}</td><td v-else>0</td>
                    <td v-if="game.sacks && game.sacks.yds" nowrap>{{ game.sacks.yds }}</td><td v-else>0</td>
                    <td v-if="game.interceptions && game.interceptions.int" nowrap>{{ game.interceptions.int }}</td><td v-else>0</td>
                    <td v-if="game.interceptions && game.interceptions.yds" nowrap>{{ game.interceptions.yds }}</td><td v-else>0</td>
                    <td v-if="game.interceptions && game.interceptions.td" nowrap>{{ game.interceptions.td }}</td><td v-else>0</td>
                    <td v-if="game.misc && game.misc.ff" nowrap>{{ game.misc.ff }}</td><td v-else>0</td>
                    <td v-if="game.misc && game.misc.fr" nowrap>{{ game.misc.fr }}</td><td v-else>0</td>
                    <td v-if="game.misc && game.misc.fryds" nowrap>{{ game.misc.fryds }}</td><td v-else>0</td>
                    <td v-if="game.misc && game.misc.qbh" nowrap>{{ game.misc.qbh }}</td><td v-else>0</td>
                    <td v-if="game.misc && game.misc.pbu" nowrap>{{ game.misc.pbu }}</td><td v-else>0</td>
                  </tr> -->
                  <!-- <tr v-for="(totals,index) in getTotalsByYear(year)" :key="index">
                    <th>TOTALS:</th>
                    <th>&nbsp;</th>
                    <th>{{ totals.result.win }}-{{ totals.result.lose }}</th>
                    <th>{{ totals.tackles.solo }}</th>
                    <th>{{ totals.tackles.ast }}</th>
                    <th>{{ totals.tackles.tot }}</th>
                    <th>{{ totals.tackles.tfl/yds }}</th>
                    <th>{{ totals.sacks.no }}</th>
                    <th>{{ totals.sacks.yds }}</th>
                    <th>{{ totals.interceptions.int }}</th>
                    <th>{{ totals.interceptions.yds }}</th>
                    <th>{{ totals.interceptions.td }}</th>
                    <th>{{ totals.misc.ff }}</th>
                    <th>{{ totals.misc.fr }}</th>
                    <th>{{ totals.misc.fryds }}</th>
                    <th>{{ totals.misc.qbh }}</th>
                    <th>{{ totals.misc.pbu }}</th>
                  </tr> -->
                </tbody>
              </table>
            </div>
          </div>
        </div>
</template>

<script>
export default {
  name: 'Quarterback',
  props: ['selected', 'gamesRecordPlayerInCleared', 'gameYears'],
  data () {
    return {
      // For UI to format dates
      months: [
        'Jan.', // "January",
        'Feb.', // "February",
        'Mar.', // "March",
        'Apr.', // "April",
        'May"', // "May",
        'Jun.', // "June",
        'Jul.', // "July",
        'Aug.', // "August",
        'Sep.', // "September",
        'Oct.', // "October",
        'Nov.', // "November",
        'Dec.' // "December",
      ],
      playerNid: 1285416,
      games: [],
      // gameYears: [],
      totalsByYear: [],
      careerTotals: []
    }
  }
}
</script>
