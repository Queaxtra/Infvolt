<script lang="ts">
  import Navbar from "./components/Navbar.svelte";
  import { API } from "revolt-api";
  let userInfo = {};
  let userProfile = {};
  let userid = "01GHR0XWEA8ZNA2Z4CQ8A1VPM0";
  let client = new API({
    authentication: {
      revolt: import.meta.env.VITE_TOKEN,
    },
  });

  async function getUser() {
    let response = await client.get(`/users/${userid}`);
    userInfo = response;
    document.getElementById("status-text").innerHTML = userInfo.status.text;
    document.getElementById("avatar").src = "https://autumn.revolt.chat/avatars/" + userInfo.avatar._id + "/" + userInfo.avatar.filename;

    let responsee = await client.get(`/users/${userid}/profile`);
    userProfile = responsee;
    document.getElementById("bio").innerHTML = userProfile.content.replace(/\n/g, "<br>")
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
    <div id="profilecard" class="lg:ml-[70rem] lg:-mt-[15rem] md:ml-64 sm:ml-80 w-70 sm:w-96 bg-white rounded m-5 p-5 shadow-md">
      <p class="text-gray-500 text-xs">{userInfo._id || "012345678901234567890123456"}</p>
      <img id="avatar" src="https://i.hizliresim.com/ixqwh1j.png" class="mt-3 w-14 h-14 rounded-full" />
      <p class="absolute left-[7rem] top-[38rem] sm:left-[25.5rem] sm:top-[39.5rem] md:left-[22rem] md:top-[39.5rem] lg:left-[75.5rem] lg:top-[23rem]" id="username">{userInfo.username || "User"}</p>
      {#if userInfo.online}
      {#if userInfo.status.presence === "Online"}
      <p class="absolute text-green-500 text-xs left-[7rem] top-[39.5rem] md:left-[22rem] sm:left-[25.5rem] sm:top-[41rem] lg:left-[75.5rem] lg:top-[24.5rem]">Online</p>
      {:else if userInfo.status.presence === "Idle"}
      <p class="absolute text-yellow-500 text-xs left-[7rem] top-[39.5rem] md:left-[22rem] sm:left-[25.5rem] sm:top-[41rem] lg:left-[75.5rem] lg:top-[24.5rem]">Idle</p>
      {:else if userInfo.status.presence === "Focus"}
      <p class="absolute text-blue-500 text-xs left-[7rem] top-[39.5rem] md:left-[22rem] sm:left-[25.5rem] sm:top-[41rem] lg:left-[75.5rem] lg:top-[24.5rem]">Focus</p>
      {:else if userInfo.status.presence === "Busy"}
      <p class="absolute text-red-500 text-xs left-[7rem] top-[39.5rem] md:left-[22rem] sm:left-[25.5rem] sm:top-[41rem] lg:left-[75.5rem] lg:top-[24.5rem]">Do Not Disturb</p>
      {/if}
      {:else}
      <p class="absolute text-gray-500 text-xs left-[7rem] top-[39.5rem] md:left-[22rem] sm:left-[25.5rem] sm:top-[41rem] lg:left-[75.5rem] lg:top-[24.5rem]">Offline</p>
      {/if}
      <p id="status-text" class="text-gray-500 text-xs relative top-3 break-words">No status</p>
      <div class="flex border-l-4 border-gray-500 p-5 rounded mt-5 bg-gray-500/20 break-normal">
        <span id="bio" class="text-gray-700 text-xs">About</span>
      </div>
    </div>
    {/if}
  </div>
</div>