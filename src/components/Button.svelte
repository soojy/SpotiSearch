<script>
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()
  export let text, icon
  function click() {
    dispatch('click')
  }
</script>

<main>
  <button on:click={click} class="btn">{text} <span>{icon}</span></button>
</main>

<style>
  .btn {
    background: linear-gradient(0deg, #4300b0 0%, #5000d1 100%);
    border: none;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.14), 0 1px 5px rgba(0, 0, 0, 0.12);
    align-items: center;
    cursor: pointer;
    display: flex;
    font-family: 'Ubuntu', sans-serif;
    font-weight: 500;
    outline: none;
    border-radius: 1.3rem;
    color: white;
    font-size: 1.1rem;
    padding: 0.8em 1.9em;
    transition: 0.3s;
    position: relative;
    overflow: hidden;

  }
  .btn:hover {
    cursor: pointer;
    color: transparent;
  }

  .btn span {
    position: absolute;
    top: 100%;
    left: 0;
    transition: 150ms ease;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .btn:hover span {
    top: 50%;
    transform: translateY(-50%);
    color: white;
  }
</style>
