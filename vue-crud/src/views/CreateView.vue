<template>
    <div class="q-pa-md" style="max-width: 400px">
         <q-form
              @submit="onSubmit"
              class="q=gutter=md"
         >
             <q-input v-model="first_Name" label="First Name" />
             <q-input v-model="last_Name" label="Last Name" />
             <q-input v-model="email" label="Email" />
             <q-input v-model="address" label="Address" />
             <q-input v-model="phone_number" label="Phone Number" />
             <q-btn label="Submit" type="submit" color="primary"/>
         </q-form>
    </div>
 </template>
 
 <script setup>
 import { ref } from 'vue'
 import router from '@/router';  // แก้จาก 'route' เป็น 'router' ให้ตรงกับการใช้งานจริง
 
 const first_Name = ref('')
 const last_Name = ref('')
 const email = ref('')
 const address = ref('')
 const phone_number = ref('')
 
 const onSubmit = () => {
     const myHeaders = new Headers();
     myHeaders.append("Content-Type", "application/json");
 
     const raw = JSON.stringify({
         first_name: first_Name.value,
         last_name: last_Name.value,
         email: email.value,
         address: address.value,
         phone_number: phone_number.value
     });
 
     const requestOptions = {
         method: "POST",
         headers: myHeaders,
         body: raw,
         redirect: "follow"
     };
 
     fetch("http://localhost:8800/api/v1/customers", requestOptions)
       .then((response) => response.json())  // เปลี่ยนจาก text() เป็น json()
       .then((result) => {
           console.log(result);
           alert(result.message);
           if (result === "ok") {  // แก้เป็น if statement ที่ถูกต้อง
               router.push('/');
           }
       })
       .catch((error) => console.error('Error:', error));
 }
 </script>
 
