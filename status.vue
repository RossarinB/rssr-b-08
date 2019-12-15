<template>
  <div>
    <link href="https://fonts.googleapis.com/css?family=Athiti&display=swap" rel="stylesheet" />
    <font face="Athiti, sans-serif">
      <b-table striped hover :data="data" outlined></b-table>
      <div class="card">
        <b-table striped hover :items="items" outlined >
          <!-- v-for="{items} in items" :key="items" -->
          <!-- <div class="card m-5" v-for="pro1 in cart" :key="pro1.id"> -->
          <!-- <img class="card-img-top" :src="pro1.image" alt=""> -->
          <!-- <div class="card-body">
              <img class="card-img" :src="pro1.image" width="40%" alt="">
          <h4 class="card-title"> {{pro1.name}} {{pro1.price}}baht </h4>-->
          <!-- </div> -->
          <!-- </div> -->
          <!-- <table v-for="items in items" :key="items">{{items.code}}</table> -->
        </b-table>
        <center>
          <b-button href="\kijhi\user\upload" class="btn btn-success my-2 my-sm-0">ชำระเงิน</b-button>
        </center>
      </div>
    </font>
  </div>
</template>
<script>
import { db } from '@/plugins/firebaseDB.js'
export default {
  data() {
    db.collection('MemberData/')
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          // doc.data() is never undefined for query doc snapshots
          this.data.push(doc.data())
        })
      })
    db.collection('Staff/')
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          // doc.data() is never undefined for query doc snapshots
          this.items.push(doc.data())
          this.data.push(doc.data())
        })
      })
       
    return {
      items: [],
      data:[]
    }
  },
  methods: {}
}
</script>
<style>
body {
  margin: 0;
  padding: 0;
  background-image: url('https://images.pexels.com/photos/2199293/pexels-photo-2199293.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940');
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
