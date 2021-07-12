<template>
  <v-app id="inspire">
    <v-navigation-drawer 
      v-model="drawer"
      app
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            Mobjects
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Manim Interactive</v-toolbar-title>

      <v-row class="d-flex flex-row-reverse">
        <v-btn
          class="mx-2"
          fab
          dark
          small 
          color="purple"
          to="/about"
        >
          <v-icon dark>
            mdi-help-box
          </v-icon>
        </v-btn>

        <v-btn
          class="mx-2"
          fab
          dark
          small 
          color="purple"
          to="/"
        >
          <v-icon dark>
            mdi-pencil
          </v-icon>
        </v-btn>

        <v-btn
          class="mx-2"
          fab
          dark
          small 
          color="purple"
        >
          <v-icon dark>
            mdi-download
          </v-icon>
        </v-btn>

      </v-row>
    </v-app-bar>

    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({ 
      drawer: null,
      items: [
          { title: 'Square', icon: 'mdi-help-box' },
          { title: 'Circle', icon: 'mdi-help-box' },
          { title: 'Triangle', icon: 'mdi-help-box' },
          { title: 'Function', icon: 'mdi-help-box' },
          { title: 'LaTeX', icon: 'mdi-help-box' },
      ]
    
    }),
  }
</script>
