<template>
  <div>
    <v-toolbar color="red lighten-3" dark>
      <v-toolbar-title>Snippet App</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat v-on:click="togglePostModal()">New Snippet</v-btn>
    </v-toolbar>
    <v-container style="height: 1000px; max-width: 2400px;">
      <v-layout>
        <v-flex xs5>
          <div style="margin:10px">
            <h2>Snippets Shortcut</h2>
            <ul>
              <li>Snippet Title</li>
            </ul>
          </div>
        </v-flex>
        <v-flex xs7>
          <v-card style="margin-top:10px" v-for="snippet in snippetList" :key="snippet.id">
            <v-card-title primary-title>
              <h3 class="headline"> {{ snippet.title }} </h3>
            </v-card-title>
            <div style="margin: 10px 20px;">
              <textarea v-model="snippet.contents" style='width:100%; height:300px; background-color:#efefef; padding:3px'></textarea>
            </div>
            <v-card-actions>
              <v-btn flat color="red">Update</v-btn>
              <v-btn flat color="gray">Delete</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
      <v-dialog v-model="dialogPostFlag" width="800" persistent>
        <v-card>
          <v-card-title class="headline red lighten-3 white--text" primary-title>
            Create Form
          </v-card-title>
          <v-text-field v-model="postTitle" label="Snippet Title" required style='margin:20px; margin-top:30px'></v-text-field>
          <v-flex d-flex>
            <v-text-field v-model="postLanguage" label="Language" required style='margin:20px; margin-bottom:0px; margin-left:20px'></v-text-field>
          </v-flex>
          <v-card-text>
            <p>Snippet</p>
            <div style='width:100%;'>
              <textarea style='width:100%; height:300px; background-color:#efefef; padding:3px' v-model='postContents'></textarea>
            </div>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="#grey lighten-4" flat v-on:click="togglePostModal">
              Cancel
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="red" flat v-on:click="postSnippet">
              Add Snippet
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      snippetList: ['',''],
      allData: ['',''],
      dialogPostFlag: false,
      postTitle: '',
      postLanguage: '',
      postContents: ''
    }
  },
  mounted () {
    this.listSnippet();
  },
  methods: {
    listSnippet() {
      axios.get('http://localhost:3000/snippets.json')
        .then(response => {
          this.allData = response.data
          this.snippetList = this.allData
        }
      );
    },
    togglePostModal() {
      this.dialogPostFlag = !this.dialogPostFlag
    },
    postSnippet() {
      axios.post('http://localhost:3000/snippets', {title: this.postTitle, language: this.postLanguage, contents: this.postContents})
        .then(
          this.listSnippet(),
          this.postTitle = '',
          this.postLanguage = '',
          this.postContents = ''
        )
      this.dialogPostFlag = !this.dialogPostFlag
    }
  }
}
</script>
