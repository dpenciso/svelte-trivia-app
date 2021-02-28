<script>
  import { _ } from "lodash";

  let selectedDifficulty;
  let selectedCategory;

  let questions = [];
  let questionsToDisplay = [];

  const fetchTrivia = async () => {
    let difficultyUrl = `&difficulty=${selectedDifficulty}`;
    let categoryUrl = `&category=${selectedCategory}`;
    const apiUrl = `https://opentdb.com/api.php?amount=10${categoryUrl}${difficultyUrl}`;
    const response = await fetch(apiUrl);
    let fullData = await response.json();

    questions = fullData.results;

    questions.forEach((question) => {
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
        answers: _.shuffle(answers),
      };
      questionsToDisplay.push(fullQuestion);
    });
    questionsToDisplay = questionsToDisplay;
    console.log(questionsToDisplay);

    const error = document.querySelector(".error");
    error.innerText =
      "Sorry! It looks like there are no questions that match your search. Try a different difficulty or category.";
  };

  let incorrectScore = 0;
  let correctScore = 0;

  function incorrectAnswer() {
    incorrectScore++;
  }

  function correctAnswer() {
    correctScore++;
  }
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
    {#if questionsToDisplay.length === 0}
      <div>
        <h3 class="error">
          Wecome to the Trivia App! Please select a difficulty and category and
          then click "Begin Trivia"!
        </h3>
      </div>
    {:else}
      {#each questionsToDisplay as question}
        <div class="question">
          <h3>
            {@html _.unescape(question.question)}
          </h3>
          {#each question.answers as answer}
            <div class="answers">
              {#if answer.correct === false}
                <button on:click={incorrectAnswer}
                  >{@html _.unescape(answer.answer)}</button
                >
              {:else}
                <button on:click={correctAnswer}
                  >{@html _.unescape(answer.answer)}</button
                >
              {/if}
            </div>
          {/each}
        </div>
      {/each}
    {/if}
  </div>
  <div class="scoreboard">
    <div>
      <h2>Correct Answers</h2>
      <p>{correctScore}</p>
    </div>
    <div>
      <h2>Incorrect Answers</h2>
      <p>{incorrectScore}</p>
    </div>
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

  button {
    margin: 1rem;
    color: black;
  }

  .scoreboard {
    position: fixed;
    right: 0;
    top: 50%;
    border: black solid 1px;
    background-color: blue;
    color: white;
  }

  .question {
    border: black solid 1px;
    margin: 1rem auto;
    width: 50%;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
