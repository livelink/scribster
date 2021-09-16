<script>
  import Artboard from './Artboard.svelte';
  import Message from './Message.svelte'

  // import io from 'socket.io-client';
  const socket = io('ws://localhost:3001');

  let inputMsg;
  let messages = [];

  function sendMessage(e) {
    e.preventDefault();
    if (inputMsg) {
      socket.emit('chat message', inputMsg);
      inputMsg = '';
    }
  }

  socket.on('chat message', function(msg) {
    messages = [...messages, msg]
    window.scrollTo(0, document.body.scrollHeight);
  });
</script>


<h1 class="heading">Scribster</h1>

<ul>
  {#each messages as message}
    <Message body={message} />
  {/each}
</ul>

<Artboard />

<form action="" method="POST" on:submit="{ sendMessage }">
  <input class="input" autocomplete="off" bind:value={ inputMsg } /><button class="button">Send</button>
</form>


<style lang="scss">
@import 'glidecss/base';
@import 'glidecss/reset';
@import 'glidecss/defaults';

@include theme((
  radius: .25rem
));

.heading {
  font-weight: theme(font, weight, bold);
  line-height: theme(font, leading, tight);
  @include screens(font-size, (font, size, 8));
  @include screens(margin-bottom, (DEFAULT: theme(spacing, 1), lg: theme(spacing, 2)));
}

.input {
  display: inline-block;
  border-radius: theme(radius);

  @include modes(palette, primary, 7) using ($color) {
    box-shadow: inset 0 0 0 1px $color;
  }

  @include screens(padding, (
    DEFAULT: theme(spacing, 2) theme(spacing, 3),
    lg: theme(spacing, 3) theme(spacing, 4)
  ));
}

.button {
  display: inline-block;
  border-radius: theme(radius);
  @include modes(color, (palette, primary, 5));
  @include modes(background, (palette, accent, 5));

  @include screens(padding, (
    DEFAULT: theme(spacing, 2) theme(spacing, 3),
    lg: theme(spacing, 3) theme(spacing, 4)
  ));

  &:hover {
    @include modes(background, (palette, accent, 6));
  }
}
</style>
