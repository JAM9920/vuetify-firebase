<template>
  <v-app>
    
    <v-navigation-drawer v-model="sideNav" absolute
      temporary>
      <v-list  v-for="item in menuItem" :key="item.title" :to="'/'+item.link">
        <v-list-tile avatar >
          <v-list-tile-action>
            <v-icon>{{item.icon}}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>{{item.title}}</v-list-tile-content>
        </v-list-tile>
        <v-list-tile avatar v-if="userIsAuthenticated"  @click="onLogout">
          <v-list-tile-action>
            <v-icon>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>Logout</v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-card
    color="grey lighten-4"
    text
    tile
    
    >
      <v-toolbar dark class="purple darken-1">
        <v-app-bar-nav-icon @click.native.stop="sideNav = !sidenav"
        class="hidden-sm-and-up"></v-app-bar-nav-icon>
        <v-toolbar-title>
          <router-link to="/" tag="span" style="cursor: pointer">DevMeetup</router-link>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items class="hidden-xs-only">
          <v-btn text v-for="item in menuItem" :key="item.title" :to="'/'+item.link">
            <v-icon>{{item.icon}}</v-icon>
            {{item.title}}
          </v-btn>
          <v-btn text v-if="userIsAuthenticated" @click="onLogout">
            <v-icon>exit_to_app</v-icon>
            Logout
          </v-btn>
        </v-toolbar-items>
      </v-toolbar>
    </v-card> 
    
    <main>
      <router-view></router-view>
    </main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      sideNav: false,
    }
  },
  computed: {
    menuItem () {
      let menuItems = [
        {icon: 'face', title: 'Sign Up', link: 'signup'},
        {icon: 'lock_open', title: 'Sign In', link: 'signin'},
      ]
      if (this.userIsAuthenticated) {
        menuItems = [
          {icon: 'supervisor_account', title: 'View Meetup', link: 'meetups'},
          {icon: 'room', title: 'Organize Meetup', link: 'meetup/new'},
          {icon: 'person', title: 'Profile', link: 'profile'},
        ]
      }
      return menuItems
    },
    userIsAuthenticated () {
      return this.$store.getters.user !== null && this.$store.getters.user !== undefined
    }
  },
  methods: {
    onLogout () {
      this.$store.dispatch('logout')
    }
  },
};
</script>

<style>
.white--text{
  user-select: none;
  text-decoration: none;
}
</style>