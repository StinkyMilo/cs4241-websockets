<script>
  import Bouncer from "./Bouncer.svelte";
  let bouncers = [];

  const ws = new WebSocket("ws://127.0.0.1:5000");

  // when connection is established...
  ws.onopen = () => {
    // ws.send( 'a new client has connected.' )

    ws.onmessage = async (msg) => {
      bouncers = JSON.parse(msg.data);
    };
  };

  function click(_, index) {
    const obj = { type: "split", index };
    ws.send(JSON.stringify(obj));
  }

  function rclick(e, index) {
    e.preventDefault();
    const obj = { type: "grow", index };
    ws.send(JSON.stringify(obj));
  }
</script>

{#each bouncers as bouncer, j}
  <Bouncer
    x={bouncer.x / 400}
    y={bouncer.y / 400}
    len={bouncer.size / 400}
    color={bouncer.color}
    on:click={(e) => click(e, j)}
    on:contextmenu={(e) => rclick(e, j)}
  ></Bouncer>
{/each}
