<script>
	import {createEventDispatcher} from 'svelte';
	import Button from '../shared/Button.svelte';

	let dispatch = createEventDispatcher();
	let fields = { question: '', answerA: '', answerB: '' };
	let errors = { question: '', answerA: '', answerB: '' };

	let valid = false;

	const submitHandler = () => {
		valid = true;

		if (fields.question.trim().length < 5) {
			valid = false;
			errors.question = 'Question must be at least 5 characters long';
		} else {
			errors.question = '';
		}

		if (fields.answerA.trim().length < 1) {
			valid = false;
			errors.answerA = 'This answer cant be empty';
		} else {
			errors.answerA = '';
		}

		if (fields.answerB.trim().length < 1) {
			valid = false;
			errors.answerB = 'This answer cant be empty';
		} else {
			errors.answerB = '';
		}

		// add new poll
		if(valid){
			let poll = {...fields, votesA: 0, votesB: 0, id: Math.random()}
			dispatch('add', poll);
		} else {
			//console.log(errors)
		}
	}
</script>

<form on:submit|preventDefault={submitHandler}>
	<div class="form_field">
		<label for="question">Question</label>
		<input type="text" name="question" bind:value={fields.question} id="question">
		<div class="error">{errors.question}</div>
	</div>

	<div class="form_field">
		<label for="answer-a">Answer a</label>
		<input type="text" name="answer-a" bind:value={fields.answerA} id="answer-a">
		<div class="error">{errors.answerA}</div>
	</div>

	<div class="form_field">
		<label for="answer-b">Answer b</label>
		<input type="text" name="answer-b"  bind:value={fields.answerB} id="answer-b">
		<div class="error">{errors.answerB}</div>
	</div>
	<Button flat={true} type="secondary">Add poll</Button>
</form>

<style>
	form{
		width:400px;
		margin:0 auto;
		text-align:center;
	}
	.form-field{
		margin:18px auto;
	}
	input{
		width:100%;
		border-radius:6px;
	}
	label{
		margin:10px auto;
		text-align:left;
	}
	.error{
		font-weight:bold;
		color:red;

	}
</style>
