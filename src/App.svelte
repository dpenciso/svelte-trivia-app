<script>
  import { onMount } from "svelte";

  const apiUrl = "https://opentdb.com/api.php?amount=10";
  let questions = [];
  let categories = [];
  let uniqueCategories = [];
  onMount(async function () {
    const response = await fetch(apiUrl);
    let fullData = await response.json();

    questions = fullData.results;

    questions.forEach((question) => {
      categories.push(question.category);
    });

    uniqueCategories = categories.filter(
      (item, i, ar) => ar.indexOf(item) === i
    );

    let answers = [];
    questions.forEach((question) => {
      answers.push(question.correct_answer);
      answers.push(question.incorrect_answers);
    });
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
        {#each uniqueCategories as category}
          <option value={category}>{category}</option>
        {/each}
      </select>
    </div>
  </div>

  {#each questions as question}
    {#if question.question.includes("&quot;") || question.question.includes("&#039;") || question.question.includes("&ldquo;") || question.question.includes("&rsquo;") || question.question.includes("&minus;")}
      <h3>
        {question.question
          .replaceAll("&quot;", "'")
          .replaceAll("&#039;", "'")
          .replaceAll("&ldquo;", "'")
          .replaceAll("&rsquo;", "'")
          .replaceAll("&minus;", "-")}
      </h3>
    {:else}
      <h3>{question.question}</h3>
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
