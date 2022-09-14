<script>
import axios from "axios";

export default {
  data() {
    return {
      inputData: "",
      inputVal: false,
      infoMsg: "",
      data: [],
    };
  },
  methods: {
    validateInputValue() {
      if (this.inputData.length >= 3) {
        return (this.inputVal = true);
      } else {
        return (this.inputVal = false);
      }
    },
    loadData() {
      if (!this.validateInputValue()) {
        this.infoMsg = "Wpisz co najmniej 3 znaki!";
        this.data = [];
        setTimeout(() => {
          this.infoMsg = "";
        }, 3000);
        return;
      }
      const query = this.inputData.trim().toLocaleLowerCase();

      axios
        .get(`https://api.github.com/search/repositories?q=${query}`, {
          headers: {
            Accept: "application/vnd.github.v3+json",
          },
        })
        .then((res) => {
          this.data = res.data.items;
          this.data = this.data.slice(0, 10);
        })
        .catch((error) => {
          if (error) {
            this.infoMsg = "Brak szukanego repozytorium";
          }
        });
    },
  },
};
</script>

<template>
  <div id="main">
    <form action="" @submit.prevent="loadData()">
      <input
        id="search"
        type="text"
        placeholder="Szukaj..."
        autocomplete="off"
        v-model="inputData"
      />
      <button><span class="material-symbols-outlined"> search </span></button>
    </form>
    <ul id="container" v-if="data.length > 0">
      <li v-for="item in data" :key="item.id" class="item">
        <a class="anhor" :href="item.html_url" target="_blank">
          <img
            class="avatar"
            :src="item.owner.avatar_url"
            alt="Obraz użytkownika"
          />
          <div class="text">
            <strong class="strong">{{ item.name }}</strong>
            <p class="para">{{ item.full_name }}</p>
          </div>
        </a>
      </li>
    </ul>
    <p>{{ infoMsg }}</p>
  </div>
</template>

<style>
form {
  width: 30rem;
  height: 4rem;
  background-color: rgb(42, 187, 155);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-inline: 1rem;
}
#main {
  width: fit-content;
  border-radius: 0.5rem;
  overflow: hidden;
  font-size: 0.9rem;
  font-family: "Monstserrat", sans-serif;
}
#search {
  background-color: transparent;
  border: none;
  color: #225855;
  width: 90%;
  height: 60%;
}
#search::placeholder {
  color: #225855;
}
#search:focus {
  outline: none;
}
button {
  transform: scale(0.8);
  background-color: transparent;
  border: none;
  cursor: pointer;
  padding: 1rem;
}
.material-symbols-outlined {
  color: #225855;
}
#container {
  margin: 0;
  padding: 0;
}
.item {
  width: 30rem;
  height: 2rem;
  background-color: #35495e;
  border-bottom: 1px solid #2c3d52;
  padding: 1rem;
}
.avatar {
  width: 2rem;
  height: 2rem;
}
li {
  list-style: none;
}
.anhor {
  text-decoration: none;
  color: black;
  display: flex;
  gap: 1rem;
}
.text {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.strong {
  color: #ffffff;
}
.para {
  color: rgb(116, 154, 177);
  margin: 0;
}
</style>
