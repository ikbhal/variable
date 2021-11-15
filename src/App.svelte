<script>
let name = 'Salma';
let variableName = '';
let variableValue = '';
</script>

<main>
	<h1>Variable</h1>
	<p>Add variable , get variable, edit variable, delete variable , list, search </p>
	<h3>Add Variable</h3>
	<form>
		<label for="name">Name:</label><br>
		<input type="text" id="variableName" name="variableName" bind:value={variableName}><br>
		<label for="value">Value:</label><br>
		<input type="text" id="variableValue" name="variableValue" bind:value={variableValue}><br>
      	<input type="submit" value="Submit">
	</form>
</main>

<style>
</style>