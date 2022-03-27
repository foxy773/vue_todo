<template>
<body>
<section id="todoapp">
   <header id="header">
     <h1>Travel-List</h1>
     <input id="new-todo" v-model="searchText" placeholder="Country I want to visit" @blur="autoFillInput()" @keyup.enter="autoFillInput()">
   </header>
   <section id="main">
     <ul id="todo-list" >
		 <CountryToDo v-for="(country, index) in filteredList" :key="country.id" :name="country.name.common" :done="country.done" @click="toggleDone(country)"/>
     </ul>
   </section>
 </section>
 </body>
</template>

<script>
import CountryToDo from "../components/CountryToDo.vue"

export default {
	components: {
        CountryToDo
    },
  data() {
    return {
      searchText: "",
      filteredCountries: [],
      countries: null,
      setupDone: false
    };
  },

  async created() {
	  this.checkLocalStorage()
    /* this.fetchCountries(); */
  },

  computed: {
    filteredList() {
      if (this.setupDone) {
      let filteredList = this.filteredCountries.filter((country) => {
        return country.name.common
          .toLowerCase()
          .includes(this.searchText.toLowerCase());
      });
      this.storeInLocal()
      return filteredList;
      }
    },

    unCheckedList() {
      let unCheckedList = this.filteredList.filter((country) => {
        return country.done === false
      })
      return unCheckedList
    }
  },

  methods: {
    async fetchCountries() {
      const API = `https://restcountries.com/v3.1/all`;

      const res = await fetch(API);
      const results = await res.json();
      return results;
    },

	async organizeArray() {
		let unorganized = await this.fetchCountries()
    let organized = []
    unorganized.forEach((country) => {
      country = {...country, done:false}
      organized.push(country)
    })
      this.filteredCountries = organized
      this.setupDone = true 
    },

	toggleDone(country){
		return country.done = !country.done
	},

  autoFillInput() {
    if (this.searchText.length > 0 && this.unCheckedList.length > 0){

      this.searchText = this.unCheckedList[0].name.common

    } else {

      this.searchText = ""
    }
  },

  storeInLocal() {
    localStorage.setItem("Countries", JSON.stringify(this.filteredCountries))
  },

  getFromLocal() {
    this.filteredCountries = JSON.parse(localStorage.getItem("Countries"))
    this.setupDone = true 
  },

  checkLocalStorage() {   // Checks if localstorage contains any data. If it does it will get the data
    if (localStorage.getItem("Countries") !== null || localStorage.length > 0) {
      this.getFromLocal() // LocalStorage is empty
      console.log("Getting from Local")
    } else {
      this.organizeArray() 
      console.log("Not anything in local")
    }
  }

  },

  mounted() {

  },

};
</script>
<style scoped>
html,
body {
 margin: 0;
 padding: 0;
}

button {
 margin: 0;
 padding: 0;
 border: 0;
 background: none;
 font-size: 100%;
 vertical-align: baseline;
 font-family: inherit;
 color: inherit;
 -webkit-appearance: none;
 -ms-appearance: none;
 -o-appearance: none;
 appearance: none;
}

body {
 font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
 line-height: 1.4em;
 color: #4d4d4d;
 width: 550px;
 margin: 0 auto;
 -webkit-font-smoothing: antialiased;
 -moz-font-smoothing: antialiased;
 -ms-font-smoothing: antialiased;
 -o-font-smoothing: antialiased;
 font-smoothing: antialiased;
}

button,
input[type='checkbox'] {
 outline: none;
}

#todoapp {
 background: #fff;
 background: rgba(255, 255, 255, 0.9);
 margin: 130px 0 40px 0;
 border: 1px solid #ccc;
 position: relative;
 border-top-left-radius: 2px;
 border-top-right-radius: 2px;
 box-shadow: 0 2px 6px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.15);
}

#todoapp input::-webkit-input-placeholder {
 font-style: italic;
}

#todoapp input::-moz-placeholder {
 font-style: italic;
 color: #a9a9a9;
}

#todoapp h1 {
 position: absolute;
 top: -120px;
 width: 100%;
 font-size: 70px;
 font-weight: bold;
 text-align: center;
 color: #b3b3b3;
 color: rgba(255, 255, 255, 0.3);
 text-shadow: -1px -1px rgba(0, 0, 0, 0.2);
 -webkit-text-rendering: optimizeLegibility;
 -moz-text-rendering: optimizeLegibility;
 -ms-text-rendering: optimizeLegibility;
 -o-text-rendering: optimizeLegibility;
 text-rendering: optimizeLegibility;
}

#header {
 padding-top: 15px;
 border-radius: inherit;
}

#header:before {
 content: '';
 position: absolute;
 top: 0;
 right: 0;
 left: 0;
 height: 15px;
 z-index: 2;
 border-bottom: 1px solid #6c615c;
 background: #8d7d77;
 background: -webkit-gradient(
   linear,
   left top,
   left bottom,
   from(rgba(132, 110, 100, 0.8)),
   to(rgba(101, 84, 76, 0.8))
 );
 background: -webkit-linear-gradient(
   top,
   rgba(132, 110, 100, 0.8),
   rgba(101, 84, 76, 0.8)
 );
 background: linear-gradient(
   top,
   rgba(132, 110, 100, 0.8),
   rgba(101, 84, 76, 0.8)
 );
 filter: progid:DXImageTransform.Microsoft.gradient(GradientType=0,StartColorStr='#9d8b83', EndColorStr='#847670');
 border-top-left-radius: 1px;
 border-top-right-radius: 1px;
}

#new-todo,
.edit {
 position: relative;
 margin: 0;
 width: 100%;
 font-size: 24px;
 font-family: inherit;
 line-height: 1.4em;
 border: 0;
 outline: none;
 color: inherit;
 padding: 6px;
 border: 1px solid #999;
 box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
 -moz-box-sizing: border-box;
 -ms-box-sizing: border-box;
 -o-box-sizing: border-box;
 box-sizing: border-box;
 -webkit-font-smoothing: antialiased;
 -moz-font-smoothing: antialiased;
 -ms-font-smoothing: antialiased;
 -o-font-smoothing: antialiased;
 font-smoothing: antialiased;
}

#new-todo {
 padding: 16px 16px 16px 60px;
 border: none;
 background: rgba(0, 0, 0, 0.02);
 z-index: 2;
 box-shadow: none;
}

#main {
 position: relative;
 z-index: 2;
 border-top: 1px dotted #adadad;
}

#todo-list {
 margin: 0;
 padding: 0;
 list-style: none;
}



#footer {
 color: #777;
 padding: 0 15px;
 position: absolute;
 right: 0;
 bottom: -31px;
 left: 0;
 height: 20px;
 z-index: 1;
 text-align: center;
}

#filters {
 margin: 0;
 padding: 0;
 list-style: none;
 position: absolute;
 right: 0;
 left: 0;
}
</style>

