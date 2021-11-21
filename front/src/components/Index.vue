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
            <h2>Select Language</h2>
            <v-select v-model='language' :items="languages" label="Language" @change="abstruct"></v-select>
            <h2>Snippets Shortcut</h2>
            <ul>
              <!-- コンテンツに一意のキーを持たせgoElm()で移動させる -->
              <li v-for="snippet in snippetList" :key="snippet.id"><span id='name' @click="goElem(snippet.id)">[{{ snippet.language }}] {{ snippet.title }}</span></li>
            </ul>
          </div>
        </v-flex>

        <!-- 画面右 -->
        <v-flex xs7 style='margin:10px'>
          <h2>Search Snippet</h2>
          <v-text-field v-model="searchWord" @keyup="abstruct" label="Input Keyword" style='margin-top:4px'></v-text-field>
          <v-card style="margin-top:10px" v-for="snippet in snippetList" :key="snippet.id">
              <a :id='snippet.id'></a>
              <v-card-title primary-title>
              <div style="background-color:#FFCDD2">
                <h3 class="headline">[{{ snippet.language }}] {{ snippet.title }}</h3>
              </div>
            </v-card-title>
            <div style="margin: 10px 20px;">
              <textarea v-model="snippet.contents" style='width:100%; height:300px; background-color:#efefef; padding:3px'></textarea>
            </div>
            <v-card-actions>
              <!-- 引数を渡してどのsnippetを更新するか特定する -->
              <v-btn text color="red" @click="togglePutModal(snippet.id)">Update</v-btn>
              <v-btn text color="gray" @click="toggleDeleteModal(snippet.id)">Delete</v-btn>
              <v-spacer></v-spacer>
              <v-btn text color="red" @click="goTop">Go Top</v-btn>
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
            <v-select v-model='postLanguage' :items="languagesForEdit" label="Select from history" style='margin:20px; margin-bottom:0px;margin-left: 0px'></v-select>
          </v-flex>
          <v-card-text>
            <p>Snippet</p>
            <div style='width:100%;'>
              <textarea style='width:100%; height:300px; background-color:#efefef; padding:3px' v-model='postContents'></textarea>
            </div>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="#grey lighten-4" text @click="togglePostModal">
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
              <v-btn color="#grey lighten-4" text @click="togglePutModal">
                Cancel
              </v-btn>
              <v-spacer></v-spacer>
              <v-btn color="red" text @click="putSnippet">
                Update Snippet
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <!-- delete -->
        <v-dialog v-model="dialogDeleteFlag" width="400">
          <v-card>
            <v-card-title class="headline red lighten-3 white--text" primary-title>
              Confirm
            </v-card-title>
            <v-spacer></v-spacer>
            <v-card-text>
              <p>本当に削除してもよろしいですか？</p>
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red" text @click="deleteSnippet()">
                Delete
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
      languages: ['All'],
      languagesForEdit: [],
      language: 'ALL',
      dialogPostFlag: false,
      postTitle: '',
      postLanguage: '',
      postContents: '',
      dialogPutFlag: false,
      putTitle: '',
      putLanguage: '',
      putContents: '',
      dialogDeleteFlag: false,
      searchWord: ''
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
          this.listLanguages()
          this.abstruct()
        }
      )
    },
    listLanguages() {
      this.languages = []
      this.languagesForEdit = []
      this.languages.push('ALL')
      for (let i = 0; i < this.allData.length; i++) {
        // indexOfで引数に与えられた内容と同じ内容を持つ配列要素の内、最初の添字を返す。
        if (this.languages.indexOf(this.allData[i].language) == -1) {
          // 一致したら配列に代入していく
          this.languages.push(this.allData[i].language)
          this.languagesForEdit.push(this.allData[i].language)
        }
      }
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
    toggleDeleteModal(id) {
      this.id = id
      this.dialogDeleteFlag = !this.dialogDeleteFlag
    },
    deleteSnippet() {
      axios.delete('http://localhost:3000/snippets/' + this.id + '.json')
        .then(
          this.listSnippet()
        )
      this.dialogDeleteFlag = !this.dialogDeleteFlag
    },
    goElem(id) {
      // 見出しをクリックすると記事まで移動する
      document.getElementById(id).scrollIntoView(true)
    },
    goTop() {
      document.getElementById("app").scrollIntoView(true)
    },
    abstruct() {
      // セレクトボックスがALLの時
      if (this.language == 'ALL') {
        this.snippetList = []
        for (let i = 0; i < this.allData.length; i++) {
          if ((this.allData[i].contents.indexOf(this.searchWord) !== -1) || (this.allData[i].title.indexOf(this.searchWord) !== -1) || (this.allData[i].language.indexOf(this.searchWord) !== -1)) {

            // 各要素を1つづつ空配列だった snippetList に追加
            this.snippetList.push(this.allData[i])
          }
        }
      } else if (this.language != '') {
        this.snippetList = []
        for (let i = 0; i < this.allData.length; i++) {
          if (this.allData[i].language == this.language) {
            if ((this.allData[i].contents.indexOf(this.searchWord) !== -1) || (this.allData[i].title.indexOf(this.searchWord) !== -1) || (this.allData[i].language.indexOf(this.searchWord) !== -1)) {

              // 選択した言語と一致したものを追加する
              this.snippetList.push(this.allData[i])
            }
          }
        }
      }
    }
  }
}
</script>
