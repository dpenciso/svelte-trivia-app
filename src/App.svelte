<script>
  import { _ } from "lodash";

  // let category = document.querySelector("#category")
  // let difficulty = document.querySelector("#difficulty")

  // console.log(category.value)
  // let categoryUrl = `&category=${category.value}`
  // let difficultyUrl = `&difficulty=${difficulty.value}`

  let selectedDifficulty
  let selectedCategory

  let questions = [];
  let questionsToDisplay = [];

  const fetchTrivia = async () => {
    let difficultyUrl = `&difficulty=${selectedDifficulty}`
    let categoryUrl = `&category=${selectedCategory}`
    const apiUrl = `https://opentdb.com/api.php?amount=10${categoryUrl}${difficultyUrl}`;
    const response = await fetch(apiUrl);
    let fullData = await response.json();

    questions = fullData.results;

    questions.forEach((question) => {
      // all_answers.push(question.incorrect_answers);

      //array to store answers
      let answers = [];
      //add incoret ones to the array
      question.incorrect_answers.forEach((answer) =>
        answers.push({ answer: answer, correct: false })
      );
      // add the correct answer to the array
      answers.push({ answer: question.correct_answer, correct: true });

      //build question object
      let fullQuestion = {
        question: question.question,
        answers: answers,
      };

      questionsToDisplay.push(fullQuestion);
    });

    questionsToDisplay = questionsToDisplay;
    console.log(questionsToDisplay)
  };
</script>

<main>
  <h1>Ultimate Trivia Quiz</h1>
  <div>
    <h2>Choose your Difficulty</h2>
    <div>
      <select bind:value={selectedDifficulty} name="difficulty" id="difficulty">
        <option value="">All</option>
        <option value="easy">Easy</option>
        <option value="medium">Medium</option>
        <option value="hard">Hard</option>
      </select>
    </div>
  </div>
  <div>
    <h2>Choose your Category</h2>
    <div>
      <select bind:value={selectedCategory} name="category" id="category">
        <option value="">All</option>
        <option value="27">Animals</option>
        <option value="25">Art</option>
        <option value="26">Celebrities</option>
        <option value="16">Entertainment: Board Games</option>
        <option value="10">Entertainment: Books</option>
        <option value="32">Entertainment: Cartoon & Animations</option>
        <option value="29">Entertainment: Comics</option>
        <option value="11">Entertainment: Film</option>
        <option value="31">Entertainment: Japanese Anime & Manga</option>
        <option value="12">Entertainment: Music</option>
        <option value="13">Entertainment: Musicals & Theatre</option>
        <option value="14">Entertainment: Television</option>
        <option value="15">Entertainment: Video Games</option>
        <option value="9">General Knowledge</option>
        <option value="22">Geography</option>
        <option value="23">History</option>
        <option value="20">Mythology</option>
        <option value="24">Politics</option>
        <option value="18">Science: Computers</option>
        <option value="30">Science: Gadgets</option>
        <option value="19">Science: Mathematics</option>
        <option value="17">Science & Nature</option>
        <option value="21">Sports</option>
        <option value="28">Vehicles</option>
      </select>
    </div>
  </div>
  <div>
    <button on:click={fetchTrivia}>Begin Trivia</button>
  </div>
  <div class="quiz">
    {#each questionsToDisplay as question}
      <div>
        <h3>
          {@html _.unescape(question.question)}
        </h3>
        {#each question.answers as answer}
          <button>{@html _.unescape(answer.answer)}</button>
        {/each}
      </div>
    {/each}
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #00758a;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  /* .quiz {
    display: none;
  } */

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
