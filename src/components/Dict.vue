<template>
	<div class="container">
		<span>Word: </span><input id="wordInput" name="wordInput" type="text" />
		<label for="languages">Language: </label>
		<select name="languages" id="languages">
			<option value="de">de</option>
			<option value="en">en</option>
		</select>
		<button v-on:click="getValueFromInputs">Start search</button>
		<div class="spacer"></div>
		<span>Gesucht nach: </span><strong>{{ globalVariableWordToSearchFor }}</strong><span> auf </span><strong>{{ searchLanguage }}</strong>
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
				languageInfo: null,
				definition: null,
				searchLanguage: 'de',
				globalVariableWordToSearchFor: '',
				starteCall: function() {
					axios.get("https://api.dictionaryapi.dev/api/v2/entries/" + this.searchLanguage + "/" + this.globalVariableWordToSearchFor).then(res => {
						if(res.request.readyState === 4 && res.status === 200) {
							this.languageInfo = res.data;
							this.definition = this.languageInfo[0].meanings[0].definitions[0].definition;
						} 
					});
				},
				getValueFromInputs: function() {
					this.globalVariableWordToSearchFor = document.getElementById('wordInput').value;
					if(this.globalVariableWordToSearchFor.length > 0) {
						this.searchLanguage = document.getElementById('languages').value;
						this.starteCall();
					}
				}
			};
		},
		created: function() {
			this.searchLanguage = 'en';
		}
	};
</script>

<style>
.spacer {
	width: 100%;
	height: 60px;
}
</style>