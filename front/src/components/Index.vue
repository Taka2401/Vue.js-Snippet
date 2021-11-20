<template>
  <div>
    <v-toolbar color="red lighten-3" dark>
      <v-toolbar-title>Snippet App</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat>New Snippet</v-btn>
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
          <v-card style="margin-top:10px">
            <v-card-title primary-title>
              <h3 class="headline">Snippet Title</h3>
            </v-card-title>
            <div style="margin: 10px 20px;">
              <textarea style='width:100%; height:300px; background-color:#efefef; padding:3px'> hoge </textarea>
            </div>

            <v-card-actions>
              <v-btn flat color="red">Update</v-btn>
              <v-btn flat color="gray">Delete</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>
