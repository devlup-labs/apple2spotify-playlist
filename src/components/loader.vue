<template>
<div id="loader" v-if="clicked">
	<center>
		<center>
			<orbit-spinner
			:animation-duration="1200"
			:size="55"
			color="#ff1d5e"
			/>
		</center>
	</center>
</div>
</template>

<script>


import { OrbitSpinner } from 'epic-spinners'

export default {
	name: "loader",
	components: {
		OrbitSpinner
	},

	props: ['clicked']
}

</script>

<style>

#loader {
  width: 10%;
  background-color: lightgray;
  padding-top: 20px;
  padding-bottom: 20px;
}

</style>