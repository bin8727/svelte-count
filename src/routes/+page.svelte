<script lang="ts">
	import Card from "../Card.svelte";
	import _ from 'lodash';
	
	// 카드 추가 시에 갯수를 세주는 이벤트를 주기 위해 작성
	let add: boolean = false;

	// cards라는 빈 배열을 생성
	let cards: CardType[] = [];

	// 카드 벨류의 최소와 최대값 설정
	const minValue: number = 0;
	const maxValue: number = 99;

	// 카드 추가 시에 갯수를 세주는 이벤트를 주기 위해 작성
	const handleAdd = () => {
		add = true;
	};

	// 추가 버튼 클릭 시 기존의 cards 객체에 concat으로 감싼 배열을 cards에 결합
	const addCards = () => {
		cards = cards.concat({
			id: cards.length + 1, 
			value: 0,
		});
	};

	// 넘겨 받은 id와 card.id가 같으면 대상을 필터하고 새로운 배열을 만들어 리턴
	const removeCards = (id: number) => {
		cards = cards.filter((card) => card.id !== id);
	};

	// card.id가 넘겨받은 id와 같고 card.value가 최대값 미만인지 확인
	// 참이라면 ...card를 이용해 card를 복사한 후 value의 값을 1 상승
	// 거짓이라면 기존의 card를 이용
	// sortBy를 이용해 cards배열의 value 값에 대해 오름차순으로 정렬
	// value를 호출하여 최종 결과를 반환하고 cards에 할당
	const incrementCardValue = (id: number) => {
  	cards = _(cards)
  	  .map((card) =>
  	    card.id === id && card.value < maxValue
  	      ? { ...card, value: card.value + 1 }
  	      : card
  	  )
  	  .sortBy("value")
  	  .value();
	};

	// card.id가 넘겨받은 id와 같고 card.value가 최소값 이상인지 확인
	// 참이라면 ...card를 이용해 card를 복사한 후 value의 값을 1 하락
	// 거짓이라면 기존의 card를 이용
	// sortBy를 이용해 cards배열의 value 값에 대해 오름차순으로 정렬
	// value를 호출하여 최종 결과를 반환하고 cards에 할당
	const decrementCardValue = (id: number) => {
  	cards = _(cards)
			.map((card) =>
				card.id === id && card.value > minValue
				? { ...card, value: card.value - 1}
				: card
			)
			.sortBy('value')
			.value();
	};

	// reactive statement cards의 길이가 변경될 때 마다 count를 업데이트
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
	<div class="flex flex-wrap justify-start items-center my-20 mx-auto w-[40rem] rounded-lg p-4 text-center bg-[#f4f0fa]">
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