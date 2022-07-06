<script>
	import PollStore from '../stores/PollStore.js';
	import Card from '../shared/Card.svelte';
	import Button from '../shared/Button.svelte';
	import { tweened } from 'svelte/motion';

	export let poll;

	//reactive values
	$: totalVotes = poll.votesA + poll.votesB;
	$: percentageA = Math.floor(poll.votesA / totalVotes * 100) || 0;
	$: percentageB = Math.floor(poll.votesB / totalVotes * 100) || 0;

	// tweened percentages§
	const tweenedA = tweened(0);
	const tweenedB = tweened(0);

	$: tweenedA.set(percentageA);
	$: tweenedB.set(percentageB);

	const handleVote = (option, id) => {
		PollStore.update(currentPolls => {
			let copied_polls = [...currentPolls];
			let upvoted_poll = copied_polls.find((poll) => poll.id == id);

			if(option === 'a') {
				upvoted_poll.votesA ++;
			}
			if(option === 'b') {
				upvoted_poll.votesB ++;
			}

			return copied_polls;
		})
	};

	const handleDelete = (id) => {
		PollStore.update(currentPolls => {
			return currentPolls.filter(poll => poll.id != id);
		})
	};
</script>

<Card>
	<div class="poll">
		<h3>{ poll.question }</h3>
		<p>Total votes: { totalVotes }</p>
		<div class="answer" on:click={ () => handleVote('a', poll.id) }>
			<div class="percent percent-a" style="width: {$tweenedA}%"></div>
			<span>{poll.answerA} ({ poll.votesA })</span>
		</div>
		<div class="answer" on:click={ () => handleVote('b', poll.id) }>
			<div class="percent percent-b" style="width: {$tweenedB}%"></div>
			<span>{poll.answerB} ({ poll.votesB })</span>
		</div>
		<div class="delete">
			<Button flat={true} on:click={ () => handleDelete(poll.id)}>Delete</Button>
		</div>
	</div>
</Card>

<style>
	h3{
		margin:0 auto;
		color:#555;
	}
	p{
		margin-top:6px;
		font-size:14px;
		color:#aaa;
		margin-bottom:30px;
	}
	.answer{
		background:#fafafa;
		cursor:pointer;
		margin:10px auto;
		position:relative;
	}
	.answer:hover {
		opacity:0.6;
	}
	span{
		display:inline-block;
		padding:10px 20px;
	}
	.percent{
		height:100%;
		position:absolute;
		box-sizing: border-box;
	}
	.percent-a {
		border-left: 4px solid red;
		background:rgba(217, 27, 66,0.2);
	}
	.percent-b {
		border-left: 4px solid green;
		background:rgba(69, 196, 150,0.2);
	}
	.delete{
		margin-top:30px;
		text-align:center;
	}
</style>
