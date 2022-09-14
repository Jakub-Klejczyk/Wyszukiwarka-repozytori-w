<script>
export default {
  data() {
    return {
      query: "",
      filter: "",
    };
  },
  methods: {
    loadData() {
      fetch("https://api.github.com/repositories")
        .then((res) => res.json())
        .then((dataRow) => {
          let data = dataRow.map((repo) => {
            return {
              name: repo.name,
              fullName: repo.full_name,
              avatar: repo.owner.avatar_url,
              url: repo.html_url,
            };
          });
          console.log(data);
          this.renderData(data);
        });
    },
    validateInputValue(e) {
      this.query = e.target.value;
      const queryVal = this.query.split("");
      if (queryVal.length >= 3) {
        this.filter = this.query;
      }
    },
    renderData(data) {
      document.querySelector("#container").innerHTML = "";

      const rootElement = document.querySelector("#container");

      if (this.filter) {
        const filteredData = data.filter((item) => {
          return item.fullName.toLowerCase().includes(this.filter);
        });

        const filteredAndSlicedData = filteredData.slice(0, 10);
        console.log(filteredAndSlicedData);
        rootElement.appendChild(this.createItemElement(filteredAndSlicedData));
      } else {
        alert("Wprowadź co najmniej trzy znaki w polu 'Szukaj...'");
      }

      this.query = "";
      this.filter = "";
      document.querySelector("#search").value = "";
    },
    createItemElement(data) {
      const itemElement = document.createElement("li");

      data.forEach((item) => {
        itemElement.appendChild(
          this.appendData(item.name, item.fullName, item.avatar, item.url)
        );
      });

      return itemElement;
    },
    appendData(name, fullName, avatar, url) {
      const divElement = document.createElement("div");
      const aElement = document.createElement("a");
      const strongElement = document.createElement("strong");
      const pElement = document.createElement("p");
      const textElement = document.createElement("div");
      const imgElement = document.createElement("img");

      divElement.classList.add("item");
      strongElement.classList.add("strong");
      pElement.classList.add("para");
      textElement.classList.add("text");
      imgElement.classList.add("avatar");
      aElement.classList.add("anhor");

      strongElement.innerText = name;
      pElement.innerText = fullName;
      imgElement.src = avatar;
      aElement.href = url;
      aElement.target = "_blank";

      textElement.append(strongElement, pElement);
      aElement.append(imgElement, textElement);
      divElement.append(aElement);

      return divElement;
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
        @input="validateInputValue($event)"
      />
      <button><span class="material-symbols-outlined"> search </span></button>
    </form>
    <ul id="container"></ul>
  </div>
</template>

<style>
form {
  width: 30rem;
  height: 4rem;
  background-color: #53bc9c;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-inline: 1rem;
}
#main {
  width: fit-content;
  border-radius: 0.5rem;
  overflow: hidden;
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
  color: #bcc4d2;
}
.para {
  color: #617587;
  margin: 0;
}
</style>
