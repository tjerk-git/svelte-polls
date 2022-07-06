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

	const handleAdd = (e) => {
		activeItem = 'Current polls';
	}

</script>

<Header/>

<main>
	<Tabs {activeItem} {items} on:tabChange={tabChange}/>
	<h1>Hello Tjerk</h1>
	{#if activeItem === 'Current polls'}
		<PollList />
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
