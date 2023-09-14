<template>
  <header>
    <div class="logo-and-title">
      <i>👤</i>
      <span class="title">My Unsplash</span>
      <span class="sub-title">devchallenges.io</span>
    </div>
    <div class="actions">
      <input type="text" name="" id="" class="search-by-name" placeholder="🔍 Search by name">
      <button type="button" class="add-photo" @click="openForm">Add a photo</button>

      <div class="modal" :class="isAddingANewPhoto ? 'active' : ''">
        <form @submit.prevent class="adding-new-photo">
          <h3 class="title-modal">Add a new photo</h3>
          <div class="field">
            <label for="label-tag">Label: </label>
            <input type="text" id="label-tag" name="label-tag" />
          </div>
          <div class="field">
            <label for="url-tag">Photo URL: </label>
            <input type="text" id="url-tag" name="url-tag" />
          </div>

          <div class="buttons">
            <button type="button" class="cancel" @click="closeForm">Cancel</button>
            <button type="submit">Submit</button>
          </div>
        </form>
      </div>
    </div>
  </header>
  <main>
    <ul class="grid-filtered-photos">
      <li class="grid-item-photo">
        Photo 1
        <div class="on-hover">
          <span class="title">This is a title example </span>
          <button class="delete">Delete</button>
        </div>
      </li>
      <li class="grid-item-photo">
        Photo 2
        <div class="on-hover">
          <span class="title">This is a title example </span>
          <button class="delete">Delete</button>
        </div>
      </li>
      <li class="grid-item-photo">
        Photo 3
        <div class="on-hover">
          <span class="title">This is a title example </span>
          <button class="delete">Delete</button>
        </div>
      </li>
      <li class="grid-item-photo">
        Photo 4
        <div class="on-hover">
          <span class="title">This is a title example </span>
          <button class="delete">Delete</button>
        </div>
      </li>
    </ul>
  </main>
  <footer>created by Nehuen - devChallenges.io</footer>
</template>

<script>
import { ref } from 'vue';


export default {
  setup() {

    const isAddingANewPhoto = ref(false)

    const openForm = () => {
      isAddingANewPhoto.value = true
    }
    const closeForm = () => {
      isAddingANewPhoto.value = false
    }

    return {
      isAddingANewPhoto,
      openForm,
      closeForm,
    }
  }
}
</script>

<style>
header {
  grid-area: header;
  width: 100%;
  display: flex;
  padding: 2vh 4vw;
  gap: 0 2vw;
}

main {
  grid-area: main;
  padding: 2vh 4vw;
}

footer {
  grid-area: footer;
  place-self: center;
}


.logo-and-title {
  display: grid;
  grid-template-columns: .25fr 75fr;
  grid-template-columns: auto;
  grid-template-areas:
    "icon title"
    "icon subTitle"
  ;
  gap: 0 .5vw;
  place-items: center;
}

.logo-and-title i {
  grid-area: icon;
  font-size: 2rem;
}

.title {
  grid-area: title;
  font: 800 14px 'Noto Sans', sans-serif;
  white-space: nowrap;
}

.sub-title {
  grid-area: subTitle;
  font: 500 9px 'Noto Sans', sans-serif;
}

.actions {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

input.search-by-name {
  width: fit-content;
  font: 500 14px 'Noto Sans', sans-serif;
  padding: 0 20px;
  border: 1px solid #BDBDBD;
  border-radius: 12px;
  outline: none;
}

button.add-photo {
  padding: 5px 15px;
  border-radius: 12px;
  box-shadow: 0px 4px 10px 0px #3DB46D33;
  background-color: #3DB46D;
  font: 700 1rem 'Noto Sans', sans-serif;
  color: #FFFF;
}

ul.grid-filtered-photos {
  width: 100%;
  height: auto;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: auto;
  gap: 4vh 2vw;
}

li.grid-item-photo {
  border-radius: 16px;
  background-color: lightslategrey;
  height: 40vh;
  position: relative;
}



.grid-item-photo .on-hover {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  transition: all .5s ease;
  opacity: 0;
}

.grid-item-photo:hover .on-hover {
  opacity: 1;
}

.on-hover>* {
  position: absolute;
}

.on-hover .title {
  left: 5%;
  bottom: 10%;
  width: 80%;
  font: 700 1.125rem 'Montserrat', sans-serif;
  color: white;
}

.on-hover button.delete {
  right: 10%;
  top: 10%;
  border: 1px solid #EB5757;
  padding: 5px 10px;
  border-radius: 38px;
  background-color: transparent;
  font: 500 12px 'Montserrat', sans-serif;
  color: #EB5757;
}

body:has(header .actions .modal.active) {
  overflow-y: hidden;
}

.modal {
  position: absolute;
  top: 0;
  left: 0;
  pointer-events: none;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity .4s ease;
  display: grid;
  place-items: center;
}

.modal.active {
  pointer-events: all;
  background-color: rgba(51, 51, 51, 0.454);
  opacity: 1;
  z-index: 999;
}

form.adding-new-photo {
  width: 50vw;
  border-radius: 12px;
  background-color: #FFFF;
  padding: 1rem;
  display: grid;
  grid-template-columns: 100%;
  grid-auto-rows: auto;
  gap: 2vh 0;
}

.modal .title-modal {
  font: 500 1.25rem 'Noto Sans', sans-serif;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 1vh 0;
  font: 500 14px 'Noto Sans', sans-serif;
}

.field label {
  color: #4F4F4F;
  cursor: pointer;
}

.field input {
  border: 1px solid #4F4F4F;
  border-radius: 12px;
  padding: 10px 10px;
  color: #BDBDBD;
}

.buttons {
  place-self: center end;
}

button.cancel {
  background-color: transparent;
  font: 500 1rem 'Noto Sans', sans-serif;
  color: #BDBDBD;
  margin-right: 20px;
}

.buttons button[type="submit"] {
  background-color: #3DB46D;
  font: 700 1rem 'Noto Sans', sans-serif;
  color: #FFFF;
  padding: 10px 15px;
  border-radius: 12px;
}
</style>