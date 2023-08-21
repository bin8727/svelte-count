<script lang="ts">
	import Card from "../Card.svelte";
	import _ from 'lodash';
	
	let add: boolean = false;
	let cards: CardType[] = [];
	const minValue: number = 0;
	const maxValue: number = 99;

	const handleAdd = () => {
		add = true;
	};

	const addCards = () => {
		cards = cards.concat({
			id: cards.length + 1, 
			value: 0,
		});
	};

	const removeCards = (id: number) => {
		cards = cards.filter((card) => card.id !== id);
	};

	const incrementCardValue = (id: number) => {
  	cards = cards.map((card) => {
    	if (card.id === id && card.value < maxValue) {
    	  const incrementedCard = { ...card, value: card.value + 1 };
    	  return incrementedCard;
    	}

    	return card;
  	});

  	cards = _.sortBy(cards, [(card) => card.value]);
	};

	const decrementCardValue = (id: number) => {
  	cards = cards.map((card) => {
    	if (card.id === id && card.value > minValue) {
    	  const decrementedCard = { ...card, value: card.value - 1 };
    	  return decrementedCard;
    	}

    	return card;
  	});

  	cards = _.sortBy(cards, [(card) => card.value]);
	};

	$: count = cards.length;
</script>

<div class="my-20 mx-auto w-[40rem] rounded-lg p-4 text-center bg-[#f4f0fa]">
	<h1 class="text-[#575757] m-0 text-base font-semibold">
		테스트용 과제
	</h1>

	{#if add}
	<div class="text-3xl font-bold text-[#3c0080] my-8 mx-0">
		{count}개의 카드
	</div>
	{/if}

	<button 
		on:click={() => {
			handleAdd(); 
			addCards();
		}}
		class="cursor-pointer bg-[#3c0080] border-2 border-solid border-[#3c0080] text-white px-6 py-2 rounded-md my-0 mx-4"
	>
		추가
	</button>
</div>

{#if add}
	<div class="flex flex-wrap justify-center items-center my-20 mx-auto w-[40rem] rounded-lg p-4 text-center bg-[#f4f0fa]">
		{#each cards as card}
			<Card 
				card={card} 
				remove={() => removeCards(card.id)}
				incrementValue={() => incrementCardValue(card.id)}
				decrementValue={() => decrementCardValue(card.id)}
			/>
		{/each}
	</div>
{/if}