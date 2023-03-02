<script>
	import FleetManager from "../components/FleetManager.svelte";
</script>

<svelte:head>
	<title>Fleet Manager</title>
</svelte:head>

<div class="fleet-app">
	<FleetManager />
</div>

<style>
	.fleet-app {
	  padding: 0;
	  margin: 0;
	  font-family: Avenir, Helvetica, Arial, sans-serif;
	  -webkit-font-smoothing: antialiased;
	  -moz-osx-font-smoothing: grayscale;
	  text-align: center;
	  color: #2c3e50;
	}
</style>