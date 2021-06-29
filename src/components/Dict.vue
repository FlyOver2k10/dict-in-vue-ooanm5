<template>
	<div class="container">
		<span for="inlineFormCustomSelectPref">Enter Word: </span>
		<input id="wordInput" name="wordInput" type="text" type="text" class="form-control" id="inlineFormInputName2" v-on:keyup.enter="onButtonConfirm()" />
		<br>
		<label for="languages" for="inlineFormCustomSelectPref">Language: </label>
		<select v-model="selected" class="custom-select" id="inlineFormCustomSelect"> 
			<option v-for="language in languages" v-bind:value="language.value"> {{language.text}} </option>
		</select>
		<button v-on:click="onButtonConfirm()" class="btn btn-primary" :class="{'btn-secondary disabled' :callRunning === true}">{{searchButtonText}}</button>
		<div class="spacer"></div>
		<div id="searched-for-text-container">
				<span>Gesucht nach: </span>
				<strong>{{ globalVariableWordToSearchFor }}</strong>
				<span> auf </span>
				<strong>{{ searchLanguageSelected }}</strong>
		</div>
		<div id="searched-for-text-definition">
			<span>Definition: </span>
			<strong>{{ definition }}</strong>
		</div>
	</div>
</template>

<script>
	import axios from "axios";

	export default {
		name: "Wörterbuch",
		data() {
			return {
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
				searchButtonText: 'Start search',
				selected: 'en_US',
				searchLanguageSelected: '',
				languageInfo: null,
				definition: null,
				callRunning: false,
				globalVariableWordToSearchFor: '',
				onButtonConfirm: function() {
					this.getValueFromInputs();
					this.getSearchLanguage();
				},
				starteCall: function() {
					axios.get("https://api.dictionaryapi.dev/api/v2/entries/" + this.selected + "/" + this.globalVariableWordToSearchFor).then(res => {
						if(res.request.readyState === 4 && res.status === 200) {
							this.languageInfo = res.data;
							this.definition = this.languageInfo[0].meanings[0].definitions[0].definition;
						} 
						this.callRunning = false; // --- auslagern für den Fall, dass nicht positive Response zurückkommt
						this.searchButtonText	 = 'Start search'
					});
				},
				getValueFromInputs: function() {
					this.globalVariableWordToSearchFor = document.getElementById('wordInput').value;
					if(this.globalVariableWordToSearchFor.length > 0) {
						this.selected;
						this.callRunning = true;
				    this.searchButtonText = 'searching...'
						this.starteCall();
					}
				},
				getSearchLanguage: function () {
					this.languages.forEach((language) => {
						if(language.value === this.selected) {
							this.searchLanguageSelected = language.text;
						}
					});
					/*for(var i=0; i < this.languages.length; i++){
						if (this.languages[i].value === this.selected) {
							this.searchLanguageSelected = this.languages[i].text;
						}
					}*/
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