<script>
    export let placeholder
</script>

<main>
    <input type="text" placeholder="{placeholder}" class="inppt">
</main>

<style>
  .inppt {
      padding: 1rem 1.5rem;
      border: none;
      outline: none;
      font-weight: bold;
      font-size: 1.1rem;
      background: #211c1c;
      font-family: 'Ubuntu', sans-serif;

  }

  .inppt:focus {
      border: 1px solid beige;
  }
</style>