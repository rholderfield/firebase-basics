<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import HelloWorld from "@/components/HelloWorld.vue";

import { firebaseConfig } from "../store/firebaseConfig";
import { initializeApp } from "firebase/app";
import { getAuth, onAuthStateChanged } from "firebase/auth";
import { getFirestore, doc, setDoc } from "firebase/firestore";
import { Currency, getAnalytics } from "firebase/analytics";

const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);

const firestore = getFirestore();

const specialOfTheDay = doc(firestore, "dailySpecial/2021-09-14");

interface docDataValidation {
  description: string;
  price: Currency;
  milk: string;
  vegan: boolean;
}

function writeDailySpecial() {
  const docData: docDataValidation = {
    description: "A vanilla latte",
    price: 3.99,
    milk: "Whole",
    vegan: false,
  };
  setDoc(specialOfTheDay, docData);
}

writeDailySpecial();

const auth = getAuth(app);

onAuthStateChanged(auth, (user) => {
  if (user) {
    console.log("logged in");
  } else {
    console.log("no user");
  }
});

@Options({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {}
</script>
