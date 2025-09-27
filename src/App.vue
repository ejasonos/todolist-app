<script setup>
import {ref, onBeforeMount} from 'vue'

let newActivity // New activity bound with v-model to input tag
let edited // Bound to v-model to edit each activity title
let activityDB // activity database targetted by template literal (accessed even before onBeforeMount)

onBeforeMount(() => {
  // Array of activities
  if (localStorage.getItem('activities') === null) {
    activityDB = ref([])
  } else {
    activityDB = ref(JSON.parse(localStorage.getItem('activities')))
    console.log(activityDB.value);
  }
})


const addActivity = () => {
  if (newActivity.toString().trim() === '') {
    return;
  }
  activityDB.value.push(newActivity.toString()); //add new activity to in-memory storage
  localStorage.setItem('activities', JSON.stringify(activityDB.value));
  newActivity = ''
}

const deleteActivity = (index) => {
  activityDB.value.splice(index, 1);
  localStorage.setItem('activities', JSON.stringify(activityDB.value)); // update localstorage with current state of activityDB
  edited = '' // this is to prevent the value of edited text to fill all edited text values
}

const editActivity = (index, editedTask) => {
  if (editedTask === null) {
    return
  }
  if (editedTask.toString().trim() === "") {
    return
  }
  activityDB.value.splice(index, 1, editedTask);
  localStorage.setItem('activities', JSON.stringify(activityDB.value)); // update localstorage with current state of activityDB
  edited = '' // this is to prevent the value of edited text to fill all edited text values
}

</script>

<template>

  <main>

      <!-- Ejakpevweoghene todolist application -->

      <!-- Activity list section -->
      <section class="card">
        <h1>{{ activityDB.length <1 ? 'No tasks': 'Tasks:' }}</h1>
        <ul v-for="(eachTask, index) in activityDB" v-bind:key="index">
          <li>{{ eachTask }} 
            <span class="editContainer">
              <input v-if="index || index === 0" v-model="edited" type="text" maxlength="150" placeholder="Edit" class="editInput"/>
              <span v-on:click="editActivity(index, edited)" class="editButton">
                <img alt="edit" src="/assets/pencil.svg" class="pencil">
              </span>
            </span>
            <span v-on:click="deleteActivity(index)" class="cancelButton">x</span>
          </li>
        </ul>
      </section>

      <!-- Add activity section -->
       <form v-on:submit.prevent="addActivity()"  class="card createActivity">
        <label for="activity">New activity:</label>
        <input v-model="newActivity" type="text" maxlength="150" placeholder="Enter an activity" required />
        <button  class="submitButton" type="submit">Add activity</button>
      </form>

  </main>

</template>

<style scoped>
/* Styling for main tag and some sub elements */
main {
  width: 100vw;
  display: flex;flex-direction:column; align-items: center;justify-content: center;
}
main p {
  color: colored;
}
/* styling for each section */
.card {
  width: 40vmax;
  gap: 1em;
  margin: 1em 0;
  border-radius: 1em;
  background-color: #444;
  padding: 1.5em 1.2em;
  display: flex;flex-direction: column;
  font-family: 'Segoe ui';
  color: white;
}
/* styling for submit activity button */
.submitButton {
  width: fit-content;
  padding: 1em 1.5em;
  color: #abc;
  background-color: green;
  font-weight: bold;
  border-radius: 50px;
  border: 3px solid green;
}
.submitButton:hover {
  color: white;
  border-color: white;
}
/* styling for each activity listed */
li {
  width: 100%;
  border-radius: 0.5em;
  padding: 0.3em;
  background-color: #333;
  box-shadow: 1px 1px 5px 5px #444;
  display: grid;
  grid-template-rows: auto;
  grid-template-columns: 70% auto auto;
  align-items: center;
}
span.editButton, span.cancelButton {
  margin: 0 0.5em;
  float: flex-end;
  font-weight: bold;
  cursor: default;
}
img.pencil {
  height: 0.8em;width: 0.8em;
}
img.pencil:hover {
  filter: invert(35%);
}
.editContainer {
  display: flex;justify-content: space-around;
  border-radius: 20px;
  border:none;
  background-color: #1af;
  color: white;
  padding: 0.3em;
}
.editInput {
  float:flex-end;
  background-color: transparent;
  color: whitesmoke;
  border: none;
  outline: none;
  width: 2em;
}
span.editButton:hover {
  color: #abd;
}
span.cancelButton:hover {
  color: red;
}
</style>
