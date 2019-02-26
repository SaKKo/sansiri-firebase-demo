<template>
  <v-layout
    justify-center
    align-center
  >
    <v-flex>
      <v-card ref="form">
        <v-card-text>
          <v-text-field
            ref="email"
            v-model="email"
            label="email"
            required
          ></v-text-field>
          <v-text-field
            type="password"
            ref="password"
            v-model="password"
            label="password"
            required
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn flat>Cancel</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="signinFacebook">Facebook</v-btn>
          <v-btn color="primary" @click="signup">Signup</v-btn>
          <v-btn color="primary" @click="signin">Signin</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import firebase from "firebase";

export default {
  data: () => ({
    email: "saklism@gmail.com",
    password: "qwertyuiop"
  }),
  mounted() {
    let CONFIG = {
      apiKey: "AIzaSyASeYCEkc8TITvGcwF3hsvlOPN31U2QRYY",
      authDomain: "geekstart-website.firebaseapp.com",
      databaseURL: "https://react-notes-38f8a.firebaseio.com"
    };
    firebase.initializeApp(CONFIG);
    firebase
      .auth()
      .getRedirectResult()
      .then(
        result => {
          console.log("RESULT", result);
          let user = result.user;
          let credential = result.credential;
          let operationType = result.operationType;
        },
        error => {
          console.log("ERROR", error);
          let email = error.email;
          let credential = error.credential;
          if (error.code === "auth/account-exists-with-different-credential") {
            auth.fetchProvidersForEmail(email).then(function(providers) {});
          }
        }
      );
  },
  methods: {
    signin() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .catch(function(error) {
          // Handle Errors here.
          var errorCode = error.code;
          var errorMessage = error.message;
          // ...
        })
        .then(resp => {
          console.log("RESP", resp);
        });
    },
    signup() {
      firebase
        .auth()
        .createUserWithEmailAndPassword(this.email, this.password)
        .catch(error => {
          // Handle Errors here.
          var errorCode = error.code;
          var errorMessage = error.message;
          // ...
        });
    },
    signinFacebook() {
      let provider = new firebase.auth.FacebookAuthProvider();
      provider.addScope("email");
      firebase.auth().signInWithRedirect(provider);
    }
  }
};
</script>
