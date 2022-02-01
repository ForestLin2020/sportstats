<template>
  <button class="btn btn-primary" @click="clearGames"> Clear </button>

  <!-- <h1>FooballAthlete</h1> -->
  <div v-for="(position, index) in playerPositions" :key="index">
    <Quarterback
      v-if="position == 'QB'"
      :selected="selected"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <RunningBack v-if="position == 'RB'" />
    <Linebacker v-if="position == 'LB'" />
    <DefensiveBack v-if="position == 'DB'" />
    <WideReceiver v-if="position == 'WR'" />
    <OffensiveLine v-if="position == 'OL'" />
    <DefensiveLine v-if="position == 'DL'" />
    <TightEnd v-if="position == 'TE'" />
    <Kicker v-if="position == 'K'" />
    <Punter v-if="position == 'P'" />
  </div>
</template>

<script>
import Quarterback from '@/components/football_position/Quarterback.vue'
import RunningBack from '@/components/football_position/RunningBack.vue'
import Linebacker from '@/components/football_position/Linebacker.vue'
import DefensiveBack from '@/components/football_position/DefensiveBack.vue'
import WideReceiver from '@/components/football_position/WideReceiver.vue'
import OffensiveLine from '@/components/football_position/OffensiveLine.vue'
import DefensiveLine from '@/components/football_position/DefensiveLine.vue'
import TightEnd from '@/components/football_position/TightEnd.vue'
import Kicker from '@/components/football_position/Kicker.vue'
import Punter from '@/components/football_position/Punter.vue'

export default {
  name: 'FootballAthlete',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      playerPositions: [],
      gamesRecordPlayerInCleared: [],
      gameYears: []
    }
  },
  components: {
    Quarterback,
    RunningBack,
    Linebacker,
    DefensiveBack,
    WideReceiver,
    OffensiveLine,
    DefensiveLine,
    TightEnd,
    Kicker,
    Punter
  },
  mounted () {
    console.log('FBA mounted')
    // clear the old data
    if (this.gamesRecordPlayerInCleared) {
      this.gamesRecordPlayerInCleared = []
      this.playerPositions = []
    }

    const data = this.gamesRecordPlayerIn
    for (var i = 0; i < data.length; i++) {
      const byuTeamIndex = data[i].fbgame.team.findIndex(x => x.name === 'BYU') // find index in array
      const oppoTeamIndex = data[i].fbgame.team.findIndex(x => x.name !== 'BYU') // find index in array
      const byuTeamStat = data[i].fbgame.team[byuTeamIndex]
      const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
      const playerStat = byuTeamStat.player[playerIndex]

      // Insert game's info into player's info -> Makes template much easier to render data
      // -> Makes data into same layer if they are stored in different place
      // -> response.data[i].fbgame.team[0].player[p].nid = response.data[i].nid
      playerStat.nid = data[i].nid
      playerStat.title = data[i].title
      playerStat.schedule_year = data[i].schedule_year
      playerStat.event_date = data[i].event_date
      playerStat.scores = data[i].fbgame.scores
      playerStat.venue = data[i].fbgame.venue
      playerStat.linescore = data[i].fbgame.team[byuTeamIndex].linescore
      playerStat.opp_score = data[i].fbgame.team[oppoTeamIndex].linescore.score
      playerStat.byu_score = data[i].fbgame.team[byuTeamIndex].linescore.score
      this.gamesRecordPlayerInCleared.push(playerStat)

      if (playerStat.opos && this.playerPositions.indexOf(playerStat.opos) === -1) {
        this.playerPositions.push(playerStat.opos)
      } else if (playerStat.dpos && this.playerPositions.indexOf(playerStat.dpos) === -1) {
        this.playerPositions.push(playerStat.dpos)
      } else if (playerStat.punt) {
        console.log('You might a Punter.') // punter and kicker at the sametime
      } else if (playerStat.pat || playerStat.ko) {
        console.log('You might a Kicker.')
      }

      // store different years for grouping stats table
      if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
        this.gameYears.push(playerStat.schedule_year)
      }
    }
    console.log(this.gamesRecordPlayerInCleared)
    console.log('playerPositions', this.playerPositions)
  },
  methods: {
    clearGames () {
      console.log('FBA clearGames')

      // clear the old data
      if (this.gamesRecordPlayerInCleared) {
        this.gamesRecordPlayerInCleared = []
        this.playerPositions = []
      }

      const data = this.gamesRecordPlayerIn
      for (var i = 0; i < data.length; i++) {
        const byuTeamIndex = data[i].fbgame.team.findIndex(x => x.name === 'BYU') // find index in array
        const oppoTeamIndex = data[i].fbgame.team.findIndex(x => x.name !== 'BYU') // find index in array
        const byuTeamStat = data[i].fbgame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].fbgame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].fbgame.scores
        playerStat.venue = data[i].fbgame.venue
        playerStat.linescore = data[i].fbgame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].fbgame.team[oppoTeamIndex].linescore.score
        playerStat.byu_score = data[i].fbgame.team[byuTeamIndex].linescore.score
        this.gamesRecordPlayerInCleared.push(playerStat)

        if (playerStat.opos && this.playerPositions.indexOf(playerStat.opos) === -1) {
          this.playerPositions.push(playerStat.opos)
        } else if (playerStat.dpos && this.playerPositions.indexOf(playerStat.dpos) === -1) {
          this.playerPositions.push(playerStat.dpos)
        } else if (playerStat.punt) {
          console.log('You might a Punter.') // punter and kicker at the sametime
        } else if (playerStat.pat || playerStat.ko) {
          console.log('You might a Kicker.')
        }

        // store different years for grouping stats table
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      console.log(this.gamesRecordPlayerInCleared)
      console.log('playerPositions', this.playerPositions)
    }
  }
}
</script>
