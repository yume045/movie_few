<template>
  <div class="box">
    <h3 class="title"> Seat : {{ movieId }}</h3>
    <span v-for="(s, index) in seats" :key="index" class="pad5">
      <button
        :class="className(s)"
        :disabled="s.seated"
        @click="chooseSeat(s)" >
          {{s.id}} {{s.price}} ฿
      </button>
    </span>
  </div>
</template>

<script>
import movie from 'Others/movie.json'
export default {
  props: [ 'movieId' , 'selectSeats' , 'firebaseSeats'],
  computed : {
    seats () {
      return movie[this.movieId]
    }
  },
  methods: {
    className (seat) {
      const ids = this.selectSeats.map(s => s.id)
      const idx = ids.indexOf(seat.id)

      const firebaseIds = this.firebaseSeats.map(s => s.id)
      const firebaseIdx = firebaseIds.indexOf(seat.id)

      return [
        'button',
        {
          'is-danger' : seat.seated,
          'is-primary' : idx != -1,
          'is-warning' : firebaseIdx != -1 && idx === -1
        }
      ]
    },
    chooseSeat (seat) {
      this.$emit('chooseSeat', seat)
    }
  }
}
</script>

<style lang="css">
  .pad5 {
    padding-right: 5px;
  }
</style>
