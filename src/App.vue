<template>
  <div class="container">
    <h1 class="w-100 text-center text-white mb-0 mt-5">Characters</h1>
    <div class="filter row mt-4">
      <div class="col-6 text-end ps-0">
        <input v-model="search" type="text" />
      </div>
      <div class="col-6 pe-0">
        <select v-model="selected">
          <option>All</option>
          <option>Alive</option>
          <option>Dead</option>
          <option>unknown</option>
        </select>
      </div>
      <div class="col-12 text-center mt-4">
        <button class="submit" v-on:click="submitFilter()">SUBMIT</button>
      </div>
    </div>
    <div class="row">
      <div v-for="item in newArray" :key="item" class="col-lg-6 cards">
        <div class="row mycard">
          <div class="col-sm-6 p-0">
            <img :src="item.image" :alt="item.name" class="h-100 w-100" />
          </div>
          <div class="col-sm-6 text-start text-white info">
            <div class="block">
              <h3 class="fw-bold mb-0">{{ item.name }}</h3>
              <div class="status d-flex align-items-center">
                <div :class="`circle ${setColor(item.status)}`"></div>
                <div class="fw-bolder">
                  {{ item.status }} - {{ item.species }}
                </div>
              </div>
            </div>
            <div class="block">
              <span class="fw-bolder">Last known location:</span> <br />
              {{ item.location.name }}
            </div>
            <div class="block">
              <span class="fw-bolder">First seen in:</span> <br />
              {{ item.origin.name }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      answer: [],
      search: "",
      selected: "All",
      newArray: [],
    };
  },
  methods: {
    async getAnswer() {
      const { data } = await axios.get(
        "https://rickandmortyapi.com/api/character"
      );
      this.answer = data.results;
      this.newArray = this.answer;
    },
    setColor(status) {
      if (status === "Alive") {
        return "green";
      } else if (status === "Dead") {
        return "red";
      } else {
        return "gray";
      }
    },
    submitFilter() {
      this.newArray = this.answer;
      if (this.selected === "All") {
        if (this.search.length > 0) {
          this.newArray = this.answer.filter((e) => {
            return e.name.toLowerCase().includes(this.search.toLowerCase());
          });
        } else {
          return this.answer;
        }
      } else {
        this.newArray = this.answer.filter((e) => {
          return (
            e.status === this.selected &&
            e.name.toLowerCase().includes(this.search.toLowerCase())
          );
        });
      }
      console.log(this.answer);
    },
  },
  beforeMount() {
    this.getAnswer();
    
  },
};
</script>

<style>
body {
  background: #272b33 !important;
}
select,
input, .submit {
  width: 50%;
  height: 30px;
  background-color: #3c3e44;
  border: 1px solid white;
  border-radius: 10px;
  color: white;
  padding: 0 0.5rem;

}
.green {
  background: #55cc44;
}
.red {
  background: #d63d2e;
}
.gray {
  background: #9e9e9e;
}
.mycard {
  margin-top: 1.75rem !important;
}
.cards:nth-child(2n + 1) .mycard {
  padding-right: 0.875rem;
}
.cards:nth-child(2n) .mycard {
  padding-left: 0.875rem;
}
.mycard img {
  border-radius: 20px 0 0 20px;
}
.mycard .info {
  background-color: #3c3e44;
  padding: 0.75rem;
  border-radius: 0 20px 20px 0;
}
.mycard .info .block {
  margin-bottom: 1rem;
}
.mycard .info .circle {
  height: 10px;
  width: 10px;
  border-radius: 25px;
  margin-right: 0.5rem;
}
.mycard .info span {
  color: #9e9e9e;
}
@media screen and (max-width: 992px) {
  .cards .mycard {
    padding: 0 !important;
  }
}
@media screen and (max-width: 575px) {
  select,
  input {
    width: 100% !important;
  }
  .container {
    padding: 0 2rem !important;
  }
  .mycard img {
    border-radius: 20px 20px 0 0px;
  }
  .mycard .info {
    border-radius: 0 0 20px 20px;
  }
}
</style>
