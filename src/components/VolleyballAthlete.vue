<template>
    <h5>Volleyball</h5>
</template>

<script>
export default {
  name: 'VolleyballAthlete',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      gamesRecordPlayerInCleared: [],
      gameYears: []
    }
  },
  methods: {
    reorganizeGames () {
      // clear the old data
      if (this.gamesRecordPlayerInCleared) {
        this.gamesRecordPlayerInCleared = []
        this.gameYears = []
      }

      const data = this.gamesRecordPlayerIn
      for (var i = 0; i < data.length; i++) {
        // x.id === 'BYU' or x.name === 'BYU' or x.name === 'BY'
        const byuTeamIndex = data[i].vbgame.team.findIndex(x => (x.name === 'BYU' | x.name === 'BY' | x.id === 'BYU')) // find index in array
        const oppoTeamIndex = data[i].vbgame.team.findIndex(x => x.name !== 'BYU') // find index in array
        const byuTeamStat = data[i].vbgame.team[byuTeamIndex]
        const playerIndex = byuTeamStat.player.findIndex(x => x.player_nid === this.selected.athleteNid - 0)
        const playerStat = byuTeamStat.player[playerIndex]

        // Insert game's info into player's info -> Makes template much easier to render data
        // -> Makes data into same layer if they are stored in different place
        // -> response.data[i].vbgame.team[0].player[p].nid = response.data[i].nid
        playerStat.nid = data[i].nid
        playerStat.title = data[i].title
        playerStat.schedule_year = data[i].schedule_year
        playerStat.event_date = data[i].event_date
        playerStat.scores = data[i].vbgame.scores
        playerStat.venue = data[i].vbgame.venue
        playerStat.linescore = data[i].vbgame.team[byuTeamIndex].linescore
        playerStat.opp_score = data[i].vbgame.team[oppoTeamIndex].linescore.score
        playerStat.byu_score = data[i].vbgame.team[byuTeamIndex].linescore.score
        this.gamesRecordPlayerInCleared.push(playerStat)

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()

      console.log('gamesRecordPlayerInCleared', this.gamesRecordPlayerInCleared)
      console.log('gameYears', this.gameYears)
    }
  }
}
</script>
