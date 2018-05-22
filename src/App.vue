<template>
  <div id="app">

    <h2>Step 1</h2>

    <b-form inline>
      <label for="orgName">Org Name:</label>
      <b-form-input
        id="orgName"
        v-model="orgName"
        type="text"
        placeholder="kayi-tsj"
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
        placeholder="kayi-tsj.github.io"
      ></b-form-input>
      <b-button v-on:click="handleCreateRepo">Create Repo</b-button>
      <p>
        <ol>


        <li>
enable Github Pages
        </li>
<li>
upload photos

</li>
<li>
get link like https://XXX.github.io/attachments/69.jpg for main photo
</li>
</ol>
      </p>
    </b-form>

    <h2>Step 3</h2>

    <b-form inline>
      <label for="attachmentsRepoName">Attachments Repo Name</label>
      <b-form-input
        id="attachmentsRepoName"
        v-model="attachmentsRepoName"
        type="text"
        placeholder="attachments"
      ></b-form-input>
      <b-button v-on:click="handleCreateAttachmentsRepo">Create Attachments Repo</b-button>
      <b-alert :show="showCreateAttachmentsRepoErrorMessage"
         dismissible
         variant="danger">
         {{createAttachmentsRepoErrorMessage}}
       </b-alert>
       <b-alert v-if="createAttachmentsRepoSuccessMessage !== ''"
          dismissible
          variant="success">
          <a :href="createAttachmentsRepoSuccessMessage">{{createAttachmentsRepoSuccessMessage}}</a>
        </b-alert>
    </b-form>

    <img src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import debugModule from "debug";

import HelloWorld from "./components/HelloWorld.vue";
import { token } from "./token.js";

const debug = debugModule("maker:App");
const octokit = require("@octokit/rest")();

// oauth
octokit.authenticate({
  type: "oauth",
  token
});

export default {
  name: "app",
  components: {
    HelloWorld
  },
  data: () => ({
    orgName: "kayi-tsj",
    repoName: "",
    attachmentsRepoName: "attachments",
    showCreateAttachmentsRepoErrorMessage: false,
    createAttachmentsRepoErrorMessage: "",
    createAttachmentsRepoSuccessMessage: ""
  }),
  mounted: function() {
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
    handleCreateOrg: function() {
      // const { orgName } = this
      // octokit.org.create({ name: orgName })
      //   .then(result => {
      //     console.log('result = ', result)
      //   })
    },
    handleCreateRepo: function() {
      this.createRepo(this.repoName);
    },
    handleCreateAttachmentsRepo: function() {
      this.createRepo(this.attachmentsRepoName);
    },
    createRepo: function(name) {
      debug(`Create repo: ${name} in Org: ${this.orgName}`);
      octokit.repos
        .createForOrg({ org: this.orgName, name, auto_init: true })
        .then(result => {
          debug("result =", result);
          this.createAttachmentsRepoSuccessMessage = result.html_url;
        })
        .catch(error => {
          debug("error = ", error);
          switch (error.code) {
            case 401:
            case 403:
            case 422: // name already exists on this account
              this.showCreateAttachmentsRepoErrorMessage = true;
              this.createAttachmentsRepoErrorMessage = error.message;
              break;
            default:
              break;
          }
        });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px;
}
</style>
