<template>
  <div id="app">

    <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-3">
      <a class="navbar-brand" href="#">Message Board</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarColor02" aria-controls="navbarColor02" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarColor02">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">Message Board <span class="sr-only">(current)</span></a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Sections</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">API</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">About</a>
          </li>
        </ul>
      </div>
    </nav>
    <div class="container">
      
      <button @click="showform = !showform" type="button" class="btn btn-secondary mb-3">Toggle Form</button>

    <form v-if="showform"  @submit.prevent="addMessage" class="mb-3">
      <div class="form-group row">
        <label for="Username" class="col-sm-2 col-form-label">Username*</label>
        <div class="col-sm-10">
          <input v-model="msg.username" type="text"  class="form-control-plaintext" >
        </div>
      </div>
      <div class="form-group row">
        <label for="Subject" class="col-sm-2 col-form-label">Subject*</label>
        <div class="col-sm-10">
          <input v-model="msg.subject" type="text" class="form-control-plaintext">
        </div>
      </div>
      <div class="form-group row">
        <label for="Message" class="col-sm-2 col-form-label">Message*</label>
        <div class="col-sm-10">
          <textarea v-model="msg.message" rows="3" class="form-control-plaintext" />
        </div>
      </div>
      <div class="form-group row">
        <label for="ImageURL" class="col-sm-2 col-form-label">ImageURL</label>
        <div class="col-sm-10">
          <input v-model="msg.imageURL" type="url" class="form-control-plaintext">
        </div>
      </div>

      <button type="submit" class="btn btn-info">Submit</button>
   </form>

    <div class="list-group">
      <a href="#" v-for="Message in reversedMessages" :key="Message._id" class="list-group-item list-group-item-action flex-column align-items-start active">
        <div class="d-flex w-100 justify-content-between">
          <h5 class="mb-1">{{Message.username}}</h5>
          <small>{{Message.created}}</small>
        </div>
        <p class="mb-1">{{Message.message}}</p>
        <small>{{Message.subject}}</small>
      </a>
    </div>
    
  </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      showform:true,
      messages:[],
      msg:{ 
        username:'',
        subject:'',
        message:'',
        imageURL:'',
      }  
    }
  },
  computed: {
    reversedMessages() {
      return this.messages.slice().reverse();
    },
  },
  methods:{
    addMessage(){
      fetch('https://message-board-vue.herokuapp.com/messages',{
        method:'POST',
        body: JSON.stringify(this.msg),
        headers:{
          'content-type' : 'application/json'
        }
      }) 
      .then(res=>res.json())
      .then(data=>{
        console.log(data)   
        this.reloadMessage()  
      })
    },
    reloadMessage(){
      fetch('https://message-board-vue.herokuapp.com/messages')
      .then(res=>res.json())
      .then(data=>{
        this.messages = data
        console.log(this.messages);
        })
    }
  },
  components: {
  },
  mounted(){
      fetch('https://message-board-vue.herokuapp.com/messages')
      .then(res=>res.json())
      .then(data=>{
        this.messages = data
        console.log(this.messages);
        })
  }
}
</script>

<style scoped>
.navbar{
  border: 2px rgba(0, 0, 0, 0.2);
  border-bottom: 2px solid rgba(0, 0, 0, 0.2);
  padding: 0;
}
.form-control-plaintext{
  background: white;
  border-radius: 5px;
}
.form-control-plaintext{
  color: black;
}


</style>
