<script>
	// component imports
	import Button from '$lib/components/Button.svelte';
</script>

<p>
	Hello, Waka Family! Test out your Kiwi Quiz knowledge using this fun web application. Ready to go?
</p>
<br />
<span class="text-2xl">Instructions</span>
<p>The higher the level, the harder the questions.</p>
<br />
<span class="text-2xl">Quizzes</span> <br /> <br />
<Button goTo="/quiz/level-1">Level 1</Button>
<br /> <br />
<Button goTo="/quiz/level-2">Level 2</Button> <br /> <br />
