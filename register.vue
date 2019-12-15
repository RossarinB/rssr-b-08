<template>
  <form class="from-group" @submit.prevent="addRegister">
    <label>
      <p class="label-txt">ENTER YOUR EMAIL</p>
      <input type="text" class="input" name="email" v-model="email" />
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>
    <label>
      <p class="label-txt">ENTER YOUR USERNAME</p>
      <input type="text" class="input" name="username" v-model="username" />
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>
    <label>
      <p class="label-txt">ENTER YOUR NAME</p>
      <input type="text" class="input" name="name" v-model="name" />
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>
    <label>
      <p class="label-txt">ENTER YOUR LASTNAME</p>
      <input type="text" class="input" name="lastname" v-model="lastname" />
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>
    <label>
      <p class="label-txt">ENTER YOUR GENDER</p>
      <input type="text" class="input" name="gender" v-model="gender" list="my-list-id" />
      <datalist id="my-list-id">
        <option>GENDER</option>
        <option v-for="gender in genders" :key="gender" :gender="gender">{{ gender }}</option>
      </datalist>
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>
    <label>
      <p class="label-txt">ENTER YOUR PHONE NUMBER</p>
      <input type="text" class="input" name="phone" v-model="phone" />
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>
    <label>
      <p class="label-txt">ENTER YOUR PASSWORD</p>
      <input type="text" class="input" name="password" v-model="password" />
      <div class="line-box">
        <div class="line"></div>
      </div>
    </label>

    <button type="submit" @click="addRegister()" class="btn btn-outline-info my-2 my-sm-0">submit</button>
  </form>
</template>
<script>
// import firebase from 'firebase/app'
// import 'firebase/auth'
import { db } from '@/plugins/firebaseDB.js'
export default {
  computed: {
    state() {
      return this.name.length >= 4 ? true : false
    },
    invalidFeedback() {
      if (this.name.length > 4) {
        return ''
      } else if (this.name.length > 0) {
        return 'Enter at least 4 characters'
      } else {
        return 'Please enter something'
      }
    },
    validFeedback() {
      return this.state === true ? 'Thank you' : ''
    }
  },
  data() {
    return {
      name: '',
      lastname: '',
      gender: '',
      username: '',
      password: '',
      email: '',
      phone: '',
      dataRegister: [],
      genders: ['Male', 'Female']
    }
  },
  methods: {
    addRegister() {
      
      db.collection('Member')
        .doc(this.username)
        .set({
          name: this.name,
          lastname: this.lastname,
          gender: this.gender,
          username: this.username,
          password: this.password,
          email: this.email,
          phone: this.phone
        })
        .then(docRef => {
          console.log('Document written with ID: ', docRef.id)
        })
        .catch(error => {
          console.error('Error adding document: ', error)
        })
    }
    // this.$router.push
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #17a2b8;
  height: 100vh;
}
form {
  width: 60%;
  margin: 60px auto;
  background: #efefef;
  padding: 60px 120px 80px 120px;
  text-align: center;
  -webkit-box-shadow: 2px 2px 3px rgba(0, 0, 0, 0.1);
  box-shadow: 2px 2px 3px rgba(0, 0, 0, 0.1);
}
label {
  display: block;
  position: relative;
  margin: 40px 0px;
}
.label-txt {
  position: absolute;
  top: -1.6em;
  padding: 10px;
  font-family: sans-serif;
  font-size: 0.8em;
  letter-spacing: 1px;
  color: rgb(120, 120, 120);
  transition: ease 0.3s;
}
.input {
  width: 100%;
  padding: 10px;
  background: transparent;
  border: none;
  outline: none;
}

.line-box {
  position: relative;
  width: 100%;
  height: 2px;
  background: #bcbcbc;
}

.line {
  position: absolute;
  width: 0%;
  height: 2px;
  top: 0px;
  left: 50%;
  transform: translateX(-50%);
  background: #17a2b8;
  transition: ease 0.6s;
}

.input:focus + .line-box .line {
  width: 100%;
}

.label-active {
  top: -3em;
}

button {
  display: inline-block;
  padding: 12px 24px;
  background: rgb(220, 220, 220);
  font-weight: bold;
  color: rgb(120, 120, 120);
  border: none;
  outline: none;
  border-radius: 3px;
  cursor: pointer;
  transition: ease 0.3s;
}

button:hover {
  background: #17a2b8;
  color: #ffffff;
}
</style>
