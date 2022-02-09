<template>
  <div>
    <p>Auth</p>
    <input v-model="email" placeholder="email" /><br/>
    <input v-model="password" placeholder="password" /><br />
    <button @click="signUp">sign up</button><br />
    <button id="sign-in-button">sign in</button><br />
    <button @click="logout">logout</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { createUserWithEmailAndPassword, getAuth, RecaptchaVerifier, signInWithEmailAndPassword, signOut } from 'firebase/auth';
import { initializeApp } from 'firebase/app';

export default defineComponent({
  name: 'App',
  data() {
    return {
      email: '',
      password: '',
      recaptchaVerifier: {} as any,
    }
  },

  methods: {
    signUp() {
      console.log('sign up');
      const auth = getAuth();
      createUserWithEmailAndPassword(auth, this.email, this.password).then((userCredential) => {
        const user = userCredential.user;
        console.log(user);
      }).catch((err) => {
        console.log(err);
      })
    },

    signIn() {
      console.log('sign in');
      const auth = getAuth();
      
      signInWithEmailAndPassword(auth, this.email, this.password).then((userCredential) => {
        const user = userCredential.user;
        console.log(user);
      }).catch((err) => {
        console.log(err)
      })
    },

    logout() {
      console.log('logout');
      const auth = getAuth();
      signOut(auth).then(() => {
        console.log('logouted');
      }).catch((err) => {
        console.log(err);
      })
    }
  },

  created() {
    console.log('onCreated');
    const firebaseConfig = {

    };

    const app = initializeApp(firebaseConfig);
  },

  mounted() {
    console.log('on mouted');
    const auth = getAuth();
    this.recaptchaVerifier = window.recaptchaVerifier = new RecaptchaVerifier('sign-in-button', {
      'size': 'invisible',
      'callback': (response: any) => {
        console.log('aaaaaaa');
        // reCAPTCHA solved, allow signInWithPhoneNumber.
        console.log(response);
        this.signIn();
      }
    }, auth);
    this.recaptchaVerifier.render();
  },
});

declare global {
  interface Window {
    recaptchaVerifier?: any;
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
