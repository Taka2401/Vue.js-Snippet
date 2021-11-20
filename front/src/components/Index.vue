<template>
  <div>
    <v-toolbar color="red lighten-3" dark>
      <v-toolbar-title>Snippet App</v-toolbar-title>
      <v-spacer></v-spacer>

      <!-- 投稿画面表示させる -->
      <v-btn text v-on:click="togglePostModal()">New Snippet</v-btn>
    </v-toolbar>
    <v-container style="height: 1000px; max-width: 2400px;">
      <v-layout>
        <!-- 画面左 -->
        <v-flex xs5>
          <div style="margin:10px">
            <h2>Snippets Shortcut</h2>
            <ul>
              <li>Snippet Title</li>
            </ul>
          </div>
        </v-flex>

        <!-- 画面右 -->
        <v-flex xs7>
          <v-card style="margin-top:10px" v-for="snippet in snippetList" :key="snippet.id">
            <v-card-title primary-title>
              <h3 class="headline"> {{ snippet.title }} </h3>
            </v-card-title>
            <div style="margin: 10px 20px;">
              <textarea v-model="snippet.contents" style='width:100%; height:300px; background-color:#efefef; padding:3px'></textarea>
            </div>
            <v-card-actions>
              <!-- 引数を渡してどのsnippetを更新するか特定する -->
              <v-btn text color="red" v-on:click="togglePutModal(snippet.id)">Update</v-btn>
              <v-btn text color="gray">Delete</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>

      <!-- post -->
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
            <v-btn color="#grey lighten-4" text v-on:click="togglePostModal">
              Cancel
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="red" text v-on:click="postSnippet">
              Add Snippet
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <!-- update -->
      <v-dialog v-model="dialogPutFlag" width="800" persistent>
          <v-card>
            <v-card-title class="headline red lighten-3 white--text" primary-title>
              Edit Form
            </v-card-title>
            <v-text-field v-model="putTitle" label="Snippet Title" required style='margin:20px; margin-top:30px'></v-text-field>
            <v-flex d-flex>
              <v-text-field v-model="putLanguage" label="Language" required style='margin:20px; margin-bottom:0px; margin-left:20px'></v-text-field>
            </v-flex>
            <v-card-text>
              <p>Snippet</p>
              <div style='width:100%;'>
                <textarea style='width:100%; height:300px; background-color:#efefef; padding:3px' v-model='putContents'></textarea>
              </div>
            </v-card-text>
            <v-divider></v-divider>
            <v-card-actions>
              <v-btn color="#grey lighten-4" text v-on:click="togglePutModal">
                Cancel
              </v-btn>
              <v-spacer></v-spacer>
              <v-btn color="red" text v-on:click="putSnippet">
                Update Snippet
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
      snippetList: [],
      allData: [],
      dialogPostFlag: false,
      postTitle: '',
      postLanguage: '',
      postContents: '',
      dialogPutFlag: false,
      putTitle: '',
      putLanguage: '',
      putContents: '',
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
    togglePutModal(id) {
        axios.get('http://localhost:3000/snippets/' + id + '.json')
        .then(response => {
          this.putTitle = response.data.title
          this.putLanguage = response.data.language
          this.putContents = response.data.contents
        }
      );
      this.id = id
      this.dialogPutFlag = !this.dialogPutFlag
    },
    putSnippet() {
      axios.put('http://localhost:3000/snippets/' + this.id + '.json', {title: this.putTitle, language: this.putLanguage, contents: this.putContents})
        .then(
          this.listSnippet()
        )
      this.dialogPutFlag = !this.dialogPutFlag
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
