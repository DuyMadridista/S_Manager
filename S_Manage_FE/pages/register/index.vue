<template>
  <section class="flex items-center justify-center h-screen bg-background">
    <div class="w-full max-w-md">
      <a href="#" class="flex items-center justify-center mb-6 text-2xl font-semibold text-primary">
        <img class="w-8 h-8 mr-2" src="../../public/losdac.png" alt="logo">
        S-Manage
      </a>
      <div class=" rounded-lg shadow-2xl bg-surface">
        <div class="flex items-center w-20 cursor-pointer p-2">
      <font-awesome-icon :icon="['fas', 'chevron-left']" />
      <button class="btn-SignUp hover:opacity-90 w-10 text-[04364A] font-bold border-b-2 border-[04364A] text-left ml-1 "
        @click="goBack()">
        Back
      </button>
    </div>

        <div class="p-6 space-y-4">
          <h1 class="text-2xl font-bold leading-tight tracking-tight text-center text-gray-900">
            Sign up
          </h1>

          <form @submit="register()" class="space-y-4" action="#">

            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-secondary">Name</label>
              <input type="email" name="email" id="email"
                class="w-full p-2.5 rounded-lg focus:ring-primary-600 focus:border-primary-600 text-secondary  "
                v-model="name" placeholder="Name" required>
                
            </div>

            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-secondary">Age</label>
              <input v-model="age" type="number"
                class="w-full px-4 py-2 border rounded focus:outline-none focus:border-blue-500" required />
            </div>
            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-secondary">Gender</label>
              <select v-model="gender" class="w-full px-4 py-2 border rounded focus:outline-none focus:border-blue-500">
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Khác</option>
              </select>

            </div>
            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-secondary">Username</label>
              <input type="email" name="email" id="email"
                class="w-full p-2.5 rounded-lg focus:ring-primary-600 focus:border-primary-600 text-secondary  "
                v-model="username" placeholder="Username" required>
            </div>
            <div>
              <label for="password" class="block mb-2 text-sm font-medium text-secondary">Password</label>
              <input type="password" name="password" id="password" placeholder="••••••••"
                class="w-full p-2.5 rounded-lg focus:ring-primary-600 focus:border-primary-600 " required
                v-model="password">
            </div>
            <div>
              <label for="email" class="block mb-2 text-sm font-medium text-secondary">Email</label>
              <input type="email" name="email" id="email"
                class="w-full p-2.5 rounded-lg focus:ring-primary-600 focus:border-primary-600 text-secondary  "
                v-model="email" placeholder="Email"  @blur="validateEmail" required>
                <span v-if="!isEmailValid" class="text-red-500">Invalid email format</span>
            </div>

            <v-btn @click="register" block variant="outlined" size="x-large" class="mb-4 bg-primary"> Sign Up </v-btn>

          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import axios from 'axios';
import { useToast } from 'vue-toastification'
const toast = useToast()

import validator from 'validator';

const isEmailValid = ref(true);

const router = useRouter()

const config = useRuntimeConfig();
const URL_BE = config.public.API_BASE_BE;

const username = ref('')
const password = ref('')
const email = ref('')
const name = ref('')
const age = ref('')
const gender = ref('')

const goBack = () => {
  router.push("/");
};

const validateEmail = () => {
  isEmailValid.value = validator.isEmail(email.value);
};


const register = async () => {

  try {
    const response = await axios.post(`${URL_BE}/api/v1/auth/register`, {
      username: username.value,
      password: password.value,
      name: name.value,
      gender: gender.value,
      email: email.value,
      age: parseInt(age.value)
    })
    if (response.data) {
      toast.success('Register successfully')
      router.push("/");
    }
  } catch (error) {
    console.log(error)
    toast.error('Register failed')

  }


}

</script>

<style scoped>
input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
