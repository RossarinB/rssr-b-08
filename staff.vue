<template>
  <div>
    <link href="https://fonts.googleapis.com/css?family=Athiti&display=swap" rel="stylesheet" />
    <font face="Athiti, sans-serif">
      <b-card no-body>
        <b-tabs card>
          <b-tab title="ข้อมูลลูกค้า">
            <b-table striped hover :items="items" outlined></b-table>
          </b-tab>
          <b-tab title="เช็คสินค้า">
            <label>
              <p class="label-txt">ENTER YOUR USERNAME</p>
              <input type="text" class="input" name="username" v-model="username" />
              <div class="line-box">
                <div class="line"></div>
              </div>
            </label>
            <div class="container mt-4">
              <table class="table table-bordered mt-4">
                <thead class="thead-light">
                  <tr>
                    <th width="1">#</th>
                    <th width="10%">ชื่อห้อง</th>
                    <th width="10%">รายการ</th>
                    <th width="10%">คำอธิบาย</th>
                    <th width="10%">จำนวน</th>
                    <th width="10%">กว้าง</th>
                    <th width="10%">ยาว</th>
                    <th width="10%">สูง</th>
                    <th width="10%">ขนาดกล่อง</th>
                    <th width="10%">ราคา</th>
                    <th width="150">Toal</th>
                    <th width="150"></th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, index) in item" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.code }}</span>
                      <span v-if="editIndex === index">
                        <input class="form-control form-control-sm" v-model="item.code" />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.name }}</span>
                      <span v-if="editIndex === index">
                        <input class="form-control form-control-sm" v-model="item.name" />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.description }}</span>
                      <span v-if="editIndex === index">
                        <input class="form-control form-control-sm" v-model="item.description" />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.qty }}</span>
                      <span v-if="editIndex === index">
                        <input
                          class="form-control form-control-sm"
                          type="number"
                          v-model.number="item.qty"
                        />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.width }}</span>
                      <span v-if="editIndex === index">
                        <input
                          class="form-control form-control-sm"
                          type="number"
                          v-model.number="item.width"
                        />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.long }}</span>
                      <span v-if="editIndex === index">
                        <input
                          class="form-control form-control-sm"
                          type="number"
                          v-model.number="item.long"
                        />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.high }}</span>
                      <span v-if="editIndex === index">
                        <input
                          class="form-control form-control-sm"
                          type="number"
                          v-model.number="item.high"
                        />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.box }}</span>
                      <span v-if="editIndex === index">
                        <input
                          class="form-control form-control-sm"
                          type="number"
                          v-model.number="item.box"
                        />
                      </span>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">{{ item.price }}</span>
                      <span v-if="editIndex === index">
                        <input
                          class="form-control form-control-sm"
                          type="number"
                          v-model.number="item.price"
                        />
                      </span>
                    </td>
                    <td>
                      <div class="text-right">{{ subtotal(item) | money }}</div>
                    </td>
                    <td>
                      <span v-if="editIndex !== index">
                        <button
                          @click="edit(item, index)"
                          class="btn btn-sm btn-outline-secondary mr-2"
                        >Edit</button>
                        <button
                          @click="remove(item, index)"
                          class="btn btn-sm btn-outline-secondary mr-2"
                        >Remove</button>
                      </span>
                      <span v-else>
                        <button
                          class="btn btn-sm btn-outline-secondary mr-2"
                          @click="cancel(item)"
                        >Cancel</button>
                        <button
                          class="btn btn-sm btn-outline-secondary mr-2"
                          @click="save(item)"
                        >Save</button>
                      </span>
                    </td>
                  </tr>
                </tbody>
              </table>

              <div class="col-3 offset-9 text-right my-3">
                <button
                  v-show="!editIndex"
                  @click="add()"
                  class="btn btn-sm btn-outline-secondary"
                >Add item</button>
              </div>
              <div>
                <b-form-group label="ประเภทและจำนวนของรถขนย้าย:">
                  <b-form-checkbox-group
                    id="checkbox-group-1"
                    v-model="selected"
                    :options="options"
                    name="flavour-1"
                  ></b-form-checkbox-group>
                </b-form-group>
              </div>
              <div class="col-3 offset-9">
                <div class="input-group input-group-sm mb-3">
                  <div class="input-group-prepend">
                    <span class="input-group-text">Sub total</span>
                  </div>
                  <input
                    class="form-control form-control-sm text-right"
                    disabled
                    :value="this.allSubTotal | money"
                  />
                </div>

                <div class="input-group input-group-sm mb-3">
                  <div class="input-group-prepend">
                    <span class="input-group-text">Tax</span>
                  </div>
                  <input class="form-control form-control-sm" type="number" v-model.number="tax" />
                  <div class="input-group-append">
                    <span class="input-group-text">฿</span>
                  </div>
                </div>

                <div class="input-group input-group-lg mb-3">
                  <div class="input-group-prepend">
                    <span class="input-group-text">Total</span>
                  </div>
                  <input
                    class="form-control form-control-sm text-right"
                    disabled
                    :value="this.total | money"
                  />
                </div>
              </div>
            </div>
           <b-modal id="modal-center" centered title="Success">
              <p class="my-4">Add Data Success!</p>
            </b-modal>
            <center>
              <b-link :to="{ path: '/kijhi/user/status'}">
                <button
                  type="submit"
                  @click="addData()"
                  class="btn btn-outline-success my-2 my-lg-0"
                  v-b-modal.modal-center
                >SUBMIT</button>
              </b-link>
            </center>
          </b-tab>
          <b-tab title="ยืนยันการทำงาน">
            <VueSignaturePad width="500px" height="500px" ref="signaturePad" />
            <div>
              <button @click="saveimg()" class="btn btn-outline-success my-2 my-sm-0">Save</button>
              <button @click="undoimg()" class="btn btn-outline-danger my-2 my-sm-0">Undo</button>
              <button @click="uploadimg()" class="btn btn-outline-info my-2 my-sm-0">Upload</button>
            </div>
          </b-tab>
        </b-tabs>
      </b-card>
    </font>
  </div>
</template>
<script>
import { db } from '@/plugins/firebaseDB.js'
import Vue from 'vue'
import VueSignaturePad from 'vue-signature-pad'
import { storageRef } from '@/plugins/firebaseDB.js'
Vue.use(VueSignaturePad)
export default {
  name: 'Uhuy',

  filters: {
    money: value =>
      new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR'
      }).format(value)
  },
  data() {
    
    db.collection('MemberData')
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          // doc.data() is never undefined for query doc snapshots
          this.items.push(doc.data())
        })
      })
    return {
      username: '',
      selected: [], // Must be an array reference!
      options: [
        { text: 'Pickup 2000', value: 'Pickup' },
        { text: 'Pickup Container 3000', value: 'Pickup Container' },
        { text: 'Container 5000', value: 'Container' }
      ],
      editIndex: null,
      originalData: null,
      item: [
        {
          code: 'ห้องครัว',
          name: 'เตาแก๊ส',
          description: 'ห้องครัวชั้น1',
          qty: 2,
          width: 20,
          long: 20,
          high: 80,
          box: 0,
          price: 10
        }, // unit: null,discount: 0,
        {
          code: 'ห้องครัว',
          name: 'เตาแก๊ส',
          description: 'ห้องครัวชั้น1',
          qty: 2,
          width: 0,
          long: 0,
          high: 0,
          box: 220,
          price: 0
        }
      ],
      tax: 2900,
      items: [],
      images: [],
      file: null,
      imageUrl: null,
      allfile: []
    }
  },
  name: 'MySignaturePad',
  methods: {
    addData() {
      this.$root.$emit('bv::add::data', 'modal-center', '#btnShow')
      db.collection('Staff')
        .doc(this.username)
        .set({
          username: this.username,
          data: this.item,
          total: this.total,
          car: this.selected
        })
        .then(docRef => {
          console.log('Document written with ID: ', docRef.id)
        })
        .catch(error => {
          console.error('Error adding document: ', error)
        })
    },
    add() {
      this.originalData = null
      this.item.push({
        code: '',
        name: '',
        description: '',
        qty: 1,
        width: 0,
        long: 0,
        high: 0,
        box: 220,
        price: 0
      }) 
      this.editIndex = this.items.length - 1
    },

    edit(item, index) {
      this.originalData = Object.assign({}, item)
      this.editIndex = index
    },

    cancel(item) {
      this.editIndex = null

      if (!this.originalData) {
        this.item.splice(this.item.indexOf(item), 1)
        return
      }

      Object.assign(item, this.originalData)
      this.originalData = null
    },

    remove(item, index) {
      this.item.splice(index, 1)
    },

    save(item) {
      this.originalData = null
      this.editIndex = null
    },

    subtotal(item) {
      return (
        item.qty *
          ((item.price * (item.width * item.long * item.high)) / 5000) +
        item.qty * item.box
      )
    },
    uploadimg() {
      var uploadTask = storageRef
        .child('recieptMember/' + this.images) //เปลี่ยนเป็นรหัสไอดีอรหัสบัตรประชาชน
        .put(this.images)
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
      var listRef = storageRef.child('recieptMember/')

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
              this.images.push({
                fullPath
              })
            })
          })
        })
        .catch(error => {
          // Uh-oh, an error occurred!
        })
    },
    undoimg() {
      this.$refs.signaturePad.undoSignature(this.images)
    },
    saveimg() {
      const { isEmpty, data } = this.$refs.signaturePad.saveSignature(
        this.images
      )
      const { img } = this.$refs.signaturePad.getPropImagesAndCacheImages(
        this.images
      )

      console.log(isEmpty)
      console.log(data)
    }
  },
  computed: {
    allSubTotal() {
      return this.item
        .map(item => this.subtotal(item))
        .reduce((a, b) => a + b, 0)
    },

    total() {
      return this.tax ? this.allSubTotal + this.tax : this.allSubTotal
    }
  }
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
