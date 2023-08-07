<template>
  <section class="flex h-screen w-full">
    <div class="w-1/2" v-if="!currentUser" style="background-image: url(https://images.unsplash.com/photo-1690555575753-7aa27df96b52?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=774&q=80); background-repeat: no-repeat; background-size: cover;">
      <h1 class="mb-5 text-2xl mt-10 ml-10 text-white">Assignment Module 6!</h1>
    </div>
    <!-- welcome page -->
    <section class="w-full content-center ml-80 pl-40 mt-8" v-if="currentUser">
      <div class="max-w-sm rounded overflow-hidden shadow-lg" >
        <div class="px-6 py-4">
          <p class="text-green-600 bolder" v-if="successMessage">
            {{ successMessage }}
          </p>
          
          <div class="font-bold text-xl mb-2">Welcome {{ currentUser }}!</div>
          <h3>In Our Authentcate system</h3>
        </div>
        <div class="px-6 pt-4 pb-2">
            <button class="bg-red-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" @click="logout">
              Logout
            </button>
        </div>
      </div>
      
    </section>
   <!-- end welcome page -->
    
    <div class="w-1/2 flex flex-col justify-center items-center bg-gray-200" v-if="!currentUser">      
      <div class="w-full max-w-xs">
        <!-- start registration section -->
        <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" v-if="showRegisterForm" @submit.prevent="register">
          <h2 class="mb-5 text-xl text-center">Register</h2>
          <p class="text-red-600 bolder" v-if="errorMessage">
            {{ errorMessage }}
          </p>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="name">
              Name
            </label>
            <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" v-model="registration.name" id="name" type="text" placeholder="Name">
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
              Username <strong class="text-red-600">*</strong>
            </label>
            <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" v-model="registration.username" id="username" type="text" placeholder="Username">
            <p class="error-message" v-if="userNameValidationError">Username is required.</p>
      
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
              Password <strong class="text-red-600">*</strong>
            </label>
            <input class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" v-model="registration.password" type="password" placeholder="*********">   
            <p class="error-message" v-if="passwordValidationError">Password is required.</p>
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
              Confirm Password <strong class="text-red-600">*</strong>
            </label>
            <input class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" v-model="registration.confirmPassword" type="password" placeholder="*********">
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="flex items-center justify-between">
          
            <button class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
              Register
            </button>           
            <button class="inline-block align-baseline font-bold text-sm text-orange-600 hover:text-orange-800" @click="toggleForm">
              {{ showRegisterForm ? 'Login' : 'Register' }}
            </button>
          </div>
        </form>
        <!-- end registration section -->

        <!-- start login section -->
        <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" v-else @submit.prevent="loginUser">
          <h2 class="mb-5 text-xl text-center">Login</h2>
          <p class="text-red-600 bolder" v-if="errorMessage">
            {{ errorMessage }}
          </p>

          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
              Username <strong class="text-red-600">*</strong>
            </label>
            <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" v-model="login.username" type="text" placeholder="Username">
            <p class="error-message" v-if="loginUserNameValidationError">Username is required.</p>
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
              Password <strong class="text-red-600">*</strong>
            </label>
            <input class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" v-model="login.password" type="password" placeholder="*********">
            <p class="error-message" v-if="loginPasswordValidationError">Password is required.</p>
          </div>
          <div class="flex items-center justify-between">
                               
            <button class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
              Login
            </button>
           
            <button class="inline-block align-baseline font-bold text-sm text-orange-600 hover:text-orange-800" @click="toggleForm">
              {{ showRegisterForm ? 'Login' : 'Register' }}
            </button>
          </div>
        </form>
        <!-- end login section -->
        <p class="text-center text-gray-500 text-xs">
          &copy;2023 Acme Corp. All rights reserved.
        </p>
      </div>
         
    </div>
     
  </section>
</template>


<script setup>
import { ref } from 'vue';

const users = ref([]);
const currentUser = ref(null);
// start validation messages
const successMessage = ref('');
const errorMessage = ref('');

const userNameValidationError = ref(false);
const passwordValidationError = ref(false);

const loginUserNameValidationError = ref(false);
const loginPasswordValidationError = ref(false);
// end validation messages

const registration = ref({
  name: '',
  username: '',
  password: '',
  confirmPassword: '',
});

const login = ref({
  name: '',
  username: '',
  password: '',
});

const showRegisterForm = ref(true);

const register = () => {
  if(!registration.value){
    errorMessage.value = 'Input all flied';
  }
  if (registration.value.password !== registration.value.confirmPassword) {
    alert('Passwords & Confirm Password do not match.');
    return;
  }
  if (registration.value.username && registration.value.password) {
    users.value.push({
      name: registration.value.name,
      username: registration.value.username,
      password: registration.value.password,
    });
    currentUser.value = registration.value.username;
    successMessage.value = 'Registration successful!';
    userNameValidationError.value = false;
    passwordValidationError.value = false;
  }else{
    userNameValidationError.value = !registration.value.username;
    passwordValidationError.value = !registration.value.password;
  }
};

const loginUser = () => {
  const user = users.value.find(user => user.username === login.value.username && user.password === login.value.password);
  if (user) {
    currentUser.value = login.value.username;
    // for validation  message
    successMessage.value = 'Login successful!';
    loginUserNameValidationError.value = false;
    loginPasswordValidationError.value = false;
  }else{
    // for validation message
    if(login.value.username && login.value.password){
      errorMessage.value = 'Login failed. Please check your credentials.';
    }
    loginUserNameValidationError.value = !login.value.username;
    loginPasswordValidationError.value = !login.value.password;
  }
};

const logout = () => {
  currentUser.value = null;
};

const toggleForm = () => {
  showRegisterForm.value = !showRegisterForm.value;
};
</script>

<style scoped>
.error-message{
  color: red;
}

</style>
