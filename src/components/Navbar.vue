<template>
	<nav>
		<v-toolbar app light class>
      <v-toolbar-side-icon @click="isDrawerOpen = !isDrawerOpen" />
      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-white secondary--text">PomoDoro</span>
        <span class="font-weight-light"></span>
      </v-toolbar-title>
		</v-toolbar>

    <v-navigation-drawer app dark v-model="isDrawerOpen">
      <v-toolbar class="secondary">
        <v-toolbar-title class="title"># of PomoDoros</v-toolbar-title>
      </v-toolbar>
	  <v-list>
		  <v-list-tile v-for="(lap, index) in laps" :key="index">
			  Lap {{ index + 1 }}: {{ lap.formattedTime }}
		  </v-list-tile>
	  </v-list>
    </v-navigation-drawer>
	</nav>
</template>

<script>
export default {
  name: 'Navbar',
  props: ['laps'],
  data () {
    return {
      isDrawerOpen: false
    }
  }
}
</script>
<style>

.text-uppercase {

float: center;
}
.v-toolbar {
    color:aquamarine;
}

</style>
