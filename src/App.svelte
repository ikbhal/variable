<script>
import { writable } from 'svelte/store';

console.log("variable loading");
let name = 'Salma';
let variableName = '';
let variableValue = '';
class Variable {
	constructor(name, value){
		this.name = name;
		this.value = value;
	}
}
let variableArray = [];
let variableStore = writable([]);
let variableCounter = 1;
let operation = 1; // 1 add, 2 edit
let editIndex = 0;
let  v1  = new Variable('v1', variableCounter);
function handleOnSubmit(){
	console.log("inside handleOnSubmit");

	if(operation == 1){ // add new 
		variableArray.push(v1);
		$variableStore =  [... $variableStore, v1];
		variableCounter++;
		variableName = "v"+ variableCounter;
		v1 = new Variable(variableName, variableCounter);
	}else if(operation == 2){ // edit -> update
		let variableStoreCopy = [... $variableStore];
		variableStoreCopy[editIndex] = v1;
		$variableStore = [... variableStoreCopy]; 
		operation = 1; // reset back to add operation 
	}
	return false;
}
$: msg = "variable Name is " + v1.name;

function deleteVariable(index){
	console.log("inside delete variable index:" , index);
	let variableStoreCopy = [... $variableStore];
    variableStoreCopy.splice(index, 1);
	$variableStore = [...variableStoreCopy];
}
function editVariable(index){
	console.log("inside edit variabble index:", index);
	editIndex = index;
	let variableStoreCopy = [... $variableStore];
	v1 = variableStoreCopy[index];
}
</script>

<main>
	<h1>Variable</h1>
	<p>Add variable , get variable, edit variable, delete variable , list, search </p>
	<h3>Add Variable</h3>
	<form on:submit|preventDefault={handleOnSubmit}>
		<label for="name">Name:</label><br>
		<input type="text" id="variableName" name="variableName" bind:value={v1.name} autocomplete="off"><br>
		<label for="value">Value:</label><br>
		<input type="text" id="variableValue" name="variableValue" bind:value={v1.value} autocomplete="off"><br>
      	<input type="submit" value="Submit">
	</form>

	<hr/>
	<p>Variable Array</p>
	<div>
		{#each $variableStore as variable, index}
		<div class="variable">
		Name: {variable.name} = Value: {variable.value} 
		<button on:click={() => deleteVariable(index)}>delete</button> 
		<button on:click={() => editVariable(index)}>edit</button>
		</div>
		{/each}
	</div>
</main>

<style>
</style>