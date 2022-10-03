<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <form class="col s12" @submit.prevent="searchMessages">
      <div class="row">
        <div class="input-field col s6">
          <input id="input_text" type="text" v-model="inputSearch">
          <label for="input_text">Enter your query</label>
        </div>
        <button class="btn waves-effect waves-light" type="submit" name="action">Search</button>
      </div>
    </form>
    <div class="row">
      <div class="col s5">
        <table class="table striped bordered">
          <thead>
            <tr>
              <th>Subject</th>
              <th>From</th>
              <th>To</th>
            </tr>

          </thead>
          <tbody>
            <tr v-for="message in messages" :key="message" v-on:click="showMessage(message._source.Message)">
              <td>{{message._source.Subject}}</td>
              <td>{{message._source.From}}</td>
              <td>{{message._source.To}}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="input-field col s5">
        <textarea id="textarea1" class="materialize-textarea" v-model="contentMessage" ref="update" ></textarea>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      messages: [],
      contentMessage: '',
      inputSearch: ''
    }
  },
  methods: {
    searchMessages() {

      const url = "http://localhost:3000/search"
      let options = {
        method: 'POST',
        headers: {
          'Access-Control-Allow-Origin' : '*',
          'Access-Control-Request-Method': 'POST, OPTIONS'
        },
        body: JSON.stringify({ input: this.inputSearch })
      }
      console.log(options)
      const resp = fetch(url, options)
      resp.then(res => res.json()).then(data => 
      data.hits.hits
      ).then( hits => hits.map((data) => this.messages.push(data)))
    },
    showMessage(data){
      this.contentMessage = data
      this.$refs.update.blur()
      console.log(this.$refs.update)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
