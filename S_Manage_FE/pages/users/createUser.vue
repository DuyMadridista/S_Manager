
<script setup>

  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import { useRoute, useRouter } from 'vue-router';
  import { notify } from '@kyvg/vue3-notification';
  import displayIMG from '../../../middleware/displayIMG';
  


  definePageMeta({
        layout: 'admin'
    });

  const router = useRouter();
  const route = useRoute();
  const userId = route.params.id;
  
  const config= useRuntimeConfig();
  const URL_BE = config.public.API_BASE_BE;

  const dataRole = ref([]);

  

  const DataUser = ref({
    id_user: "",
    name: "",
    age: "",
    gender: "",
    email: "",
    username: "",
    password: "",
    avatar: "",
    status: "",
    nameRole: "",
    idRole: ""
  });

  const errorValue = ref({
    id_user: "",
    name: "",
    age: "",
    gender: "",
    email: "",
    username: "",
    password: "",
    avatar: "",
    status: ""
  });

  const validate = () => {
    let isValue = true;

    if (DataUser.value.name == "") {
      isValue = false;
      errorValue.value.name = "Name is required";
    } else {
      errorValue.value.name = "";
    }

    if (DataUser.value.age == "") {
      isValue = false;
      errorValue.value.age = "age is required";
    } else {
      errorValue.value.age = "";
    }

    if (DataUser.value.gender == "") {
      isValue = false;
      errorValue.value.gender = "gender is required";
    } else {
      errorValue.value.gender = "";
    }

    if (DataUser.value.email == "") {
      isValue = false;
      errorValue.value.email = "email is required";
    } else {
      errorValue.value.email = "";
    }
    return isValue;
  }

  const getRole = () => {
    axios
      .get(`${URL_BE}/api/v1/author/get_all_role`)
      .then((res) => {
        return dataRole.value = res.data
      })
      .catch((error) => {
        console.log(error);
      })
  }
  


  onMounted( async () => {
    displayIMG("imageInput", "imageDisplay");
    getRole();
  });


  const saveUser = () => {
    if(validate()){

      // Lấy tệp avatar từ input
      const avatarInput = document.getElementById('imageInput');
      const selectedAvatar = avatarInput.files[0];

      const data = DataUser.value;

      // Tạo FormData để chứa dữ liệu và tệp avatar
     const formData = new FormData();

     if (selectedAvatar) {
      formData.append('avatar', selectedAvatar);
      }

    for (const key in data) {
        formData.append(key, data[key]);
      }

      axios
        .post(`${URL_BE}/api/v1/users`, formData)
        .then((response) => {
          console.log(response);
          notify({
            type: 'success',
            title: 'Create success',
            text: 'Create success',
          });
          Cancel();
        })
        .catch((error) => {
          console.log(error);
        });
    }
  };

  const Cancel = () => {
    router.push('/users');
  }

</script>


<template>
 
  <div class="px-20 py-10">
    <div class="w-20  cursor-pointer flex items-center">
      <font-awesome-icon :icon="['fas', 'chevron-left']" />
      <button
        class="btn-SignUp hover:opacity-90 w-10 text-black font-bold border-b-2 border-black text-left ml-1 "
        @click="Cancel()"
      >
        Back
      </button>
    </div>

    <div class="flex ">

      <form  
          @submit.prevent="saveUser()"
          class=" w-full pr-20 "
          >
            <div class=" flex justify-between">
              <div  class=" w-3/5 ">
          
          <div class="input  p-2 mb-4 w-3/4 relative" >
            <label for="" class="text-base text-black font-semibold">Họ và tên:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="name" 
              v-model="DataUser.name"
              type="text" 
              placeholder="name"
              @blur="validate()"
                :class="{'is-invalid': errorValue.name}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.name" > {{errorValue.name }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>

          <div class="input  p-2 mb-4 w-3/4 relative" >
            <label for="" class="text-base text-black font-semibold">Tuổi:</label>
            <input 
              class=" bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="age" 
              v-model="DataUser.age"
              type="text" 
              placeholder="age"
              @blur="validate()"
                :class="{'is-invalid': errorValue.age}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.age" > {{errorValue.age }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>

          <div class="input  p-2 mb-4 w-3/4 relative" >
            <label for="" class="text-base text-black font-semibold">Giới tính:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="gender" 
              v-model="DataUser.gender"
              type="text" 
              placeholder="gender"
              @blur="validate()"
                :class="{'is-invalid': errorValue.gender}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.gender" > {{errorValue.gender }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>

          <div class="input  p-2 mb-4 w-3/4 relative" >
            <label for="" class="text-base text-black font-semibold">Số điện thoại:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="phone" 
              v-model="DataUser.phone"
              type="text" 
              placeholder="phone"
              @blur="validate()"
                :class="{'is-invalid': errorValue.phone}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.phone" > {{errorValue.phone }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>

          <div class="input  p-2 mb-4 w-3/4 hidden" >
            <label for="" class="text-base text-black font-semibold">Tên đăng nhập:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="username" 
              v-model="DataUser.username"
              type="text" 
              placeholder="username"
              @blur="validate()"
                :class="{'is-invalid': errorValue.username}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.username" > {{errorValue.username }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>

          <div class="input  p-2 mb-4 w-3/4 hidden" >
            <label for="" class="text-base text-black font-semibold">Password:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="password" 
              v-model="DataUser.password"
              type="text" 
              placeholder="password"
              @blur="validate()"
                :class="{'is-invalid': errorValue.password}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.password" > {{errorValue.password }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>


          <div class="input  p-2 mb-4 w-3/4 relative" >
            <label for="" class="text-base text-black font-semibold">Địa chỉ:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="address" 
              v-model="DataUser.address"
              type="text" 
              placeholder="address"
              @blur="validate()"
                :class="{'is-invalid': errorValue.address}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.age" > {{errorValue.age }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>


          <div class="input  p-2 mb-4 w-3/4 relative" >
            <label for="" class="text-base text-black font-semibold">Emai:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="email" 
              v-model="DataUser.email"
              type="text" 
              placeholder="email"
              @blur="validate()"
                :class="{'is-invalid': errorValue.email}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.email" > {{errorValue.email }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>

          <div class="input  p-2 mb-4 w-3/4 " >
            <label for="" class="text-base text-black font-semibold">Quyền truy cập:</label>
            <!-- <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="role" 
              v-model="DataUser.role"
              type="text" 
              placeholder="role"
              @blur="validate()"
                :class="{'is-invalid': errorValue.role}"
              /> -->

              <div>
                <select 
                  name="role" id="role" 
                  v-model="DataUser.idRole"
                  class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base"
                  >
                  <option selected disabled>Chọn quyền truy cập:</option>
                  <option 
                  v-for="role in dataRole.roles"
                  :key="role.id_role"
                  :value="role.id_role"
                  >{{ role.name_role }}</option>
                </select>
              </div>

              <div class="invalid-feedback text-left" v-if="errorValue.role" > {{errorValue.role }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer z-10"
                :icon="['fas', 'chevron-down']" />

          </div>

          <div class="input  p-2 mb-4 w-3/4 hidden" >
            <label for="" class="text-base text-black font-semibold">Trạng thái:</label>
            <input 
              class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base" 
              name="status" 
              v-model="DataUser.status"
              type="text" 
              placeholder="status"
              @blur="validate()"
                :class="{'is-invalid': errorValue.status}"
              />

              <div class="invalid-feedback text-left" v-if="errorValue.status" > {{errorValue.status }} </div>

              <font-awesome-icon 
                class="absolute right-6 top-12 text-xl cursor-pointer"
                :icon="['fas', 'pen-to-square']" />
          </div>
          
              </div>

              <div class="w-2/5 flex flex-col justify-start items-center">
                
                <div>
                  <img 
                  id="imageDisplay"
                  alt="avt"
                  class="w-[500px] h-[600px] rounded-3xl shadow-xl "
                  >

                  <input type="file" id="imageInput" name="avatar">
                </div>

                <div class="flex justify-center mt-6 w-3/4">
                  <button type="submit"  class="btn-SignIn hover:opacity-90 bg-[04364A] text-black font-bold py-2 px-10 rounded mr-8">
                    Submit
                  </button>
                </div>

              </div>
            </div>
            
        
      </form>
  
    </div>

  </div>

 

 
</template>


