<template>
  <v-app>
    <v-navigation-drawer
      class="white drawer"
      v-model="drawer"
      fixed
      app
      width="300"
      v-if="$route.path !== '/'"
    >
      <v-list>
        <v-list-item
          class="my-1"
          v-for="(item, i) in drawer.top"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title class="body-2" v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-divider class="my-4"></v-divider>

      <v-list>
        <v-list-item
          class="my-1"
          v-for="(item, i) in drawer.bottom"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title class="body-2" v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar v-if="$route.path !== '/'" class="secondary nav-bar" flat>
      <v-spacer />
      <div class="accent--text mx-3">Take a tour</div>
      <v-badge dot color="error" class="mx-3" overlap>
        <v-icon class="white--text" color="primary" depressed>
          mdi-email
        </v-icon>
      </v-badge>
      <v-icon class="mx-3" color="accent"> mdi-bell </v-icon>
      <v-avatar class="mx-3">
        <img src="https://cdn.vuetifyjs.com/images/john.jpg" alt="John" />
      </v-avatar>
    </v-app-bar>
    <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer" /> -->

    <v-main class="secondary">
      <v-container class="py-0 my-0" style="height: 100%">
        <Nuxt style="height: 100%" />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import drawerLinks from '../functions/drawerLinks'
export default {
  name: 'DefaultLayout',
  data() {
    return {
      drawer: true,
      fixed: false,
      drawer: drawerLinks,
    }
  },
}
</script>

<style>
.nav-bar {
  height: 100px;
  padding: 1% 0;
}
.drawer {
  padding-top: 100px;
}
.v-list .v-list-item--active {
  color: white !important;
  background-color: #2a36a4;
}
</style>
