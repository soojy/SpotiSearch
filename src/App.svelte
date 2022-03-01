<script>
  import Button from './components/Button.svelte'
  let client_id = 'daf0f9a4cece44c99e92113c2975c7ef'
  let client_secret = '29bf5e346a124b859ff51732b6fa18df'
  let searchValue = ''
  let access_token = ''
  let items = {}
  let loading = false
  let modal = false
  localStorage.getItem('access_token') !== null
    ? (access_token = localStorage.getItem('access_token'))
    : authApi()
  console.log(access_token)
  function authApi() {
    fetch('https://accounts.spotify.com/api/token', {
      method: 'POST',
      headers: {
        Authorization: 'Basic ' + btoa(client_id + ':' + client_secret),
        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
      },
      body: new URLSearchParams({
        grant_type: 'client_credentials',
      }),
    })
      .then((res) => {
        return res.json()
      })
      .then((json) => {
        access_token = json.access_token
        localStorage.setItem('access_token', access_token)
        console.log(access_token)
      })
      .catch((err) => {
        console.error(err)
      })
  }

  function search() {
    loading = true
    if (searchValue !== '') {
      access_token == '' ? authApi() : ''
      fetch(`https://api.spotify.com/v1/search?q=${searchValue}&type=album%2Cartist%2Ctrack`, {
        method: 'GET',
        headers: {
          Authorization: `Bearer ${access_token}`,
        },
      })
        .then((res) => {
          return res.json()
        })
        .then((json) => {
          console.log(json)
          items.tracks = json.tracks.items
          items.albums = json.albums.items
          items.artists = json.artists.items
          loading = false
        })
        .catch((err) => {
          console.error(err)
        })
    } else {
    }
  }
</script>

<main>
  <div class="header flex">
    <div class="logo">
      <a href="https://github.com/soojy">
        <img src="images/github-icon.svg" alt="" >
      </a>
    </div>
    <div class="search-input flex">
      <input bind:value="{searchValue}" type="text" class="field" placeholder="🎧" />
      <Button on:click="{search}" text="Поиск" icon="🔎" />
      
    </div>
    <div  class="about"><button on:click="{()=>{modal=true}}">О разработчике</button></div>
  </div>
  {#if modal}
    <div class="modal">
      <div class="title">О разработчике</div>
      <div class="link">Ариков Ярослав Вячеславович</div>
      <div class="link">25.02.2022</div>
      <div class="link"><a href="https://api.spotify.com/v1/" class="link">API</a></div>
      <div class="link"><Button on:click="{()=>{modal=false}}" text="Закрыть" icon="❌" /></div>
    </div>
    {/if}
  <div class="container">
    {#if loading}
      <div class="loading">ЗАГРУЗКА...</div>
      {/if}
      {#if items.tracks?.length > 0}
    <div class="title">Треки</div>
    <div class="card-album-wrapper" >
      {#each items.tracks as item, i}
        <a href="{item.external_urls.spotify}" class="card-album">
          <div class="img">
            <img src="{item.album.images[0].url}" alt="" >
          </div>
          <div class="card-album-text">
            <div class="card-album-title">{item.name}</div>
            <div class="card-album-subtitle">{item.artists[0].name}</div>
          </div>
        </a>
      {/each}
    </div>
    {/if}

    {#if items.albums?.length > 0}
    <div class="title">Альбомы</div>
    <div class="card-album-wrapper">
      {#each items.albums as item, i}
        <a href="{item.external_urls.spotify}" class="card-album">
          <div class="img">
            <img src="{item.images[0].url}" alt="" >
          </div>
          <div class="card-album-text">
            <div class="card-album-title">{item.name}</div>
            <div class="card-album-subtitle">{item.artists[0].name}</div>
          </div>
        </a>
      {/each}

    </div>
    {/if}
      {#if items.artists?.length > 0}
    <div class="title">Исполнители</div>
    <div class="card-album-wrapper">
      {#each items.artists as item, i}
        <a href="{item.external_urls.spotify}" class="card-album">
          <div class="img">
            <img src="{item.images[0]?.url || ''}" alt="" >
          </div>
          <div class="card-album-text">
            <div class="card-album-title">{item.name}</div>
            <div class="card-album-subtitle">{item.name}</div>
          </div>
        </a>
      {/each}

    </div>
      {/if}
  </div>
</main>


<style>
  .link {
    color: white;
    font-size: 1.2rem;
    font-weight: normal;
    text-align: center;
    display: flex;
    justify-content: center;
    margin: .3rem;
    align-items: center;
  }
  .modal > .title {
    margin: .5rem;
    text-align: center;
  }

  .modal {
    z-index: 999;
    background: rgba(62, 67, 109, 1);
    border: 0.5px solid #4300b000;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.14), 0 1px 5px rgba(0, 0, 0, 0.12);
    font-weight: 500;
    outline: none;
    border-radius: 1.3rem;
    position: absolute;
    color: white;
    font-size: 1.1rem;
    padding: 1.5em 1.7em;
    transition: 0.3s;
    left: 0;
    right: 0;
    top: 43vh;
    max-height: 800px;
    width: 300px;
    margin: 0 auto;
    overflow: hidden;
  }
  .loading {
    color: white;
    font-size: 1rem;
    margin: 2rem;
    text-align: center;
  }
  .card-album-wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
  }
  .card-album {
    width: 250px;
    height: 250px;
    padding: 15px;
    background: #3e436d4f;
    display: block;
    position: relative;
    border-radius: 1.3rem;
    margin: 1rem;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.14), 0 1px 5px rgba(0, 0, 0, 0.12);

    overflow: hidden;
  }
  .card-album img {
    object-fit: cover;
    width: 100%;
    height: 100%;
    border-radius: 1.3rem;
    transition: 0.4s;
  }
  .card-album-text {
    max-width: 60%;
    margin: 0 auto  ;
    text-align: center;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    color: transparent;

    font-weight: bold;
    transition: 0.4s;
  }
  .card-album-title {
    font-size: 1.1rem;
  }
  .card-album-subtitle {
    font-size: 0.7rem;
  }
  .card-album:hover .img img {
    transform: scale(1.3);
    filter: blur(4px);
  }
  .card-album:hover .card-album-text {
    color: white;
    transform: scale(1.2);

  }
  .container {
    width: 90%;
    max-width: 1170px;
    margin-left: auto;
    margin-right: auto;
  }
  .title {
    color: white;
    font-size: 1.5rem;
    font-weight: bold;
    margin-top: 2.5rem;
    margin-left: 1rem;
  }
  .flex {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .logo {
    width: 33%;
    margin-right: auto;
  }
  .search-input {
    width: 33%;
  }
  .logo img {
    max-width: 100px;
    /* max-height: 3rem; */
  }
  .about {
    margin-left: auto;
    text-align: right;
    width: 33%;
  }
  button {
    background: none;
    border: none;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.14), 0 1px 5px rgba(0, 0, 0, 0.12);

    font-family: 'Ubuntu', sans-serif;
    font-weight: 500;
    outline: none;
    color: white;
    font-size: 1.3rem;
    cursor: pointer;
  }
  .field {
    background: #3e436d4f;
    border: 0.5px solid #4300b000;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.14), 0 1px 5px rgba(0, 0, 0, 0.12);
    align-items: center;
    cursor: pointer;
    display: flex;

    font-weight: 500;
    outline: none;
    border-radius: 1.3rem;

    color: white;
    font-size: 1.1rem;
    padding: 0.8em 1.7em;
    transition: 0.3s;
    position: relative;
    overflow: hidden;
    margin-right: -40px;
  }
  .field:focus {
    border: 0.5px solid #4300b0;
  }
  main {
    padding: 1.5rem;
  }
</style>
