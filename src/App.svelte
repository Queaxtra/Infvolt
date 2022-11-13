<script>
  import Navbar from "./components/Navbar.svelte";
  import Footer from "./components/Footer.svelte";
  let userid = "01GHR0XWEA8ZNA2Z4CQ8A1VPM0";
  import { API } from "revolt-api";
  const client = new API({
    authentication: {
      revolt: "f87zd1xDzAZFyeHXbbOzE-kv7NUPWzNsdBNKooV9kw5RjjKg48UDZyzsdv5kmiXF",
    },
  });

  async function getUser() {
    client.get(`/users/${userid}`).then(x => {
      document.getElementById("username").innerHTML = x.username;
      document.getElementById("id").innerHTML = x._id;
      document.getElementById("avatar").src = "https://autumn.revolt.chat/avatars/" + x.avatar._id + "/" + x.avatar.filename;
      const online = x.online;

      if (online) {
        document.getElementById("online").innerHTML = "Online";
        document.getElementById("online").style.color = "green";        
      } else {
        document.getElementById("online").innerHTML = "Offline";
        document.getElementById("online").style.color = "gray";
      }
    });
  }
</script>

<Navbar />
<div class="max-w-xl text-center sm:text-left">
  <h1 class="text-white mt-20 text-xl font-extrabold lg:ml-80 lg:mt-20 md:ml-64 sm:text-4xl sm:ml-80 sm:mt-20">Search User</h1>
  <p class="text-gray-500 text-sm lg:ml-80 md:ml-64 sm:text-base sm:ml-80">Search for a user by their ID.</p>
  <div class="mb-3 pt-0">
    <input type="text" bind:value={userid} placeholder="User ID" class="mt-5 lg:ml-80 md:ml-64 sm:ml-80 sm:mt-5 px-3 py-3 placeholder-slate-300 text-slate-600 relative bg-white rounded text-sm border-0 shadow w-80 sm:w-96"/>
    <p class="text-red-500 text-xs mt-2 lg:ml-80 md:ml-64 sm:text-xs sm:ml-80 sm:mt-2">WARNING: If you are not on the server, <br> the bot will not show your information. <br><span class="text-green-500">https://rvlt.gg/bScHAYGx</span></p>
    {#if !userid}
    <button class="mt-5 lg:ml-80 md:ml-64 sm:ml-80 sm:mt-5 inline-block rounded opacity-50 bg-[#fe4654] px-8 py-3 text-sm font-medium text-white" disabled>Submit</button>
    {:else}
    <button on:click={getUser} class="mt-5 lg:ml-80 md:ml-64 sm:ml-80 sm:mt-5 inline-block rounded bg-[#fe4654] px-8 py-3 text-sm font-medium text-white">Submit</button>
    {/if}
    {#if getUser }
    <div class="lg:ml-[70rem] lg:-mt-[15rem] md:ml-64 sm:ml-80 w-70 sm:w-96 bg-white rounded m-5 p-5">
      <p id="id" class="text-gray-500 text-xs"></p>
      <div>
        <img id="avatar" class="flex mt-3 w-14 h-14 rounded-full" />
        <p class="absolute left-[7rem] top-[38rem] sm:left-[25.5rem] sm:top-[39.5rem] md:left-[22rem] md:top-[39.5rem] lg:left-[75.5rem] lg:top-[23rem]" id="username"></p>
      </div>
      <p id="online" class="absolute text-gray-500 text-xs left-[7rem] top-[39.5rem] md:left-[22rem] sm:left-[25.5rem] sm:top-[41rem] lg:left-[75.5rem] lg:top-[24.5rem]"></p>
    </div>
    {/if}
  </div>
</div>
<Footer />