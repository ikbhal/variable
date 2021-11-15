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
let searchResultStore = writable([]);
let variableCounter = 1;
let operation = 1; // 1 add, 2 edit
let editIndex = 0;
let  v1  = new Variable('v1', variableCounter);
let searchText = '';
let svar = new Variable('not found', 'no value');
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

function handleSearchKeyPress(){
	console.log("Inside handle search key press");
	console.log("searchText:", searchText);
	if(searchText.trim() == ""){
		$searchResultStore = [];
		svar = new Variable('not found', 'no value');
		return;
	}
	let variableStoreCopy = [... $variableStore];
	console.log("variablestore copy: ", variableStoreCopy);
	let searchResult = variableStoreCopy.filter(v => {
		// console.log("searchText: ", searchText, "; v is ", v);
		return v.name.toUpperCase().includes(searchText.toUpperCase());
		}
		// v.name.toUpperCase().contains(searchText.toUpperCase())
	);
	console.log("inside search result: ", searchResult);
	if(searchResult && searchResult.length>0){
		svar = searchResult[0];
		$searchResultStore = [...searchResult];
	}
}

function selectSearchVar(index){
	console.log("inside selectSearchVar index:", index);
	let searchResultStoreCopy = [... $searchResultStore];
	svar = searchResultStoreCopy[index];
}
</script>

<main>
	<h1>Variable</h1>
	<p>Add variable , get variable, edit variable, delete variable , list, search </p>

	<h3>Search Variable</h3>
	<form on:submit|preventDefault={handleSearchKeyPress}>
		<input type="text" autocomplete="off" 
			bind:value={searchText}	
			name="searchText" 
			on:keyup={handleSearchKeyPress}>

		<p>Suggestions</p>
		<div class="searchResults">
			{#each $searchResultStore as searchVar, index}
			<div class="searchResultVar">
				<p on:click={() => selectSearchVar(index)}>{searchVar.name}</p>
			</div>
			{/each}
		</div>
	</form>
	<p>Search Match: {svar.name} {svar.value}</p>
	<hr/>
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
.searchResults {
	/* height:50px; */
	width: 100px;
	overflow: auto;
	border: 1px solid grey;
	border-radius: 5px;
	padding: 5px;
	margin: 5px;
}
</style>