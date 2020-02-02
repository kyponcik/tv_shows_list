<template>
  <div class="container">
    <div class="search-field">
      <input class="search-input" placeholder="Название сериала" type="text" v-model="serialName">
      <button @click="searchShows()" class="search-btn">Поиск</button>
      <button @click="loadTop()" class="btn">Вернутся на главную</button>
    </div>
    <div class="shows-list">
      <show
        :key="serial.id"
        :serial="serial"
        v-for="serial in serials"
      />
    </div>
  </div>


</template>

<script>
  import Show from './Show.vue'

  export default {
    name: 'ShowsList',
    
    components: {
      Show
    },

    data() {
      return {
        serials: [],
        serialName: ''
      }
    },

    created() {
      this.loadTop()
    },

    methods: {
      loadTop() {
        fetch('https://api.myshows.me/v2/rpc/', {
          method: 'POST',
          body: JSON.stringify({
            'jsonrpc': '2.0',
            'method': 'shows.Top',
            'params': {
              'mode': 'all',
              'count': 500
            },
            'id': 1
          })
        })
          .then(r => r.json())
          .then(serials => this.serials = serials.result.map(serial => serial.show))
      },

      searchShows() {
        this.serials = []
        fetch('https://api.myshows.me/v2/rpc/', {
          method: 'POST',
          body: JSON.stringify({
            'jsonrpc': '2.0',
            'method': 'shows.Search',
            'params': {
              'query': this.serialName
            },
            'id': 1
          })
        })
          .then(r => r.json())
          .then(r => this.serials = r.result)
      }
    }
  }
</script>

<style scoped>
  .shows-list {
    display: inline-flex;
    flex-wrap: wrap;
    justify-content: space-around;
    text-align: center;
    margin-top: 10px;
  }

  .search-field {
    padding: 10px;
  }

  .search-input {
    width: 250px;
    margin-right: 20px;
    padding: 10px;
    background-color: #3a3a3d;
    border: 2px solid transparent;
    border-radius: 5px;
    color: #b9b9bb;
    font-size: 15px;
    box-sizing: content-box;


  }

  .search-input:focus {
    outline: none;
    border: 2px solid #9147ff;
    background-color: black;

  }

  .search-btn {
    width: 80px;
    height: 40px;
    border: none;
    border-radius: 5px;
    color: white;
    background-color: #9147ff;
    font-size: 15px;
    margin-right: 20px;
  }

  .search-btn:hover {
    background-color: #772ce8;
  }

  .btn {
    width: 200px;
    height: 40px;
    border: none;
    border-radius: 5px;
    color: white;
    background-color: #9147ff;
    font-size: 15px;

  }

  .btn:hover {
    background-color: #772ce8;
  }

</style>
