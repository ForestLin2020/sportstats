<template>
  <div v-for="(type, index) in tableTypes" :key="index">
    <Quarterback
      v-if="type == 'QB_table'"
      :selected="selected"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <OffensiveLine v-if="type == 'OL_table'" />
    <Offensive v-if="type == 'Offensive_table'" />
    <Defensive v-if="type == 'Defensive_table'" />
    <KickerPunter v-if="type == 'KP_table'" />
  </div>
</template>

<script>
import Quarterback from '@/components/football_position/Quarterback.vue'
import OffensiveLine from '@/components/football_position/OffensiveLine.vue'
import Offensive from '@/components/football_position/Offensive.vue'
import Defensive from '@/components/football_position/Defensive.vue'
import KickerPunter from '@/components/football_position/KickerPunter.vue'

export default {
  name: 'FootballAthlete',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      playerPositions: [],
      tableTypes: [],
      gamesRecordPlayerInCleared: [],
      gameYears: [],
      // QB_table
      QB: ['QB'],
      // OL_table
      OL: ['OL', 'RG', 'LG', 'RT', 'LT', 'C'],
      // Offensive_table >> WR, RB, and TE are the same table type
      TE: ['TE'],
      WR: ['WR', 'slot'],
      RB: ['RB', 'FB', 'HB'],
      // Defensive_table >> LB, DB, DL are all the same table type
      DB: ['CB', 'FS', 'SS', 'DB', 'RC', 'RCB', 'LC', 'LCB'],
      LB: ['MLB', 'OLB', 'Nick', 'ILB', 'WLB'],
      DL: ['DL', 'NT', 'DT', 'DE', 'edge'],
      // KP_table >> Special Teams
      K: ['K', 'kicker'],
      P: ['P']
    }
  },
  components: {
    Quarterback,
    OffensiveLine,
    Defensive,
    Offensive,
    KickerPunter
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
        // x.id === 'BYU' or x.name === 'BYU' or x.name === 'BY'
        const byuTeamIndex = data[i].fbgame.team.findIndex(x => (x.name === 'BYU' | x.name === 'BY' | x.id === 'BYU')) // find index in array
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

        // ===== Determine player position =====
        if (playerStat.opos && this.playerPositions.indexOf(playerStat.opos) === -1) {
          this.playerPositions.push(playerStat.opos)
        } else if (playerStat.dpos && this.playerPositions.indexOf(playerStat.dpos) === -1) {
          this.playerPositions.push(playerStat.dpos)
        } else if (playerStat.punt || playerStat.pat || playerStat.ko) {
          this.playerPositions.push('KP') // punter and kicker at the sametime
        }

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()
      // ===== Determine the table type for player =====
      for (var j = 0; j < this.playerPositions.length; j++) {
        // QB_table
        if (this.QB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('QB_table') === -1) this.tableTypes.push('QB_table')
        // OL_table
        if (this.OL.includes(this.playerPositions[j]) & this.tableTypes.indexOf('OL_table') === -1) this.tableTypes.push('OL_table')
        // WR, RB, and TE are the same table type >> Offensive_table
        if (this.WR.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Offensive_table') === -1) this.tableTypes.push('Offensive_table')
        if (this.TE.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Offensive_table') === -1) this.tableTypes.push('Offensive_table')
        if (this.RB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Offensive_table') === -1) this.tableTypes.push('Offensive_table')
        // LB, DB, DL are all the same table type >> Defensive_table
        if (this.LB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Defensive_table') === -1) this.tableTypes.push('Defensive_table')
        if (this.DB.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Defensive_table') === -1) this.tableTypes.push('Defensive_table')
        if (this.DL.includes(this.playerPositions[j]) & this.tableTypes.indexOf('Defensive_table') === -1) this.tableTypes.push('Defensive_table')
        // Special Teams >> 'KP_table'
        if (this.playerPositions[j] === 'KP' & this.tableTypes.indexOf('KP_table') === -1) this.tableTypes.push('KP_table')
      }

      console.log('gamesRecordPlayerInCleared', this.gamesRecordPlayerInCleared)
      console.log('playerPositions', this.playerPositions)
      console.log('tableTypes', this.tableTypes)
      console.log('gameYears', this.gameYears)
    }
  }
}
</script>
