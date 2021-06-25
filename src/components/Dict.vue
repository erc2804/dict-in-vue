<template>
	<div class="container">
		<span class="my-1 mr-2" for="inlineFormCustomSelectPref">Enter Word: </span><input id="wordInput" name="wordInput" type="text" type="text" class="form-control mb-2 mr-sm-2" id="inlineFormInputName2" /><br>
		<label for="languages" class="my-1 mr-2" for="inlineFormCustomSelectPref">Language: </label>
		<select v-model ="selected" class="custom-select mr-sm-2" id="inlineFormCustomSelect"> 
	<option v-for = "language in languages" v-bind:value="language.value"> {{language.text}} </option>
		</select>
		<button v-on:click="getValueFromInputs" class="btn btn-primary my-1">Start search</button>
		<div class="spacer"></div>
		<span>Gesucht nach: </span><strong>{{ globalVariableWordToSearchFor }}</strong><span> auf </span><strong>{{ languages.text }}</strong>
		<strong>Definition</strong>
		<span>{{ definition }}</span>
	</div>
</template>

<script>
	import axios from "axios";

	export default {
		name: "Wörterbuch",
		data() {
			return {
				selected: '',
				languages: [
					{text: 'English (US)', value: 'en_US'},
					{text: 'Hindi', value: 'hi'},
					{text: 'Spanish', value: 'es'},
					{text: 'French', value: 'fr'},
					{text: 'Japanese', value: 'ja'},
					{text: 'Russian', value: 'ru'},
					{text: 'English (GB)', value: 'en_GB'},
					{text: 'Deutsch', value: 'de'}
				],
				languageInfo: null,
				definition: null,
				globalVariableWordToSearchFor: '',
				starteCall: function() {
					axios.get("https://api.dictionaryapi.dev/api/v2/entries/" + this.selected + "/" + this.globalVariableWordToSearchFor).then(res => {
						if(res.request.readyState === 4 && res.status === 200) {
							this.languageInfo = res.data;
							this.definition = this.languageInfo[0].meanings[0].definitions[0].definition;
						} 
					});
				},
				getValueFromInputs: function() {
					this.globalVariableWordToSearchFor = document.getElementById('wordInput').value;
					if(this.globalVariableWordToSearchFor.length > 0) {
						this.selected;
						this.starteCall();
					}
				}
			};
		},
	};
</script>

<style>
.spacer {
	width: 100%;
	height: 60px;
}
</style>