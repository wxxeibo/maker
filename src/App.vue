<template>
  <div id="app">

    <h2>Step 1</h2>

    <b-form inline>
      <label for="orgName">Org Name:</label>
      <b-form-input
        id="orgName"
        v-model="orgName"
        type="text"
        placeholder="chen-shy"
      ></b-form-input>
      <b-button disabled v-on:click="handleCreateOrg">Create Org</b-button>
      <b-alert show variant="danger">Currently not support create Org</b-alert>
    </b-form>

    <h2>Step 2</h2>

    <b-form inline>
      <label for="repoName">Repo Name:</label>
      <b-form-input
        id="repoName"
        v-model="repoName"
        type="text"
        placeholder="chen-shy.github.io"
      ></b-form-input>
      <b-button v-on:click="handleCreateRepo">Create Repo</b-button>

      <label for="attachmentsRepoName">Attachments Repo Name</label>
      <b-form-input
        id="attachmentsRepoName"
        v-model="attachmentsRepoName"
        type="text"
        placeholder="attachments"
      ></b-form-input>
      <b-button v-on:click="handleCreateAttachmentsRepo">Create Attachments Repo</b-button>

    </b-form>
    <img src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
const octokit = require('@octokit/rest')()

// oauth
octokit.authenticate({
  type: 'oauth',
  token: '179b9d52abaa3e40b13cbf64af54f006a0928a37'
})

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data: () => ({
    orgName: 'chen-shy',
    repoName: '',
    attachmentsRepoName: 'attachments'
  }),
  mounted: function () {
    // var self = this;
    // octokit.repos
    //   .get({
    //     owner: 'wxxeibo',
    //     repo: 'maker'
    //   })
    //   .then(({data}) => {
    //     self.foo = data;
    //   })
  },
  methods: {
    // Currently not support creating Org
    handleCreateOrg: function () {
      // const { orgName } = this
      // octokit.org.create({ name: orgName })
      //   .then(result => {
      //     console.log('result = ', result)
      //   })
    },
    handleCreateRepo: function () {
      this.createRepo(this.repoName)
    },
    handleCreateAttachmentsRepo: function () {
      this.createRepo(this.attachmentsRepoName)
    },
    createRepo: function(name) {
      octokit.repos
        .createForOrg({ org: this.orgName, name })
        .then(result => {
          console.log('result = ', result)
        })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px;
}
</style>
