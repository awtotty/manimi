<template>
    <v-list
        rounded="lg"
        dense
        nav
    >
    <v-list-item
        v-for="item in items"
        :key="item.title"
        link
        @click="setShape(item.title)"
    >
        <v-list-item-icon>
        <v-icon>{{ item.icon }}</v-icon>
        </v-list-item-icon>

        <v-list-item-content>
        <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item-content>
    </v-list-item>
    </v-list>
</template>



<script>
export default ({
    name: "Mobjects",

    data: () => ({
      items: [
          { title: 'Square', icon: 'mdi-help-box' },
          { title: 'Circle', icon: 'mdi-help-box' },
          { title: 'Triangle', icon: 'mdi-help-box' },
          { title: 'Function', icon: 'mdi-help-box' },
          { title: 'LaTeX', icon: 'mdi-help-box' },
      ],
    }), 

    methods: {
        setShape(shapeStr) {
            console.log("set shape to " + shapeStr.toLowerCase()); 
        }
    }
})
</script>