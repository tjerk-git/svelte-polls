<script>
	import Header from './components/Header.svelte';
	import CreatePollForm from './components/CreatePollForm.svelte';
	import Footer from './components/Footer.svelte';
	import PollList from './components/PollList.svelte';
	import Tabs from './shared/Tabs.svelte';

	// tabs
	let items = ['Current polls', 'Add new poll'];
	let activeItem = 'Current polls';

	const tabChange = (e) => {
		activeItem = e.detail;
	}

	let polls = [
		{
			id: 1,
			question: 'Python or Javascript?',
			answerA: 'Python',
			answerB: 'Javascript',
			votesA: 9,
			votesB: 15
		},
	];

	const handleAdd = (e) => {
		const poll = e.detail;
		polls = [poll, ...polls]
		activeItem = 'Current polls';
	}

	const handleVote = (e) => {
		// destructure
		const { id, option } = e.detail;
		let copied_polls = [...polls];
		let upvoted_poll = copied_polls.find((poll) => poll.id == id);

		if(option === 'a') {
			upvoted_poll.votesA ++;
		}
		if(option === 'b') {
			upvoted_poll.votesB ++;
		}

		polls = copied_polls;
	}
</script>

<Header/>

<main>
	<Tabs {activeItem} {items} on:tabChange={tabChange}/>
	<h1>Hello Tjerk</h1>
	{#if activeItem === 'Current polls'}
		<PollList {polls} on:vote={handleVote} />
	{:else if activeItem === 'Add new poll'}
		<CreatePollForm on:add={handleAdd}/>
	{/if}
</main>

<Footer/>

<style>
	main{
		max-width:960px;
		margin: 40px auto;
	}
</style>
