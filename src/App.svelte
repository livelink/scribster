<script>
  import Artboard from './Artboard.svelte';
</script>

<h1 class="heading">Scribster</h1>

<Artboard />

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
