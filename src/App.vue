<template>
  <div id="app">
    <div v-if="ready">
      <div v-if="authorized" >
        <img :src="profile.photoURL" alt="">
        <h1>{{ profile.displayName }}</h1>
        <button type="button" @click="logout" class="button is-danger">logout</button>
      </div>
      <div v-else>
        <button type="button" @click="login" class="button is-primary">login</button>
      </div>
    </div>
    <div v-else>
      <button type="button" name="button" class="button is-primary is-loading">login</button>
    </div>
  </div>
</template>

<script>

import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyA4D64nxPAZZShEkAHkZXGYnKiaTMNx0Yc',
  authDomain: 'dbpresent-f7638.firebaseapp.com',
  databaseURL: 'https://dbpresent-f7638.firebaseio.com',
  projectId: 'dbpresent-f7638',
  storageBucket: 'dbpresent-f7638.appspot.com',
  messagingSenderId: '192584276178'
}
firebase.initializeApp(config)
var provider = new firebase.auth.FacebookAuthProvider()
import Hello from './components/Hello'
export default {
  name: 'app',
  data () {
    return {
      todoInput: '',
      xxx: 'xxx',
      profile: {},
      ready: false,
      authorized: false,
      todos: []
    }
  },
  methods: {
    login () {
      firebase.auth().signInWithRedirect(provider)
    },
    logout () {
      let vm = this
      firebase.auth().signOut().then(function () {
        vm.authorized = false
      }, function (error) {
        console.error(error)
      })
    }
  },
  components: {
    Hello
  },
  mounted () {
    let vm = this
    firebase.auth().onAuthStateChanged(function (user) {
      if (user) {
        vm.authorized = true
        vm.profile = user
      }
      vm.ready = true
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
