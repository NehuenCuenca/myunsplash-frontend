<template>
  <header>
    <div class="logo-and-title">
      <i>👤</i>
      <span class="title">My Unsplash</span>
      <span class="sub-title">devchallenges.io</span>
    </div>
    <div class="actions">
      <input type="text" name="" id="" class="search-by-name" placeholder="🔍 Search by name">
      <button type="button" class="add-photo">Add a photo</button>
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

export default {
  setup() {

    return {
    }
  }
}
</script>

<style scoped>
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

.on-hover > * { position: absolute; }
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

</style>