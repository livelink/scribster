<script>
import { getStroke } from "perfect-freehand";
import Ably from "ably";

const ably = new Ably.Realtime({
  authUrl:'https://scribster.netlify.app/.netlify/functions/ably-auth-url',
});
const drawingChannel = ably.channels.get('drawing');

function getSvgPathFromStroke(stroke) {
  if (!stroke.length) return ""

  const d = stroke.reduce(
    (acc, [x0, y0], i, arr) => {
      const [x1, y1] = arr[(i + 1) % arr.length]
      acc.push(x0, y0, (x0 + x1) / 2, (y0 + y1) / 2)
      return acc
    },
    ["M", ...stroke[0], "Q"]
  )

  d.push("Z")
  return d.join(" ")
}

let points = [];

drawingChannel.subscribe('drawing', (event) => {
  points = [...points, JSON.parse(event.data)]
});

function handlePointerDown(e) {
  e.preventDefault();
  points = [...points, [e.pageX, e.pageY, e.pressure]];
}

function handlePointerMove(e) {
  if (e.buttons === 1) {
    e.preventDefault();
    const point = [e.pageX, e.pageY, e.pressure];
    drawingChannel.publish('drawing', JSON.stringify(point));
  }
}
</script>

<svg
on:pointerdown={handlePointerDown}
on:pointermove={handlePointerMove}
class='notouch'
>
{#each points as point}
  <path
  d={getSvgPathFromStroke(
    getStroke(points, {
      size: 24,
      thinning: 0.5,
      smoothing: 0.5,
      streamline: 0.5,
      simulatePressure: true,
      type: 'pen'
    })
  )}
  />
{/each}
</svg>

<style>
  .notouch {
    touch-action: none;
  }

  svg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: #ffffff;
  touch-action: none;
}
</style>