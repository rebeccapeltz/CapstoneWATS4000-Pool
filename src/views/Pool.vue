<template>
  <div id="pool" class="container">
    <div class="jumbotron">
      <h1 class="display-">Baby Pool</h1>
      <p class="lead">When do you think the baby will be born? Enter below!</p>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Enter Contest</h3>
      </div>
      <div class="form-row">
       
        <div class="col">
          <input type="text" class="form-control" placeholder="Your name"
          id="entryContestant" v-model="newEntry.contestant">
        </div>
         <div class="col">
          <input type="text" v-model="newEntry.email" class="form-control" placeholder="Email">
        </div>
         <div class="col">
          <input type="text" v-model="newEntry.weight" class="form-control" placeholder="Guess Weight">
        </div>
          <div class="col">
          <input type="text" class="form-control" v-model="newEntry.babyname" placeholder="Guess Name">
        </div>
      </div>
      <div class="panel-body">
        <form id="form" class v-on:submit.prevent="addEntry">
          <!-- <div class="form-group">
            <label for="entryContestant">Your name:</label>
            <input
              type="text"
              id="entryContestant"
              class="form-control"
              v-model="newEntry.contestant"
            >
          </div> -->
          <!-- <div class="form-group">
            <label for="entryEmail">Email:</label>
            <input type="text" id="entryTitle" class="form-control" v-model="newEntry.email">
          </div>
          <div class="form-group">
            <label for="bookUrl">Guess Date:</label>
            <input type="text" id="entryDate" class="form-control" v-model="newEntry.date">
          </div>
          <div class="form-group">
            <label for="entryWeight">Guess Weight:</label>
            <input type="text" id="entryWeight" class="form-control" v-model="newEntry.weight">
          </div>
          <div class="form-group">
            <label for="entryBabyname">Guess Name:</label>
            <input type="text" id="entryBabyname" class="form-control" v-model="newEntry.babyname">
          </div> -->
          <input type="submit" class="btn btn-primary" value="Add Entry">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Entries</h3>
      </div>
      <div class="panel-body">
        <div class="table-responsive">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Name</th>
                <th>Email</th>
                <th>Date</th>
                <th>Weight</th>
                <th>Baby's Name</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(entry,index) in entries" :key="index">
                <td>{{entry.contestant}}</td>
                <td>{{entry.email}}</td>
                <td>{{entry.date}}</td>
                <td>{{entry.weight}}</td>
                <td>{{entry.babyname}}</td>
                <td>
                  <span class="far fa-trash-alt" aria-hidden="true" v-on:click="removeEntry(entry)"></span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";
import toastr from "toastr";
let config = {
  apiKey: "AIzaSyCQphEKrAx7M6kCA0wqlj2QICzcrycLpeA",
  authDomain: "birthdate-pool.firebaseapp.com",
  databaseURL: "https://birthdate-pool.firebaseio.com",
  projectId: "birthdate-pool",
  storageBucket: "birthdate-pool.appspot.com",
  messagingSenderId: "838139933356"
};

let app = firebase.initializeApp(config);
let db = app.database();
let entriesRef = db.ref("entries");

export default {
  name: "pool",
  firebase: {
    entries: entriesRef
  },

  data() {
    return {
      newEntry: {
        contestant: "",
        email: "",
        date: "",
        weight: "",
        babyname: ""
      }
    };
  },

  methods: {
    addEntry: function() {
      entriesRef.push(this.newEntry);
      this.newEntry.contestant = "";
      this.newEntry.email = "";
      this.newEntry.date = "";
      this.newEntry.weight = "";
      this.newEntry.babyname = "";
    },
    removeEntry: function(entry) {
      entriesRef.child(entry[".key"]).remove();
      toastr.success("Entry removed successfully");
    }
  }
};
</script>
<style>
#pool {
  font-family: "Handlee", cursive;
  /* 'Avenir', Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0c2238;
  margin-top: 20px;
  background-color: #fff;
}

.jumbotron {
  text-align: center;
  font-family: "Handlee", cursive;
  background-color: #f8ab99;
  color: #400040;
  font-weight: 575;
  border-color: #400040;
}

.btn-primary {
  background-color: #6ee2b8;
  color: rgb(27, 13, 65);
  border-color: #d0d3d2;
  font-weight: 575;
}

.fa-trash-alt {
  cursor: pointer;
}

.lead {
  font-weight: 575;
}

h1 {
  font-weight: 600;
}

h3 {
  font-weight: 600;
}
.btn {
  margin: 1rem 0;
}
</style>