<template>
  <h1>Soccer</h1>
  <Goalkeeper
    v-if="playerPositions.indexOf('gk') !== -1 & playerPositions.length > 0"
    :selected="selected"
    :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
    :gameYears="gameYears"
  />
    <SoccerPlayer
    v-if="playerPositions.indexOf('gk') === -1 & playerPositions.length > 0"
    :selected="selected"
    :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
    :gameYears="gameYears"
  />
</template>

<script>
// import Quarterback from '@/components/football_position/Quarterback.vue'
import SoccerPlayer from '@/components/soccer_tables/SoccerPlayer.vue'
import Goalkeeper from '@/components/soccer_tables/Goalkeeper.vue'

export default {
  name: 'Soccer',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      playerPositions: [],
      gamesRecordPlayerInCleared: [],
      gameYears: []
    }
  },
  components: {
    SoccerPlayer,
    Goalkeeper
  },
  methods: {
    reorganizeGames () {
      // clear the old data
      if (this.gamesRecordPlayerInCleared) {
        this.gamesRecordPlayerInCleared = []
        this.playerPositions = []
        this.gameYears = []
      }

      const data = this.gamesRecordPlayerIn
      for (var i = 0; i < data.length; i++) {
        const byuTeamIndex = data[i].sogame.team.findIndex(x => (x.id === 'BYU' || x.name === 'BYU')) // find index in array
        const oppoTeamIndex = data[i].sogame.team.findIndex(x => x.id !== 'BYU') // find index in array
        const byuTeamStat = data[i].sogame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].sogame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].sogame.scores
        playerStat.venue = data[i].sogame.venue
        playerStat.linescore = data[i].sogame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].sogame.team[oppoTeamIndex].linescore.score
        playerStat.byu_score = data[i].sogame.team[byuTeamIndex].linescore.score
        this.gamesRecordPlayerInCleared.push(playerStat)

        // ===== Determine player position =====
        if (playerStat.pos && this.playerPositions.indexOf(playerStat.pos) === -1) {
          this.playerPositions.push(playerStat.pos)
        }

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()
      this.gameYears.reverse()

      console.log('gamesRecordPlayerInCleared', this.gamesRecordPlayerInCleared)
      console.log('playerPositions', this.playerPositions)
      console.log('gameYears', this.gameYears)
    }
  }
}
</script>
