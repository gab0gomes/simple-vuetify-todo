<template>
	<v-app>
		<v-toolbar
			app
			dark
			color="cyan darken-4"
		>
			<v-icon
				color="teal lighten-2"
				large
			>
				playlist_add_check
			</v-icon>

			<v-toolbar-title
				class="teal--text text--lighten-2"
			>
				Vuetify Todo List
			</v-toolbar-title>
		</v-toolbar>

		<v-content>
			<router-view></router-view>
		</v-content>
		</v-app>
</template>

<script>

export default {
  name: 'App',
  data () {
	return {
	  //
	}
  }
}
</script>
