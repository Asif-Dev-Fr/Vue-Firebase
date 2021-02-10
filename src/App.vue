<template>
  <div id="app">
    <h1>Survey</h1>
    <form class="form" @submit="submitForm">
      <div>
        <input type="text" v-model="name" placeholder="Enter your name" />
      </div>
      <div>
        <input type="text" placeholder="Enter your age" v-model.lazy="age" />
      </div>
      <div>
        <select v-model="nameCoach">
          <option value="default" selected>--Please choose an option--</option>
          <option value="jean">Jean</option>
          <option value="edouard">Edouard</option>
          <option value="jessica">Jessica</option>
          <option value="marie">Marie</option>
        </select>
      </div>
      <div>
        <textarea name="exerciceDone" v-model="exercices" id="" cols="30" rows="10"></textarea>
      </div>
      <div>
        <input type="text" v-model.number="rate" placeholder="Rate your coach" />
      </div>
      <div>
        Would you recommend this coach ?
        <div>
          <input type="radio" id="yes" name="yes" value="yes" v-model="willSuggest">
          <label for="yes">Yes</label>
        </div>
        <div>
          <input type="radio" id="no" name="no" value="no" v-model="willSuggest">
          <label for="no">No</label>
        </div>
      </div>
      <div>
        <button type="submit">
          Submit
        </button>
      </div>
      

    </form>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import firebase from 'firebase/app'
import '@firebase/firestore'
import firebaseConfig from './config/firebase';
import Vue from "vue";
const firebaseApp = firebase.initializeApp(firebaseConfig);
const db = firebaseApp.firestore()
Vue.prototype.$db = db;

export default {
  name: 'App',
  data() {
    return {
      name: '',
      age: 0,
      nameCoach: '',
      exercices: '',
      rate: 0,
      willSuggest: ''
    }
  },
  components: {
    // HelloWorld
  },
  methods: {
    submitForm(e) {
      e.preventDefault();
      // console.log(this.nameCoach);
      // Your web app's Firebase configuration
      const addSurvey = db.collection('survey');
      addSurvey.add({
        name: this.name,
        age: parseInt(this.age),
        nameCoach: this.nameCoach,
        exercices: this.exercices,
        rate: this.rate,
        willSuggest: this.willSuggest
      });
      this.name = "";
      this.age = "";
      this.nameCoach = "default";
      this.exercices = "";
      this.rate = ""
      this.willSuggest = false;

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
