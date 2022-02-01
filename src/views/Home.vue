<template>
  <div>
    <button class="btn btn-primary" @click="submit">Game</button>
    <h3 class="color-block">Career & Season Stats</h3>
    <div v-for="(game, _id) in games" :key="_id">
      {{ game.schedule_year }}
      {{ game.fbgame.venue.stadium }}
      {{ game.fbgame.venue.neutralgame}}
    </div>

  </div>
</template>

<script>
export default {
  name: 'Quarterback',
  props: [],
  data () {
    return {
      player: [],
      games: []
    }
  },
  async mounted () {
    // ======= fetch from URL API ======= //1288224
    const athletesUrl = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/athlete/1706/1294759'
    const res = await fetch(athletesUrl)
    const data = await res.json()
    this.player = data
    // console.log(data)
  },
  methods: {
    async submit () {
      // player stats record in each game state
      // const gameUrl = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/boxscore/gameNid'
      const gameUrl = 'https://gamestats.byucougars.byu-dept-athletics-dev.amazon.byu.edu/boxscore/1288224'
      const res = await fetch(gameUrl)
      const data = await res.json()
      this.games.push(data[0])
      // console.log(this.games)
    }
  }
}
</script>
