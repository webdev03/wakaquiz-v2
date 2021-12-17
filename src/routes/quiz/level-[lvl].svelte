<script context="module">
	export function load({ page }) {
		return {
			props: {
				level: page.params.lvl
			}
		};
	}
</script>

<script lang="ts">
	import { onMount } from 'svelte';
	export let level = '1';
	let questionData;
	let status = '';
	let loading = true;
	let problem,
		alreadyAnswered = false;
	let answerQuestion1, answerQuestion2, answerQuestion3, answerQuestion4;
	level = Number(level).toString();
	function generateRandomNumber(min, max) {
		// thanks StackOverflow https://stackoverflow.com/a/7228322/
		return Math.floor(Math.random() * (max - min + 1) + min);
	}
	onMount(async () => {
		loading = true;
		const answerFetch = await fetch(`answers/level${level}.json`);
		if (!answerFetch.ok) {
			problem = true;
			loading = false;
		}
		const answers = await answerFetch.json();
		loading = false;
		const generateQuestion = () => {
			let questionNumber = generateRandomNumber(0, answers.length);
			questionData = answers[questionNumber];
			alreadyAnswered = false;
		};
		generateQuestion();
		const answerBase = (num) => {
			if (alreadyAnswered) {
				status = "Your answer is now invalid, don't try to click it twice.";
			} else if (questionData['correct-answer'] == num) {
				status = 'Congratulations! You have got the correct answer!';
			} else {
				status = `Sorry, your answer is incorrect. The correct answer was ${
					questionData['answer' + questionData['correct-answer']]
				}.`;
			}
			alreadyAnswered = true;
		};
		// aliases
		answerQuestion1 = () => answerBase(1);
		answerQuestion2 = () => answerBase(2);
		answerQuestion3 = () => answerBase(3);
		answerQuestion4 = () => answerBase(4);
	});
</script>

<h2 class="text-2xl">Level {level}</h2>
<br /><br />
{#if loading}
	Loading your WakaQuiz...
{:else if problem}
	<div class="bg-red-400 w-full min-h-3 px-2 py-4 rounded text-black">
		<span class="font-semibold">Oh no!</span> It seems like an error has occurred. Please try again later.
	</div>
{:else}
	<h3 class="text-lg">{questionData.question}</h3>
	<button
		on:click={answerQuestion1}
		class="bg-blue-300 border-blue-500 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
		>{questionData['answer1']}</button
	>
	<button
		on:click={answerQuestion2}
		class="bg-blue-300 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
		>{questionData['answer2']}</button
	>
	<button
		on:click={answerQuestion3}
		class="bg-blue-300 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
		>{questionData['answer3']}</button
	>
	<button
		on:click={answerQuestion4}
		class="bg-blue-300 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
		>{questionData['answer4']}</button
	>
	<br />
	{status}
	{#if alreadyAnswered}
		<br />
		<button
			on:click={answerQuestion4}
			class="bg-blue-300 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
			>Next</button
		>
	{/if}
{/if}
