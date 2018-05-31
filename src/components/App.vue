<template>
  <div class="box">
    <h3 class="title">App: {{ movieId }}</h3>
    <h4>Count : {{status.count}} Price : {{status.price}} ฿</h4>
    <movie @chooseMovie="handleChooseMovie" :movieId="movieId" />
    <seat :movieId="movieId"
        @chooseSeat="handleChooseSeat"
        :selectSeats="selectSeats"
        :firebaseSeats="firebaseSeats" />
  </div>
</template>

<script>
  import firebase from 'firebase'
  import _ from 'lodash'
  import Movie from 'Components/Movie.vue'
  import Seat from 'Components/Seat.vue'
  import { pushToArray } from 'Others/lib'
  const config = {
    apiKey: 'AIzaSyDRsXGKz0ha-0NyQVEy9WjBUkw5HsjUWMI',
    authDomain: 'vuefirebasef.firebaseapp.com',
    databaseURL: 'https://vuefirebasef.firebaseio.com',
    projectId: 'vuefirebasef',
    storageBucket: 'vuefirebasef.appspot.com',
    messagingSenderId: '667997093730'
  }

  const app = firebase.initializeApp(config)
  const db = app.database()

  export default{
    components: {
      Movie,
      Seat
     },
    data(){
      return{
          movieId: '',
          selectSeats: [],
          firebaseSeats: [],
          status: { count: 0, price: 0}
      }
    },
    methods: {
      handleChooseMovie (movieId) {
        if (this.status.count) {
          if (confirm('Data will be lost ?')) {
            this.status = {count: 0, price: 0}
            this.selectSeats = []
          } else {
            return false
          }
        }
        this.movieId = movieId
        const movieRef = db.ref('/').child(this.movieId)
        movieRef.on('value' ,snapshot => {
          const seats = snapshot.val()
          _.forOwn(seats, s => {
            pushToArray(s , this.firebaseSeats)
          })
        })

      },
      handleChooseSeat (seat) {
        // const ids = this.selectSeats.map(s => s.id)
        // const idx = ids.indexOf(seat.id)
        // if (idx === -1) {
        //   this.selectSeats.push(seat)
        // } else {
        //   this.selectSeats.splice(idx, 1)
        // }
        pushToArray(seat, this.selectSeats)


        const movieRef = db.ref().child(this.movieId)
        movieRef.push(seat)

        this.status = this.selectSeats.reduce((summary, s) => {
          summary.count ++
          summary.price += s.price
          return summary
        }, {count: 0, price: 0})
      }
    }
  }
</script>
