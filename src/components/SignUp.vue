<template>
  <img src="../assets/logo-vertical.svg" alt="logo ohmytasks!" class="" width="200" height="200" />
  <h5 class=" ">Sign up and get started today</h5>
  <form @submit.prevent="signUp">
    <label class="" for="form3Example8">Email</label>
    <div class="">
      <input type="email" id="form3Example8" class="" v-model="email" required />
    </div>
    <label class="" for="form3Example5">Password</label>
    <div class="">
      <input v-model="password" class="" :type="passwordFieldType" id="exampleInputPassword5" required />
      <div class="">
        <i v-if="!hidePassword" @click="hidePassword = !hidePassword" class="fa-solid fa-eye"></i>
        <i v-else @click="hidePassword = !hidePassword" class="fa-solid fa-eye-slash"></i>
      </div>
    </div>
    <label class="" for="exampleInputPassword2">Confirm password</label>
    <div class="">
      <input v-model="confirmPassword" class="" :type="passwordFieldType" id="exampleInputPassword2" required />
      <div class="">
        <i v-if="!hidePassword" @click="hidePassword = !hidePassword" class="fa-solid fa-eye"></i>
        <i v-else @click="hidePassword = !hidePassword" class="fa-solid fa-eye-slash"></i>
      </div>
    </div>
    <p v-if="errorMsg" class="" role="alert">{{ errorMsg }}</p>
    <button type="submit" class="">
      Sign Up
    </button>
    <div>
      <p>Have an account?
        <PersonalRouter :route="route" :buttonText="buttonText" />
      </p>
    </div>
  </form>
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";

// Route Variables
const route = "/auth/login";
const buttonText = "Sign In";
// Input Fields
const email = ref("");
const password = ref("");
const confirmPassword = ref("");
// Error Message
const errorMsg = ref("");
const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);
const hidePassword = ref(true);
const redirect = useRouter();
const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      await useUserStore().signUp(email.value, password.value);
      redirect.push({ path: "/auth/login" });
    }
    catch (error) {
      errorMsg.value = "The email or password you entered is incorrect, please try again.";
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "Invalid token";
};
</script>

<style>

</style>
