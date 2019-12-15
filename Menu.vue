<template>
  <div>
    <link href="https://fonts.googleapis.com/css?family=Athiti&display=swap" rel="stylesheet" />
    <font face="Athiti, sans-serif">
      <nav class="navbar navbar-expand-sm navbar-light bg-light">
        <a class="navbar-brand" href="/">KIJHI</a>
        <button
          class="navbar-toggler d-lg-none"
          type="button"
          data-toggle="collapse"
          data-target="#collapsibleNavId"
          aria-controls="collapsibleNavId"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="collapsibleNavId">
          <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
            <li class="nav-item active">
              <a class="nav-link" href="/">
                HOME
                <span class="sr-only">(current)</span>
              </a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/kijhi/user/status">เช็คสถานะ</a>
            </li>

            <li class="nav-item">
              <a class="nav-link" href="/kijhi/user/registerData">กรอกข้อมูล</a>
            </li>
            <li class="nav-item" aria-hidden="staff">
              <a class="nav-link" href="/kijhi/staff/staff">STAFF</a>
            </li>
             <li class="nav-item" aria-hidden="staff">
              <a class="nav-link" href="/kijhi/staff/admin">ADMIN</a>
            </li>
          </ul>
          <button
            @click="SignIn()"
            v-show="show"
            color="success"
            class="btn btn-outline-success my-2 my-sm-0"
          >SignIn</button>
          <button
            @click="SignOut()"
            v-show="show"
            color="danger"
            class="btn btn-outline-danger my-2 my-sm-0"
          >SignOut</button>
          <!-- <b-dropdown id="dropdown-dropleft" dropleft text="LOGIN" variant="outline-info" class="my-2 my-sm-0">
        <b-dropdown-item href="/kijhi/login">USER LOGIN</b-dropdown-item>
        <b-dropdown-item href="/">STAFF LOGIN</b-dropdown-item>
          </b-dropdown> -->
          <!-- <a to="kijhi/login" href="/kijhi/login" class="btn btn-outline-info my-2 my-sm-0">LOGIN</a> -->
        </div>
      </nav>
    </font>
  </div>
</template>

<script>
import firebase from 'firebase/app'
import 'firebase/auth'
import { db } from '@/plugins/firebaseDB.js'
export default {
  data() {
    return {
      userName: '',
      email: '',
      disableSignin: false,
      show: true,
      tokenG: ''
    }
  },
  methods: {
    staff() {
      firebase.auth().onAuthStateChanged(user => {
        if (user) {
          // User is signed in.
        } else {
          // No user is signed in.
        }
      })
    },
    SignIn() {
      var provider = new firebase.auth.GoogleAuthProvider()
      firebase
        .auth()
        .signInWithPopup(provider)
        .then(result => {
          // This gives you a Google Access Token. You can use it to access the Google API.
          var token = result.credential.accessToken
          console.log('Token: ' + token)

          // The signed-in user info.
          var user = result.user
          console.log(user)
          this.userName = user.displayName
          this.show = false
          this.disableSignin = true
          // ...
        })
        .catch(error => {
          alert('Oop. What the _uck Error' + error.message)
          // Handle Errors here.
          var errorCode = error.code
          var errorMessage = error.message
          // The email of the user's account used.
          var email = error.email
          // The firebase.auth.AuthCredential type that was used.
          var credential = error.credential
          // ...
        })
    },

    SignOut() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          console.log('Sign-out')
          this.loginUser = null
          this.userName = 'Bye'
          // Sign-out successful.
        })
        .catch(error => {
          // An error happened.
          alert('Oop. What the _uck Error' + error.message)
        })
    }
  }
}
</script>

<style>
</style>