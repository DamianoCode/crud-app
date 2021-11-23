<template>
    <div class="container">
     <div class="row justify-content-md-center">
    <input class="col-md-7 g-2" v-model="value" placeholder="Podaj filtrowaną wartość"><button class="btn btn-primary col-md-2 g-2" @click="filterData(value)">Filtruj</button>
    <input class="col-md-4  g-2" v-model="firstname" placeholder="Podaj zmienione imię"><input class="col-md-4  g-2" v-model="lastname" placeholder="Podaj zmienione nazwisko">
     </div>
    </div>
  <div class="container">
      <div class="row gx-5">
      <div class="col-md-4" v-for="(user, index) in usersListFiltered" :key="user.id">
          <br>
          <p>{{user.first_name}}</p>
          <p>{{user.last_name}}</p>
          <p>{{user.email}}</p>
          <img :src="`${user.avatar}`" :alt="`${user.id}`">
          <br>
          <button @click="remove(index)" class="btn btn-danger">usuń</button>
          <button @click="edit(index)" class="btn btn-success">edytuj</button>
      </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
import {ref} from 'vue';

export default {
    name: 'Wrapper',
    components: {
    },
    setup(){
        const API = 'https://reqres.in/api/users?page=2';
        let usersList = ref([]);
        let usersListFiltered = ref([]);
        let firstname = ref("");
        let lastname = ref("");
        const value = ref("");
        const getUsers = () => {
            axios.get(`${API}`)
            .then(resp => {
                usersList.value = resp.data.data;
                usersListFiltered.value = resp.data.data;
            })
            .catch( (e) => console.log(e));
        };
        getUsers();
        const filterData = (value) => {
            usersList.value.forEach(element => {
                if(value == ""){
                    usersListFiltered.value = usersList.value;
                }
                else{
                if(element.email == value){
                    usersListFiltered.value = usersListFiltered.value.filter(user => user.email === value);
                }
                if(element.first_name == value){
                    usersListFiltered.value = usersListFiltered.value.filter(user => user.first_name === value);
                }
                if(element.last_name == value){
                   usersListFiltered.value = usersListFiltered.value.filter(user => user.last_name === value);
                }
                }
            });
        }
        const remove = (id) => {
            if (id > -1) {
             usersList.value.splice(id, 1);
            }
        }
        const edit = (id) => {
            if(firstname.value != ""){
                usersListFiltered.value[id].first_name = firstname.value;
            }
            if(lastname.value != ""){
            usersListFiltered.value[id].last_name = lastname.value;
            }
            firstname.value = lastname.value = "";
        }
        return{
            filterData,
            usersListFiltered,
            remove,
            edit,
            firstname,
            lastname,
            value
        }
    }
}
</script>

<style>

</style>