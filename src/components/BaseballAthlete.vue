<template>
  <h1>Baseball</h1>
  <div v-for="(type, index) in tableTypes" :key="index">
    <Infielder
      v-if="type == 'Hitter'"
      :selected="selected"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"
    />
    <!-- 1. Right Hand Pitcher, Left Hand Pitcher -->
    <!-- 2. Infielder, Outfielder, Designated Hitter, Catcher -->
    <!-- First Base -->
  </div>
</template>

<script>
// import Quarterback from '@/components/football_position/Quarterback.vue'
import Infielder from '@/components/baseball_tables/Infielder.vue'

export default {
  name: 'Baseball',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      playerPositions: [],
      tableTypes: [],
      gamesRecordPlayerInCleared: [],
      gameYears: []
      // // QB_table
      // QB: ['QB'],
      // // OL_table
      // OL: ['OL', 'RG', 'LG', 'RT', 'LT', 'C'],
      // // Offensive_table >> WR, RB, and TE are the same table type
      // TE: ['TE'],
      // WR: ['WR', 'slot'],
      // RB: ['RB', 'FB', 'HB'],
      // // Defensive_table >> LB, DB, DL are all the same table type
      // DB: ['CB', 'FS', 'SS', 'DB', 'RC', 'RCB', 'LC', 'LCB'],
      // LB: ['MLB', 'OLB', 'Nick', 'ILB', 'WLB'],
      // DL: ['DL', 'NT', 'DT', 'DE', 'edge'],
      // // KP_table >> Special Teams
      // K: ['K', 'kicker'],
      // P: ['P']
    }
  },
  components: {
    Infielder
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
        const byuTeamIndex = data[i].bsgame.team.findIndex(x => x.id === 'BYU') // find index in array
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
        // if (playerStat.opos && this.playerPositions.indexOf(playerStat.opos) === -1) {
        //   this.playerPositions.push(playerStat.opos)
        // } else if (playerStat.dpos && this.playerPositions.indexOf(playerStat.dpos) === -1) {
        //   this.playerPositions.push(playerStat.dpos)
        // } else if (playerStat.punt || playerStat.pat || playerStat.ko) {
        //   this.playerPositions.push('KP') // punter and kicker at the sametime
        // }

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()
      this.gameYears.reverse()
      // ===== Determine the table type for player =====
      // for (var j = 0; j < this.playerPositions.length; j++) {
      //   // QB_table
      //   if (this.QB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('QB_table') === -1) this.tableTypes.push('QB_table')
      //   // OL_table
      //   if (this.OL.includes(this.playerPositions[j]) & this.tableTypes.indexOf('OL_table') === -1) this.tableTypes.push('OL_table')
      //   // WR, RB, and TE are the same table type >> Offensive_table
      //   if (this.WR.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Offensive_table') === -1) this.tableTypes.push('Offensive_table')
      //   if (this.TE.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Offensive_table') === -1) this.tableTypes.push('Offensive_table')
      //   if (this.RB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Offensive_table') === -1) this.tableTypes.push('Offensive_table')
      //   // LB, DB, DL are all the same table type >> Defensive_table
      //   if (this.LB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Defensive_table') === -1) this.tableTypes.push('Defensive_table')
      //   if (this.DB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Defensive_table') === -1) this.tableTypes.push('Defensive_table')
      //   if (this.DL.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Defensive_table') === -1) this.tableTypes.push('Defensive_table')
      //   // Special Teams >> 'KP_table'
      //   if (this.playerPositions[j] === 'KP' & this.tableTypes.indexOf('KP_table') === -1) this.tableTypes.push('KP_table')
      // }
      console.log(
        'gamesRecordPlayerInCleared',
        this.gamesRecordPlayerInCleared
      )
      // console.log('playerPositions', this.playerPositions)
      // console.log('tableTypes', this.tableTypes)
      console.log('gameYears', this.gameYears)
      this.tableTypes.push('Hitter')
    }
  }
}
</script>
