<template>
    <v-list
    rounded="lg"
    dense
    nav
    >
        <v-list-item
            v-for="item in items"
            :key="item.key"
        >
            <v-list-item-content>
            <v-list-item-title>{{ item.key }} {{ item.val }}</v-list-item-title>
            
            </v-list-item-content>
        </v-list-item>
    </v-list>
</template>


<script>
export default ({
    name: "Anims",

    data: () => ({
      items: [
          { key: 'Pos', val: '(0,0)' },
      ]
    })
})
</script>