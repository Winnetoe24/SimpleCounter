<script>
  import { exit } from "@tauri-apps/api/process";

  let is_control = false;
  export let value = 0;

  const keydown = async (event) => {
    if (event.key == "Escape") {
      await exit(0);
    }
    switch (event.key) {
      case "Control":
        is_control = true;
        break;
      case "c":
        if (is_control) navigator.clipboard.writeText(value+"");
        break;
      case "v":
        if (is_control)  {
          navigator.clipboard.readText().then((string) => {
            if (!isNaN(string)) {
              value = Number(string);
            }
          })
        }
        break;
    }
    console.log(event);
  };
  const keyup = (event) => {
    if (event.key == "Control") {
      is_control = false;
    }
  };
</script>

<svelte:window on:keydown={keydown} on:keyup={keyup} />

<main class="container" data-tauri-drag-region >
  <div data-tauri-drag-region>
    <span class="padding  noselect" data-tauri-drag-region>{value}</span>
    <div data-tauri-drag-region>
      <button
        class="button  noselect"
        on:click={(event) => {
          value--;
        }}>-</button
      >
      <button
        class="button noselect"
        on:click={(event) => {
          value++;
        }}>+</button
      >
    </div>
  </div>
</main>

<style>
  .padding {
    padding: 2%;
  }
  .button {
    margin: 3px;
  }
  .noselect {
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Old versions of Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently supported by Chrome, Opera and Firefox */
  }
</style>
