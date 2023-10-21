<script>
  import { spring } from "svelte/motion";

  let value = 0;
  const valueSpring = spring(value);
  $: valueSpring.set(value);

  $: offset = modulo($valueSpring, 1);

  function modulo(n, m) {
    // handle negative numbers
    return ((n % m) + m) % m;
  }
</script>

<input type="number" bind:value />

<div class="viewport">
  <div class="wheel" style="transform: translateX({-offset * 35}px)">
    <div class="number previous" style="transform:scale({3 + offset})">
      {Math.floor($valueSpring - 1)}
    </div>
    <div class="number current" style="transform:scale({2 + offset})">
      {Math.floor($valueSpring)}
    </div>
    <div class="number next" style="transform:scale({1 + offset})">
      {Math.floor($valueSpring + 1)}
    </div>
    <div class="number incoming">
      {Math.floor($valueSpring + 2)}
    </div>
  </div>
</div>

<style>
  .viewport {
    width: 100px;
    height: 50px;
    border: solid 1px red;
    overflow: hidden;
  }

  .wheel {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  .number {
    position: absolute;
    width: 20px;
  }

  .number.previous {
    left: 5px;
  }

  .number.next {
    right: 5px;
  }

  .number.incoming {
    right: -30px;
  }
</style>
