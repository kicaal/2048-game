<script>
	let matrix = [
		[0, 0, 0, 0],
		[0, 0, 0, 0],
		[0, 0, 0, 0],
		[0, 0, 0, 0]
	];

	let score = 0;

	const MAX_POSITIONS = 4;
	const FIRST_VALUE = 2;

	let userWins = false;
	let isGameOver = false;

	const newGame = () => {
		matrix = [
			[0, 0, 0, 0],
			[0, 0, 0, 0],
			[0, 0, 0, 0],
			[0, 0, 0, 0]
		];
		userWins = false;
		isGameOver = false;
		score = 0;
		insertEmptyPosition();
		insertEmptyPosition();
	};

	const moveLeft = () => {
		let original = matrix.map((x) => x);
		let modified = [];

		const deepClone = [...matrix];
		deepClone.forEach((clone, index) => {
			const cloneFiltered = clone.filter((item) => item !== 0).reverse();
			const arr = [];

			cloneFiltered.forEach((item, index) => {
				if (item === cloneFiltered[index + 1]) {
					cloneFiltered.splice(index + 1, 1);
					arr.push(item * FIRST_VALUE);
					score += item * FIRST_VALUE;
				} else {
					arr.push(item);
				}
			});
			arr.reverse();
			modified.push(arr);
			matrix[index] = arr;
			matrix[index].push(...Array(MAX_POSITIONS - arr.length).fill(0));
		});
		if (JSON.stringify(original) !== JSON.stringify(modified)) {
			insertEmptyPosition();
		}

		gameOver(matrix);
	};

	const moveRight = () => {
		let original = matrix.map((x) => x);
		let modified = [];
		const deepClone = [...matrix];

		deepClone.forEach((clone, index) => {
			const cloneFiltered = clone.filter((item) => item !== 0);
			const arr = [];

			cloneFiltered.forEach((item, index) => {
				if (item === cloneFiltered[index + 1]) {
					cloneFiltered.splice(index + 1, 1);
					arr.push(item * FIRST_VALUE);
					score += item * FIRST_VALUE;
				} else {
					arr.push(item);
				}
			});
			modified.push = arr;
			matrix[index] = arr;
			matrix[index].unshift(...Array(MAX_POSITIONS - arr.length).fill(0));
		});
		if (JSON.stringify(original) !== JSON.stringify(modified)) {
			insertEmptyPosition();
		}
		gameOver(matrix);
	};

	const moveDown = () => {
		let original = JSON.parse(JSON.stringify(matrix));
		let emptyMatrix = [[], [], [], []];

		matrix.forEach((item, index) => {
			item.forEach((item, index) => {
				emptyMatrix[index].push(item);
			});
		});

		emptyMatrix.forEach((item, index) => {
			let arr = item.filter((num) => num !== 0).reverse();
			let arr2 = [];

			arr.forEach((item, index) => {
				if (item === arr[index + 1]) {
					arr.splice(index + 1, 1);
					arr2.push(item * FIRST_VALUE);
					score += item * FIRST_VALUE;
				} else {
					arr2.push(item);
				}
			});
			emptyMatrix[index] = arr2.reverse();
			emptyMatrix[index].unshift(...Array(MAX_POSITIONS - arr2.length).fill(0));
		});

		emptyMatrix.forEach((item, indexEmptyMatrix) => {
			item.forEach((item, indexItem) => {
				matrix[indexItem][indexEmptyMatrix] = item;
			});
		});
		if (JSON.stringify(original) !== JSON.stringify(matrix)) {
			insertEmptyPosition();
		}
		gameOver(matrix);
	};

	const moveUp = () => {
		let original = JSON.parse(JSON.stringify(matrix));
		let emptyMatrix = [[], [], [], []];

		matrix.forEach((item, index) => {
			item.forEach((item, index) => {
				emptyMatrix[index].push(item);
			});
		});

		emptyMatrix.forEach((item, index) => {
			let arr = item.filter((num) => num !== 0);
			let arr2 = [];
			arr.forEach((item, index) => {
				if (item === arr[index + 1]) {
					arr.splice(index + 1, 1);
					arr2.push(item * FIRST_VALUE);
					score += item * FIRST_VALUE;
				} else {
					arr2.push(item);
				}
			});
			emptyMatrix[index] = arr2;
			emptyMatrix[index].push(...Array(MAX_POSITIONS - arr2.length).fill(0));
		});

		emptyMatrix.forEach((item, index1) => {
			item.forEach((item, index2) => {
				matrix[index2][index1] = item;
			});
		});
		if (JSON.stringify(original) !== JSON.stringify(matrix)) {
			insertEmptyPosition();
		}
		gameOver(matrix);
	};

	const insertEmptyPosition = () => {
		const flat = matrix.flat();

		if (flat.some((number) => number === 2048)) userWins = true;

		const emptyPositions = flat.map((item, index) => {
			if (item === 0) {
				return index;
			}
		});

		const randomIndex = Math.floor(Math.random() * Math.floor(emptyPositions.length));
		const randomNumber = emptyPositions[randomIndex];
		const positionInArray = Math.floor(randomNumber / MAX_POSITIONS);
		const numberPosition = randomNumber - positionInArray * MAX_POSITIONS;

		if (!isNaN(positionInArray) && !isNaN(numberPosition)) {
			matrix[positionInArray][numberPosition] = FIRST_VALUE;
		}

		if (!emptyPositions) {
			isGameOver = true;
		}
	};

	const gameOver = (arr) => {
		if (!arr.flat().includes(0)) {
			isGameOver = true;
		}
	};

	insertEmptyPosition();
	insertEmptyPosition();

	const keyMoves = {
		37: () => moveLeft(),
		38: () => moveUp(),
		39: () => moveRight(),
		40: () => moveDown()
	};

	const keyPress = (e) => (!userWins || !isGameOver) && keyMoves[e.keyCode]();

	const classes = {
		0: 'bg-sky-800',
		2: 'bg-sky-800',
		4: 'bg-sky-900',
		8: 'bg-orange-500',
		16: 'bg-orange-600',
		32: 'bg-red-500',
		64: 'bg-red-600',
		128: 'bg-yellow-500',
		256: 'bg-yellow-600',
		512: 'bg-green-500',
		1024: 'bg-green-600',
		2048: 'bg-violet-600'
	};
</script>

<div class="flex flex-col items-center justify-around h-full bg-sky-600">
	<div class="flex gap-4">
		<div class="bg-sky-700 text-white py-4 px-6 text-2xl rounded-md flex items-center">
			<button on:click={newGame}>New Game!</button>
		</div>
		<div class="bg-sky-700 text-white py-4 px-6 text-2xl rounded-md">
			<div>Score</div>
			<div class="text-center">{score}</div>
		</div>
	</div>
	<div class="flex flex-col relative">
		{#if userWins || isGameOver}
			<div
				class="absolute bg-sky-800 bg-opacity-60 w-full h-full text-white flex items-center justify-center text-6xl"
			>
				{userWins ? "You've won!!" : "You've lost!!"}
			</div>
		{/if}
		{#each matrix as array}
			<div class="flex bg-sky-700 gap-2 pt-4 last:pb-4 px-4 first:rounded-t-md last:rounded-b-md">
				{#each array as value}
					<div
						class={`flex justify-center items-center w-24 h-24 text-white text-5xl rounded-md ${classes[value]}`}
					>
						{value !== 0 ? value : ''}
					</div>
				{/each}
			</div>
		{/each}
	</div>
	<div class="text-white text-lg">
		Welcome to 2048 game!<br /> To PLAY: Use your arrow keys to move the tiles.<br /> The tiles are the
		same number to be able to be mixed
	</div>
</div>

<svelte:window on:keydown={keyPress} />
