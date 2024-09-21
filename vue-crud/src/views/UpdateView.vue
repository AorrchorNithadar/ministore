<template>
    <div>
        <div class="q-pa-md" style="max-width: 400px">
            <q-form @submit="onSubmit">
                <q-input v-model="id" label="ID" readonly />
                <q-input v-model="first_Name" label="First Name" />
                <q-input v-model="last_Name" label="Last Name" />
                <q-input v-model="email" label="Email" />
                <q-input v-model="address" label="Address" />
                <q-input v-model="phone_number" label="Phone Number" />
                <q-btn label="Submit" type="submit" color="primary" />
            </q-form>
        </div>
    </div>
</template>

<script setup>
 import { ref } from 'vue'
 import { useRoute, useRouter } from 'vue-router';
 

 const route = useRoute()
 const router = useRouter()

 const id = ref(route.params.id)
 const first_Name = ref('')
 const last_Name = ref('')
 const email = ref('')
 const address = ref('')
 const phone_number = ref('')

 const fetchData = () => {
    fetch('http://localhost:8800/api/v1/customers/' + id.value)
    .then(res => res.json())
    .then((result) => {
        first_Name.value = result.first_Name
        last_Name.value = result.last_Name
        email.value = result.email
        address.value = result.address
        phone_number.value = result.phone_number
    })
    .catch((error) => {
        console.error('Error fearcg data:' , error);
        alert('Error featch data:' , error);
    })
 };
 fetchData()

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
  method: "PUT",
  headers: myHeaders,
  body: raw,
  redirect: "follow"
};

fetch("http://localhost:8800/api/v1/customers/" + id.value, requestOptions)
  .then ((response) => {
    if (!response.ok ) {
        return response.json().then((err) => {
            throw new Error(err.message || 'Something went wrong!');
        });
    }
    return response.json()
  })
  .then((result) => {
    alert(result.message);
    console.log(result);
    if (result.status === 'ok') {
        router.push('/');
    }
    fetchData();
  })
  .catch((error) => {
    console.error('Error', error);
    alert('Error: ${error.message}');
  });
};

</script>