<script>
  import { onMount } from "svelte";

  const apiUrl = "https://opentdb.com/api.php?amount=10";
  let questions = [];
  let categories = [];
  let uniqueCategories = new Set(categories);

  onMount(async function () {
    const response = await fetch(apiUrl);
    let fullData = await response.json();

    questions = fullData.results;


    questions.forEach((question) => {
      categories.push(question.category);
    });

    console.log(uniqueCategories);

	let answers = []
	questions.forEach((question) => {
		answers.push(question.correct_answer)
		answers.push(question.incorrect_answers)
	})
	console.log(answers)
  });
</script>

<main>
  <h1>Svelte Trivia App</h1>
  <div>
    <h2>Choose your Difficulty</h2>
    <div>
      <button>Easy</button>
      <button>Medium</button>
      <button>Hard</button>
    </div>
  </div>
  <div>
    <h2>Choose your Category</h2>
    <div>
      <select name="category" id="category">
        <option value="all">All</option>
        <option value="all">Animals</option>
        <option value="all">Art</option>
        <option value="all">Celebrities</option>
        <option value="all">Entertainment: Books</option>
        <option value="all">Entertainment: Board Games</option>
        <option value="all">Entertainment: Cartoon & Animations</option>
        <option value="all">Entertainments: Comics</option>
        <option value="all">Entertainment: Film</option>
        <option value="all">Entertainment: Japanese Anime & Manga</option>
        <option value="all">Entertainment: Music</option>
        <option value="all">Entertainment: Musicals & Theatres</option>
        <option value="all">Entertainment: Television</option>
        <option value="all">Entertainment: Video Games</option>
        <option value="all">General Knowledge</option>
        <option value="all">Geography</option>
        <option value="all">History</option>
        <option value="all">Mythology</option>
        <option value="all">Politics</option>
        <option value="all">Science: Computers</option>
        <option value="all">Science: Gadgets</option>
        <option value="all">Science: Mathematics</option>
        <option value="all">Science & Nature</option>
        <option value="all">Sports</option>
        <option value="all">Vehicles</option>
      </select>
    </div>
  </div>

  {#each questions as question}
    {#if question.question.includes("&quot;")}
      <h3>{question.question.replaceAll("&quot;", "'")}</h3>
    {:else if question.question.includes("&#039;")}
      <h3>{question.question.replaceAll("&#039;", "'")}</h3>
    {:else if question.question.includes("&ldquo;")}
      <h3>{question.question.replaceAll("&ldquo;", "'")}</h3>
    {:else if question.question.includes("&rsquo;")}
      <h3>{question.question.replaceAll("&rsquo;", "'")}</h3>
    {:else}
      <h3>{question.question} {question.category}</h3>
    {/if}
  {/each}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
