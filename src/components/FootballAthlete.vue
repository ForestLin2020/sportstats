<template>
  <div v-for="(type, index) in tableTypes" :key="index">
    <Quarterback
      v-if="type == 'QB_table'"
      :selected="selected"
      v-bind:key="selected.athleteNid"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <OffensiveLine
      ref="myTable"
      v-if="type == 'OL_table'"
      :selected="selected"
      v-bind:key="selected.athleteNid"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <Offensive
      v-if="type == 'Offensive_table'"
      :selected="selected"
      :key="selected.athleteNid"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <Defensive
      v-if="type == 'Defensive_table'"
      :selected="selected"
      v-bind:key="selected.athleteNid"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <KickerPunter
      v-if="type == 'KP_table'"
      :selected="selected"
      v-bind:key="selected.athleteNid"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
    <Return
      v-if="type == 'KrPr_table'"
      :selected="selected"
      v-bind:key="selected.athleteNid"
      :gamesRecordPlayerInCleared="gamesRecordPlayerInCleared"
      :gameYears="gameYears"/>
  </div>
</template>

<script>
import Quarterback from '@/components/football_tables/Quarterback.vue'
import OffensiveLine from '@/components/football_tables/OffensiveLine.vue'
import Offensive from '@/components/football_tables/Offensive.vue'
import Defensive from '@/components/football_tables/Defensive.vue'
import KickerPunter from '@/components/football_tables/KickerPunter.vue'
import Return from '@/components/football_tables/Return.vue'

export default {
  name: 'FootballAthlete',
  props: ['selected', 'gamesRecordPlayerIn'],
  data () {
    return {
      uniqueKey: 0,
      playerPositions: [],
      tableTypes: [],
      gamesRecordPlayerInCleared: [],
      gameYears: [],
      // Quarterback table
      QB_table: ['QB'],
      // Offensive Line table
      OL_table: ['OL', 'RG', 'LG', 'RT', 'LT', 'C'],
      // Offensive table >> WR, RB, and TE are the same table type
      Offensive_table: ['TE', 'WR', 'slot', 'RB', 'FB', 'HB'],
      // Defensive table >> LB, DB, DL are all the same table type
      Defensive_table: ['DB', 'CB', 'FS', 'SS', 'DB', 'RC', 'RCB', 'LC', 'LCB', 'MLB', 'OLB', 'Nick', 'ILB', 'WLB', 'LB', 'DL', 'NT', 'DT', 'DE', 'edge'],
      // Kicker Punter table
      KP_table: ['KP', 'K', 'kicker', 'P']
    }
  },
  components: {
    Quarterback,
    OffensiveLine,
    Defensive,
    Offensive,
    KickerPunter,
    Return
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
        const byuTeamIndex = data[i].fbgame.team.findIndex(x => x.id === 'BYU') // find index in array
        const oppoTeamIndex = data[i].fbgame.team.findIndex(x => x.id !== 'BYU') // find index in array
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

        // (1)KP >> (2)QB >> (3)Offensive >> (4)Defensive >> (5)OL
        if ((playerStat.punt || playerStat.ko || playerStat.fg) && this.tableTypes.indexOf('KP_table') === -1) {
          this.tableTypes.push('KP_table')
        } else if ((playerStat.pr || playerStat.kr) && this.tableTypes.indexOf('KrPr_table') === -1) {
          this.tableTypes.push('KrPr_table')
        } else if (((playerStat.rcv && playerStat.rush && playerStat.pass) || this.QB_table.includes(playerStat.opos) !== false) && (this.tableTypes.indexOf('QB_table') === -1)) {
          this.tableTypes.push('QB_table')
        } else if (((playerStat.rcv || playerStat.rush || playerStat.fumbles) && (this.tableTypes.indexOf('Offensive_table') === -1))) {
          //  && (this.Offensive_table.includes(playerStat.opos) !== false)
          this.tableTypes.push('Offensive_table') // WR, RB, and TE are the same table type >> Offensive_table
        } else if ((playerStat.defense || playerStat.ir) && this.tableTypes.indexOf('Defensive_table') === -1) {
          //  && (this.Defensive_table.includes(playerStat.dpos) !== false)
          this.tableTypes.push('Defensive_table') // LB, DB, DL are all the same table type >> Defensive_table
        } else if ((playerStat.gp === '1' || this.OL_table.includes(playerStat.opos) !== false) && this.tableTypes.indexOf('OL_table') === -1) {
          if (!playerStat.defense && !playerStat.ir && !playerStat.rcv && !playerStat.ruch && !playerStat.fumbles) {
            this.tableTypes.push('OL_table')
            // console.log(i)
            // console.log(playerStat.event_date)
            // console.log(!playerStat.defense)
            // console.log(!playerStat.ir)
            // console.log(!playerStat.rcv)
            // console.log(!playerStat.rush)
            // console.log(!playerStat.fumbles)
          }
        }

        // ===== First if: Determine player position -> add position, playerStat.opos='WR'=====
        // ===== Second if: Whether this player in this position do some contribution -> adding table type =====
        if (playerStat.opos && this.playerPositions.indexOf(playerStat.opos) === -1) {
          this.playerPositions.push(playerStat.opos)
        } else if (playerStat.dpos && this.playerPositions.indexOf(playerStat.dpos) === -1) {
          this.playerPositions.push(playerStat.dpos)
        } else if ((playerStat.punt || playerStat.ko || playerStat.fg) && this.playerPositions.indexOf('KP') === -1) {
          this.playerPositions.push('KP') // punter and kicker at the sametime
        }

        // ===== store different years for grouping stats table =====
        if (this.gameYears.indexOf(playerStat.schedule_year) === -1) {
          this.gameYears.push(playerStat.schedule_year)
        }
      }
      // ===== Order the games' year for table order =====
      this.gameYears.sort()
      this.gameYears.reverse()

      console.log('gamesRecordPlayerInCleared in FA', this.gamesRecordPlayerInCleared)
      console.log('playerPositions', this.playerPositions)
      console.log('tableTypes', this.tableTypes)
      console.log('gameYears', this.gameYears)
    }
  }
}
</script>
