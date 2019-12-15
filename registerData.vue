<template>
  <div>
    <link href="https://fonts.googleapis.com/css?family=Athiti&display=swap" rel="stylesheet" />
    <font face="Athiti, sans-serif">
      <form class="from-group" @submit.prevent="addData">
        <label>
          <p class="label-txt">USERNAME</p>
          <input type="text" class="input" name="username" v-model="username" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        <label>
          <p class="label-txt">EMAIL</p>
          <input type="email" class="input" name="email" v-model="email" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        <label>
          <p class="label-txt">วันเวลาที่ต้องการให้เข้าไปประเมินการขนย้าย</p>
          <input type="date" class="input" name="oldDate" v-model="oldDate" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        <label>
          <p class="label-txt">วันเวลาที่ต้องการให้เข้าไปขนย้าย</p>
          <input type="date" class="input" name="newDate" v-model="newDate" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        <label>
          <p class="label-txt">ที่อยู่เก่า(ที่อยู่ที่ต้องการทำการประเมิน)</p>
          <input type="text" class="input" name="oldAddress" v-model="oldAddress" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        <label>
          <p class="label-txt">ที่อยู่ใหม่(ที่อยู่ปลายทาง)</p>
          <input type="text" class="input" name="newAddress" v-model="newAddress" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        <label>
          <p class="label-txt">ความต้องการเพิ่มเติม</p>
          <input type="text" class="input" name="other" v-model="other" />
          <div class="line-box">
            <div class="line"></div>
          </div>
        </label>
        
        <b-modal id="modal-center" centered title="Success">
          <p class="my-4">Register Success!</p>
        </b-modal>

        <!-- @click="addRegister()" -->
        <b-link :to="{ path: '/'}">
          <button
            type="submit"
            @click="addData()"
            class="btn btn-outline-info my-2 my-sm-0"
            v-b-modal.modal-center
          >submit</button>
        </b-link>

        <!-- {{dataRegister}} -->
        <!-- <b-table striped hover :items="items"></b-table> -->
      </form>
    </font>
  </div>
</template>
<script>
// import firebase from 'firebase/app'
// import 'firebase/auth'
import { db } from '@/plugins/firebaseDB.js'
export default {
  data() {
    return {
      username: '',
      email: '',
      other: '',
      oldAddress: '',
      newAddress: '',
      oldDate: '',
      newDate: ''
    }
  },
  methods: {
    addData() {
      this.$root.$emit('bv::add::data', 'modal-center', '#btnShow')
      //let id = new Date()
      db.collection('MemberData')
        .doc(this.username)
        .set({
          username: this.username,
          email: this.email,
          Olddate: this.oldDate,
          Newdate: this.newDate,
          OldAddress: this.oldAddress,
          Newaddress: this.newAddress,
          Other: this.other
        })
      //   db.collection('Member')
      //     .doc(this.oldDate)
      //     .set()
      //     .then(docRef => {
      //       console.log('Document written with ID: ', docRef.id)
      //     })
      //     .catch(error => {
      //       console.error('Error adding document: ', error)
      //     })
    }
  }
}

/*this.dataRegister.push({
        name: this.name,
        lastname: this.lastname,
        gender: this.gender,
        username: this.username,
        password: this.password,
        email: this.email,
        phone: this.phone
      })
      console.log(this.dataRegister);*/
// console.log(this.name);
// console.log(this.lastname);
// console.log(this.gender);
// console.log(this.username);
// console.log(this.password);
// console.log(this.email);
// console.log(this.phone);
// console.log(this.PushInList);
// db.collection('Member')
//   .doc(this.name)
//   .set({
//     name: this.name,
//     lastname: this.lastname,
//     gender: this.gender,
//     username: this.username,
//     password: this.password,
//     email: this.email,
//     phone: this.phone,
//     PushInList: this.PushInList,
//   })
//   .then((docRef)=> {
//     //console.log("Document written with ID: ", docRef.id);
//   })
//   .catch((error)=> {
//     console.error('Error adding document: ', error)
//   })
//  }
// }
//}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-image:url("https://images.pexels.com/photos/2199293/pexels-photo-2199293.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940");
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
