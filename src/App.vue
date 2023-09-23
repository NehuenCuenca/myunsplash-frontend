<template>
  <header>
    <div class="logo-and-title">
      <i>👤</i>
      <span class="title">My Unsplash</span>
      <span class="sub-title">devchallenges.io</span>
    </div>
    <div class="actions">
      <input type="text" name="" id="" class="search-by-name" placeholder="🔍 Search by name">
      <button type="button" class="add-photo" @click="openForm('adding-new-photo', -1)">Add a photo</button>

      <div class="modal" :class="isAddingANewPhoto ? 'active' : ''">
        <form @submit.prevent="addingNewPhoto" class="adding-new-photo">
          <h3 class="title-modal">Add a new photo</h3>
          <div class="field">
            <label for="label-tag">Label: </label>
            <input type="text" id="label-tag" name="label-tag" v-model="labelNewImage" />
          </div>
          <div class="field">
            <label for="url-tag">Photo URL: </label>
            <input type="text" id="url-tag" name="url-tag" v-model="urlNewImage" />
          </div>

          <div class="buttons">
            <button type="button" class="cancel" @click="closeForm('adding-new-photo')">Cancel</button>
            <button type="submit">Submit</button>
          </div>
        </form>
      </div>
    </div>
  </header>

  <main>
    <ul class="grid-filtered-photos">
      <li class="grid-item-photo" v-for="(image, indexImage) in allImages" :key="indexImage">
        <img :src="image.url" :alt="image.name">
        <div class="on-hover">
          <span class="title">{{ image.name }}</span>
          <button class="delete" @click="openForm('deleting-photo', image.id)"> 🗑 delete </button>
        </div>
      </li>
    </ul>

    <div class="modal" :class="isDeletingAPhoto ? 'active' : ''">
      <form @submit.prevent="deletePhoto" class="deleting-photo">
        <h3 class="title-modal">Are you sure?</h3>
        <div class="field">
          <label for="password-tag">Password: </label>
          <input type="password" id="password-tag" name="password-tag" v-model="passwordForDelete" />
        </div>

        <div class="buttons">
          <button type="button" class="cancel" @click="closeForm('deleting-photo')">Cancel</button>
          <button type="submit">Delete</button>
        </div>
      </form>
    </div>
  </main>

  <footer>created by Nehuen - devChallenges.io</footer>
</template>

<script>
import { onMounted, ref } from 'vue';


export default {
  setup() {

    onMounted(async () => {
      await getAllImages()
    })

    const isAddingANewPhoto = ref(false)
    const isDeletingAPhoto = ref(false)

    const newImage = ref(null)
    const labelNewImage = ref('')
    const urlNewImage = ref('https://picsum.photos/200/300')
    const allImages = ref([]);

    const imageSelected = ref(-1)
    const passwordForDelete = ref('')


    const getAllImages = async () => {
      const urlApi = `http://127.0.0.1:8000/api/images`;

      const resp = await fetch(urlApi);
      const data = await resp.json();
      console.log(data);
      allImages.value = data.images
    }

    const deletePhoto = async () => {
      if (passwordForDelete.value.trim().length === 0) {
        return alert('Type the password please.')
      }

      try {
        const resp = await fetch(`http://127.0.0.1:8000/api/image/delete/${imageSelected.value}`, {
          method: 'DELETE',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ password: passwordForDelete.value }),
        })
        const data = await resp.json();
        console.log(data);
      } catch (error) {
        console.error('Something explode: ', error)
      }

    }

    const uploadImage = async () => {
      const fallbackName = labelNewImage.value.trim() || `nuevaImagen${Date.now()}`

      // https://api.cloudinary.com/v1_1/de9d1foso/image/upload

      const apiUrl = `http://127.0.0.1:8000/api/image/add`

      const formData = new FormData();
      formData.append('name', fallbackName);
      formData.append('newImage', newImage.value);
      formData.append('_method', 'POST')


      try {
        const resp = await fetch(apiUrl, {
          method: 'POST',
          'Content-Type': 'multipart/form-data',
          body: formData,
        });


        if (!resp.ok) {
          const { msg } = await resp.json()
          throw new Error(`${resp.statusText}: ${msg}`)
        }

        const data = await resp.json();
        console.log(data);
      } catch (error) {
        console.error(error, error.message);
        // alert(`${error}`)
      }
    }



    const createFile = (file) => {
      if (!file.type.match('image.*')) {
        alert('The file specified on the url is not an image');
        return;
      }

      let reader = new FileReader();

      reader.addEventListener('load', async function (e) {
        newImage.value = e.target.result;
        console.log(newImage.value);
        uploadImage()
      })

      reader.readAsDataURL(file);
    }

    const getImage = async () => {
      try {
        const resp = await fetch(urlNewImage.value)
        const achievedImage = await resp.blob()
        createFile(achievedImage)

      } catch (error) {
        console.error('An error has ocurred: ', error)
        alert('An error has ocurred: ', error)
      }
    }

    const addingNewPhoto = async () => {
      await getImage()
    }

    const openForm = async (form, imageId) => {
      console.log({ form, imageId });
      switch (form) {
        case 'adding-new-photo':
          isAddingANewPhoto.value = true
          break;
        case 'deleting-photo':
          isDeletingAPhoto.value = true
          break;
      }

      imageSelected.value = imageId
    }

    const closeForm = (form) => {
      switch (form) {
        case 'adding-new-photo':
          isAddingANewPhoto.value = false
          break;
        case 'deleting-photo':
          isDeletingAPhoto.value = false
          break;
      }

      imageSelected.value = -1
    }

    return {
      isAddingANewPhoto,
      isDeletingAPhoto,
      newImage,
      labelNewImage,
      urlNewImage,
      openForm,
      closeForm,
      addingNewPhoto,
      allImages,
      passwordForDelete,
      deletePhoto
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

.grid-item-photo img {
  width: 100%;
  height: 100%;
  border-radius: inherit;
  transition: all .5s ease;
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

.grid-item-photo:hover img {
  filter: brightness(.7);
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

.modal form {
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

.modal form .buttons button[type="submit"] {
  font: 700 1rem 'Noto Sans', sans-serif;
  color: #FFFF;
  padding: 10px 15px;
  border-radius: 12px;
}

.adding-new-photo .buttons button[type="submit"] {
  background-color: #3DB46D;

}

.deleting-photo .buttons button[type="submit"] {
  background: #EB5757;
}
</style>