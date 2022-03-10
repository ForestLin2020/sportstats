<template>
  <h1>Baseball</h1>
    <NonPitcher
      v-if="playerPositions.indexOf('p') === -1 & playerPositions.length > 0"
      :selected="selected"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"
    />
    <Pitcher
      v-if="playerPositions.indexOf('p') !== -1 & playerPositions.length > 0"
      :selected="selected"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"
    />
</template>

<script>
// import Quarterback from '@/components/football_position/Quarterback.vue'
import NonPitcher from '@/components/baseball_tables/NonPitcher.vue'
import Pitcher from '@/components/baseball_tables/Pitcher.vue'

export default {
  name: 'Baseball',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      playerPositions: [],
      tableTypes: [],
      gamesRecordPlayerInCleared: [],
      gameYears: []
    }
  },
  components: {
    NonPitcher,
    Pitcher
  },
  methods: {
    reorganizeGames () {
      // clear the old data
      if (this.gamesRecordPlayerInCleared) {
        this.gamesRecordPlayerInCleared = []
        this.playerPositions = []
        this.gameYears = []
        this.tableTypes = []
      }

      const data = this.gamesRecordPlayerIn
      for (var i = 0; i < data.length; i++) {
        const byuTeamIndex = data[i].bsgame.team.findIndex(x => (x.id === 'BYU' || x.name === 'BYU')) // find index in array
        const oppoTeamIndex = data[i].bsgame.team.findIndex(x => x.id !== 'BYU') // find index in array
        const byuTeamStat = data[i].bsgame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].bsgame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].bsgame.scores
        playerStat.venue = data[i].bsgame.venue
        playerStat.linescore = data[i].bsgame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].bsgame.team[oppoTeamIndex].linescore.runs
        playerStat.byu_score = data[i].bsgame.team[byuTeamIndex].linescore.runs
        this.gamesRecordPlayerInCleared.push(playerStat)

        // ===== Determine player position =====
        if (playerStat.pos && this.playerPositions.indexOf(playerStat.pos) === -1) {
          this.playerPositions.push(playerStat.pos)
        } else if (playerStat.pitching && this.playerPositions.indexOf('p') === -1) {
          this.playerPositions.push('p')
        } else if (playerStat.hitting === -1) {
          this.playerPositions.push('non-p')
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
