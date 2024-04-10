<template>
  <div class="avatar-container">
    <div class="img">
      <img
        src="https://assets.codepen.io/1480814/av+1.png"
        alt="a random avatar picture"
      />
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.avatar-container {
  width: 100px;
  height: 100px;
  /* background: blue; */
  margin: 0 20px;
  flex-shrink: 0;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.5);
  border-radius: 50%;
}

img {
  --s: 100%; /* image size */
  --b: 5px; /* border thickness */
  --c: #ffda61; /* border color */
  --f: 1; /* initial scale */

  width: var(--s);
  aspect-ratio: 1;
  padding-top: calc(var(--s) / 5);
  cursor: pointer;
  border-radius: 0 0 999px 999px;
  --_g: 50% / calc(100% / var(--f)) 100% no-repeat content-box;
  --_o: calc((1 / var(--f) - 1) * var(--s) / 2 - var(--b));
  outline: var(--b) solid var(--c);
  outline-offset: var(--_o);
  background: radial-gradient(
      circle closest-side,
      #fefced calc(99% - var(--b)),
      var(--c) calc(100% - var(--b)) 99%,
      #0000
    )
    var(--_g);
  -webkit-mask: linear-gradient(#000 0 0) no-repeat 50% calc(1px - var(--_o)) /
      calc(100% / var(--f) - 2 * var(--b) - 2px) 50%,
    radial-gradient(circle closest-side, #000 99%, #0000) var(--_g);
  transform: scale(var(--f));
  transition: 0.5s;
}
img:hover {
  --f: 1.35; /* hover scale */
}

.img {
  display: flex;
  height: 100%;
  justify-content: center;
  align-items: center;
}
img {
  display: block;
  padding-bottom: calc(var(--s) / 5);
  user-select: none;
}

/* body {
  margin: 0;
  min-height: 100vh;
  display: grid;
  place-content: center;
  background: #e0e4cc;
} */
</style>
