<template>
  <v-card>
    <v-layout>
      <v-app-bar color="primary">
        <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

        <v-toolbar-title>My files</v-toolbar-title>

        <template v-if="$vuetify.display.mdAndUp">
          <v-btn icon="mdi-magnify" variant="text"></v-btn>

          <v-btn icon="mdi-filter" variant="text"></v-btn>
        </template>

        <v-btn icon="mdi-dots-vertical" variant="text"></v-btn>
      </v-app-bar>

      <v-navigation-drawer
        v-model="drawer"
        :location="$vuetify.display.mobile ? 'bottom' : undefined"
        temporary
      >
        <v-list
          :items="items"
        ></v-list>
      </v-navigation-drawer>

      <v-main style="height: 500px;">
        <v-card-text>
          The navigation drawer will appear from the bottom on smaller size screens.
        </v-card-text>
      </v-main>
    </v-layout>
  </v-card>
</template>

<script setup>
  import { ref, watch } from 'vue'

  const items = [
    {
      title: 'Foo',
      value: 'foo',
    },
    {
      title: 'Bar',
      value: 'bar',
    },
    {
      title: 'Fizz',
      value: 'fizz',
    },
    {
      title: 'Buzz',
      value: 'buzz',
    },
  ]

  const drawer = ref(false)
  const group = ref(null)

  watch(group, () => {
    drawer.value = false
  })
</script>

<script>
  export default {
    data: () => ({
      drawer: false,
      group: null,
      items: [
        {
          title: 'Foo',
          value: 'foo',
        },
        {
          title: 'Bar',
          value: 'bar',
        },
        {
          title: 'Fizz',
          value: 'fizz',
        },
        {
          title: 'Buzz',
          value: 'buzz',
        },
      ],
    }),

    watch: {
      group () {
        this.drawer = false
      },
    },
  }
</script>
