<script>
let name = 'Salma';
let variableName = '';
let variableValue = '';
class Variable {
	constructor(name, value){
		this.name = name;
		this.value = value;
	}
}
let  v1  = new Variable('v1', '');
</script>

<main>
	<h1>Variable</h1>
	<p>Add variable , get variable, edit variable, delete variable , list, search </p>
	<h3>Add Variable</h3>
	<form>
		<label for="name">Name:</label><br>
		<input type="text" id="variableName" name="variableName" bind:value={v1.name}><br>
		<label for="value">Value:</label><br>
		<input type="text" id="variableValue" name="variableValue" bind:value={v1.value}><br>
      	<input type="submit" value="Submit">
	</form>
</main>

<style>
</style>