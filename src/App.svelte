<script lang="ts">
  interface Pilot {
    firstName: string;
    lastName: string;
    phoneNumber: string;
    email: string;
    lastBroken: number;
    closestDistanceToNest: number;
  }

  interface Pilots {
    [pilotId: string]: Pilot;
  }

  const updateInterval = 2000; // how often should the data be updated

  let pilots: Pilots = {};

  async function fetchData() {
    // TODO: update to websockets
    pilots = await (
      await fetch("https://birdnest-backend-vizitys.fly.dev", {
        cache: "no-store",
      })
    ).json();
  }

  setInterval(fetchData, updateInterval);
</script>

<main>
  <div class="about">
    <h1>Birdnest</h1>
    <p>
      My solution for the Reaktor Birdnest challenge, backend available <a
        href="https://github.com/Vizitys/birdnest-backend">here</a
      >
    </p>
    <p>
      Check out my <a href="https://github.com/Vizitys">Github</a> and
      <a href="https://vinski.fi">WIP homepage</a>
    </p>
  </div>

  <table>
    <thead>
      <th>Last broke the perimeter</th>
      <th>Closest distance to the nest</th>
      <th>Name</th>
      <th>Email</th>
      <th>Phone Number</th>
    </thead>
    <tbody>
      {#each Object.entries(pilots) as [pilotId, pilot]}
        <tr>
          <td>
            {new Date(Date.now() - pilot.lastBroken).getMinutes()}m
            {new Date(Date.now() - pilot.lastBroken).getSeconds()}s ago
          </td>
          <td>{Math.round(pilot.closestDistanceToNest / 1000)}m away</td>
          <td>{pilot.lastName} {pilot.firstName}</td>
          <td>{pilot.email}</td>
          <td>{pilot.phoneNumber}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</main>

<style>
  th {
    padding: 0 10px 0 10px;
  }
</style>
