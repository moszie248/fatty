<template>
  <div class="hello">
    <section class="hero head is-medium is-bold">
      <div class="hero-body">
        <router-link tag="li" to="/">
          <a><img src="../assets/logo.png" class="logo"></a>
        </router-link>
        <router-link tag="li" to="/profile">
          <a>profile</a>
        </router-link>
        <button class="button is-danger" @click="showAddProfile()">test</button>
        <div class="columns is-centered">
          <div class="column is-desktop-7 is-mobile-10 is-narrow">
            <b-field lable="search">
                <b-input v-model="keyword"
                placeholder="อาหารที่ต้องการค้นหา"
                ></b-input>
                <button class="button is-danger" @click="search()">search</button>
            </b-field>
          </div>
        </div>
      </div>
    </section>
    <div class="columns">
      <div class="column is-8 is-offset-2">
        <foodspanal :foods="foods"></foodspanal>
      </div>
    </div>
<b-modal :active.sync="isModalActive">
      <div class="card card-content">
        <div class="title columns is-centered">
         กรอกข้อมูลส่วนตัว
        </div>

        <div class="columns is-centered">
          <div class="column is-7">
            <b-field label="ชื่อ">
          <b-input type="number" placeholder="ชื่อ" v-model="subid"></b-input>
        </b-field>
          </div>
          <div class="column">
          <b-field label="อายุ">
          <b-input type="number" placeholder="อายุ" v-model="subid"></b-input>
        </b-field>
        </div>
        </div>

      <div class="columns">
        <div class="column">
            <b-field label="เพศ">
          <div class="block">
            <b-radio v-model="radio"
                native-value="male">
                ชาย
            </b-radio>
            <b-radio v-model="radio"
                native-value="female">
                หญิง
            </b-radio>
        </div>
        </b-field>
        </div>
      </div>

      <div class="columns">
        <div class="column">
          <b-field label="น้ำหนัก">
          <b-input type="number" placeholder="น้ำหนัก" v-model="subid"></b-input>
        </b-field>
        </div>
        <div class="column">
          <b-field label="ส่วนสูง">
          <b-input type="number" placeholder="ส่วนสูง" v-model="subid"></b-input>
        </b-field>
        </div>
      </div>

        <div class="columns is-centered">
          <button class="button is-success " @click="addSub(subid, subname, unit, grade)">ตกลง</button>
        </div>
      </div>
    </b-modal>

  </div>
</template>

<script>
import axios from 'axios'
import { mapGetters, mapActions } from 'vuex'
import foodspanal from './foodspanal'
export default {
  name: 'HelloWorld',
  components: { foodspanal },
  data () {
    return {
      isModalActive: false,
      foods: [],
      keyword: '',
      Cart: [],
      tmp: '',
      name: '',
      sex: '',
      age: '',
      weight: '',
      height: ''
    }
  },
  mounted () {
    this.getAllMembers()
  },
  methods: {
    ...mapActions([
      'storeCart'
    ]),
    showSeemore (food) {
      if (this.tmp === food.Food_ID) {
        this.tmp = ''
      } else {
        this.tmp = food.Food_ID
      }
    },
    search () {
      let self = this
      axios.get('//fatty-db.herokuapp.com?crud=search&key=' + this.keyword).then(function (response) {
        self.foods = response.data.foods
      })
    },
    getAllMembers: function () {
      let self = this
      axios.get('//fatty-db.herokuapp.com/index.php').then(function (response) {
        self.foods = response.data.foods
      })
    },
    showAddProfile () {
      this.isModalActive = true
    }
  },
  computed: {
    ...mapGetters([
      'cart'
    ]),
    sumCal () {
      var sum = 0
      for (var i = 0; i < this.cart.length; i++) {
        sum += parseInt(this.cart[i].Energy)
      }
      return sum
    }
  }
  // created () {
  //   document.body.style.background = "#0001 url('/static/24840.jpg')no-repeat right top"
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.name {
  margin-right: 30px;
}
.bg {

   background: url(/static/24840.jpg) no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
.fatty{
  font-family: 'Poiret One', cursive;
  color: aliceblue;
  font-size: 100px;
}

.logo {
  height: 80px;
}
.hero.head {

  background-image: url('../assets/24840.jpg');
  background-size: cover;
}
</style>
