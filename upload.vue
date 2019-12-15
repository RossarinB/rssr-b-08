<template>
  <div>
    <form class="from-group">
       <label>
          <p class="label-txt">USERNAME</p>
          <input type="username" class="input" name="username" v-model="username" />
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
        <p class="label-txt">DATE</p>
        <input type="date" class="input" name="date" v-model="date" />
        <div class="line-box">
          <div class="line"></div>
        </div>
      </label>
      <label>
        <p class="label-txt">TIME</p>
        <input type="time" class="input" name="time" v-model="time" />
        <div class="line-box">
          <div class="line"></div>
        </div>
      </label>
      <label>
        <p class="label-txt">เลขที่ 4 ตัวท้ายบัญชี</p>
        <input type="text" class="input" name="num" v-model="num" />
        <div class="line-box">
          <div class="line"></div>
        </div>
      </label>
      <label>
        <p class="label-txt">ธนาคาร</p>
        <input type="text" class="input" name="bank" v-model="bank" />
        <div class="line-box">
          <div class="line"></div>
        </div>
      </label>
      <label>
        <!-- <b-form-file
          multiple
          :file-name-formatter="formatNames"
          v-model="file"
          accept="images/*"
          style="outline-success"
          show-size
        ></b-form-file>-->

        <b-form-file plain v-model="file" accept="images/*" style="outline-success" show-size></b-form-file>
      </label>
       <b-modal id="modal-center" centered title="Success">
          <p class="my-4">Upload Success!</p>
        </b-modal>
      <b-button @click="upload()" variant="outline-success" >Uploade</b-button>
    </form>
    <!-- <v-file-input v-model="file" accept="images/*" label="File input" show-size></v-file-input> -->
    <!-- <b-button @click="listfile()" color="primary" variant="outline-info">List All File</b-button> -->
  </div>
</template>
<script>
import { db } from '@/plugins/firebaseDB.js'
import { storageRef } from '@/plugins/firebaseDB.js'
export default {
  data() {
    return {
      username:'',
      email:'',
      date: '',
      time: '',
      num: '',
      bank: '',
      file: null,
      imageUrl: null,
      allfile: []
    }
  },
  methods: {
    formatNames(files) {
      if (files.length === 1) {
        return files[0].name
      } else {
        return `${files.length} files selected`
      }
    },
    upload() {
      this.$root.$emit('bv::add::data', 'modal-center', '#btnShow')
      //let id = new Date()
      db.collection('MemberData')
        .doc(this.email)
        .add({
          username: this.username,
          email: this.email,
          num: this.num,
          bank: this.bank,
          time: this.time,
          date: this.date,
         
        });
      var uploadTask = storageRef
        .child('images/' + this.file.name) //เปลี่ยนเป็นรหัสไอดีอรหัสบัตรประชาชน
        .put(this.file)
        .then(snapshot => {
          // use the Blob or File API
          console.log('Uploaded a blob or file!')
        })
        .catch(error => {
          console.error('Error adding document: ', error)
        })
    },
    listfile() {
      // Create a reference under which you want to list
      var listRef = storageRef.child('images/')

      // Find all the prefixes and items.
      listRef
        .listAll()
        .then(res => {
          // res.prefixes.forEach((folderRef)=> {
          // All the prefixes under listRef.
          // You may call listAll() recursively on them.
          //})
          res.items.forEach(itemRef => {
            console.log(itemRef.fullPath)
            // All the items under listRef.
            itemRef.getDownloadURL().then(url => {
              // Insert url into an <img> tag to "download"
              this.allfile.push({
                fullPath
              })
            })
          })
        })
        .catch(error => {
          // Uh-oh, an error occurred!
        })
    }
  }
}
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
