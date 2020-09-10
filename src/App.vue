<template>
	<div id="app">
		<Navigation :user="user" @logout="logout"/>
		<router-view class="container" :user="user" @logout="logout"/>
	</div>
</template>

<script>

import Navigation from "@/components/Navigation.vue";
import Firebase from "firebase";
import db from "./db.js";

export default {
	name: "app",
	data: function(){
		return {
			user:null
		}
	},
	methods : {
		logout : function (){
			Firebase.auth().signOut().then (()=> {
				this.user = null;
				this.$router.push("login");

			})
		}
	}
	,
	mounted(){
		Firebase.auth().onAuthStateChanged(user => {
			if (user) {
				this.user = user.displayName;
			}
		});
		db.collection("Users")
		.doc("WnEm5vbP4DrcuMNgfdT6")
		.get()
		.then((response) => {
			this.user = response.data().name;
		});

	},
	components: {
		Navigation
	}
};

</script>
<style lang="scss">
$primary: #05b2dd;
@import "node_modules/bootstrap/scss/bootstrap"; 
</style>
