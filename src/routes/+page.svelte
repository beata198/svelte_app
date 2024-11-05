<script lang="ts">
	let view = $state('showPolls');
	let pollName = $state('');
	let optionA = $state('');
	let optionB = $state('');
	let polls = $state([
		{
			id: 1,
			name: 'Python or JavaScript',
			optionA: 'JavaScript',
			optionB: 'Python',
			votesA: 10,
			votesB: 5
		},
		{ id: 2, name: 'Xbox or PS', optionA: 'Xbox', optionB: 'PS', votesA: 0, votesB: 3 }
	]);

	function showPolls() {
		view = 'showPolls';
	}

	function addNewPoll() {
		view = 'addPoll';
	}

	function deletePoll(id: Number) {
		polls = polls.filter((poll) => poll.id !== id);
	}

	function handleAddPoll(e: Event) {
		e.preventDefault();
		polls.push({
			id: polls.length + 1,
			name: pollName,
			optionA: optionA,
			optionB: optionB,
			votesA: 0,
			votesB: 0
		});

		pollName = '';
		optionA = '';
		optionB = '';
		view = 'showPolls';
	}

	function addVote(vote: String, id: Number) {
		const newPolls = polls.map((poll) => {
			if (poll.id === id) {
				return {
					...poll, // Kopiujemy istniejące właściwości ankiety
					votesA: vote === 'A' ? poll.votesA + 1 : poll.votesA, // Zwiększamy `votesA` jeśli `vote === 'A'`
					votesB: vote === 'B' ? poll.votesB + 1 : poll.votesB // Zwiększamy `votesB` jeśli `vote === 'B'`
				};
			}
			return poll;
		});
		polls = newPolls;
	}
</script>

<div class="rounded-md bg-gray-200 px-6 py-10">
	<div class="flex justify-center gap-8">
		<button
			class="relative after:absolute after:-bottom-1 after:left-0 after:h-[2px] after:w-full after:content-[''] hover:text-red-500 hover:after:bg-red-500"
			onclick={showPolls}>Show Polls</button
		>
		<button
			class="relative after:absolute after:-bottom-1 after:left-0 after:h-[2px] after:w-full after:content-[''] hover:text-red-500 hover:after:bg-red-500"
			onclick={addNewPoll}>Add New Poll</button
		>
	</div>
	<div>
		{#if view === 'showPolls'}
			<div class="mt-10 grid grid-cols-2 gap-4">
				{#each polls as { id, name, votesA, votesB, optionA, optionB }, i}
					<div class="flex flex-col gap-4 rounded-md bg-gray-100 p-4 shadow-md">
						<div>
							<h3 class="font-bold">{name}?</h3>
							<p>Total votes: {votesA + votesB}</p>
						</div>
						<div class="flex flex-col gap-2">
							<button
								style="width: {Math.floor((100 / (votesA + votesB)) * votesA)}%"
								onclick={() => addVote('A', id)}
								class="relative min-w-fit bg-red-200 p-3 text-left before:absolute before:left-0 before:top-0 before:h-full before:w-[5px] before:bg-red-500 before:content-[''] hover:opacity-85"
								>{optionA} ({votesA} votes)</button
							>
							<button
								style="width: {Math.floor((100 / (votesA + votesB)) * votesB)}%"
								onclick={() => addVote('B', id)}
								class="relative min-w-fit bg-green-200 p-3 text-left before:absolute before:left-0 before:top-0 before:h-full before:w-[5px] before:bg-green-500 before:content-[''] hover:opacity-85"
								>{optionB} ({votesB} votes)</button
							>
						</div>
						<button
							class="m-auto mt-3 rounded-md bg-red-500 px-4 py-1 text-sm font-semibold uppercase text-white"
							onclick={() => deletePoll(id)}>Delete</button
						>
					</div>
				{/each}

				{#if polls.length === 0}
					<p>No polls available.</p>
				{/if}
			</div>
		{:else if view === 'addPoll'}
			<form class="m-auto flex w-1/2 flex-col gap-3 py-14" onsubmit={handleAddPoll}>
				<label class="flex w-full flex-col gap-2">
					Poll Name:
					<input
						class="rounded-md border border-b-[1px] border-gray-300"
						type="text"
						name="pollName"
						bind:value={pollName}
						required
					/>
				</label>
				<label class="flex w-full flex-col gap-2">
					Option A: <input
						class="rounded-md border border-b-[1px] border-gray-300"
						type="text"
						name="optionA"
						bind:value={optionA}
						required
					/></label
				>
				<label class="flex w-full flex-col gap-2">
					Option B: <input
						class="rounded-md border border-b-[1px] border-gray-300"
						type="text"
						name="optionB"
						bind:value={optionB}
						required
					/></label
				>

				<button
					class="m-auto w-fit rounded-md bg-green-700 px-4 py-1 text-sm font-semibold text-white"
					type="submit">Add Poll</button
				>
			</form>
		{/if}
	</div>
</div>
