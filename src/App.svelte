<script>
  import TeamCounter from "./TeamCounter.svelte";

  let playing = true;
  let winningTeamName = null;
  let teams = [
    {
      id: 1,
      name: "Red",
      score: 20,
    },
    {
      id: 2,
      name: "Blue",
      score: 20,
    },
  ];

  function handleScoreUpdate(e) {
    let { id, updateAction } = e.detail;

    let foundTeam = teams.find((t) => t.id === id);

    if (playing) {
      if (updateAction === "add") {
        foundTeam.score += 1;
      }
      if (updateAction === "minus") {
        foundTeam.score -= 1;
      }

      if (foundTeam.score === 0) {
        playing = false;
        let winningTeam = teams.reduce((p, c) => {
          return p.score > c.score ? p : c;
        });
        winningTeamName = winningTeam.name;
      }
    }
    teams = teams;
  }

  function resetGame() {
    //loop through teams and reset score to 20
    let resetTeams = teams.map(({ id, name, score }) => {
      return { id, name, score: 20 };
    });
    teams = resetTeams;

    //set playing to true
    playing = true;
    winningTeamName = null
  }
</script>

<div class="container text-center ">
  <div class="row">
    <h1>MTG Counter App</h1>
  </div>
  <div class="row">
    {#each teams as team (team.id)}
      <div class="col">
        <TeamCounter {team} {playing} on:updateScore={handleScoreUpdate} />
      </div>
    {/each}
  </div>
  {#if playing}
    <div class="row mt-5">
      <button class="btn btn-warning" on:click={resetGame}>Reset Game</button>
    </div>
  {/if}
  {#if !playing}
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
