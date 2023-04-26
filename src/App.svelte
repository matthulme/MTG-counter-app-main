<script>
  import TeamCounter from "./TeamCounter.svelte";

  let teams = [
    {
      id: 20,
      name: "Red",
      score: 20,
      disabled: false,
    },
    {
      id: 2,
      name: "Blue",
      score: 20,
      disabled: false,
    },
    {
      id: 3,
      name: "Green",
      score: 20,
      disabled: false,
    },
  ];

  let gameOver = false;
  let teamsPlaying = teams.length;
  let winningTeamName = null;

  function findWinningTeamName(array) {
    const result = array.find((element) => element.score > 0);
    return result ? result.name : undefined;
  }

  function handleScoreUpdate(e) {
    //destructure team id and update action from event
    let { id, updateAction } = e.detail;

    //find the correct team in the array
    let foundTeam = teams.find((t) => t.id === id);

    //if the game is still going update the found teams score
    if (!gameOver) {
      if (updateAction === "add") {
        foundTeam.score += 1;
      }
      if (updateAction === "minus") {
        foundTeam.score -= 1;
      }
      //if the updated score for that team equals 0, disable them, reduce the number of teams playing by 1
      if (foundTeam.score === 0) {
        foundTeam.disabled = true;
        teamsPlaying -= 1;

        //if there is only 1 team remaining, they win - set gameOver to true
        if (teamsPlaying <= 1) {
          gameOver = true;
          winningTeamName = findWinningTeamName(teams);
        }
      }
    }
    //require equals sign for svelte to update the DOM
    teams = teams;
  }

  function resetGame() {
    //loop through teams and reset score to 20
    let resetTeams = teams.map(({ id, name, score, disabled }) => {
      return { id, name, score: 20, disabled: false };
    });
    teams = resetTeams;

    //set playing to true
    gameOver = false;
    winningTeamName = null;
    teamsPlaying = teams.length;
  }
</script>

<div class="container text-center">
  <div class="row">
    <h1>MTG Counter App</h1>
  </div>
  <div class="row">
    {#each teams as team (team.id)}
      <div class="col mb-3">
        <TeamCounter {team} {gameOver} on:updateScore={handleScoreUpdate} />
      </div>
    {/each}
  </div>
  {#if !gameOver}
    <div class="row mt-2">
      <button class="btn btn-warning" on:click={resetGame}>Reset Game</button>
    </div>
  {/if}
  {#if gameOver}
    <div class="row text-center mt-5">
      <h3 class="mb-3" style="color: {winningTeamName.toLowerCase()};">
        Team {winningTeamName} Wins!
      </h3>
    </div>
    <div class="row">
      <button class="btn btn-success" on:click={resetGame}>New Game</button>
    </div>
  {/if}
</div>

<style>
  @media (min-width: 768px) {
    .col {
      max-width: 50%;
    }
  }
</style>
