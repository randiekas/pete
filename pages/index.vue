<template>
  <div class="container">
    <img src="https://prod-notion-assets.s3-us-west-2.amazonaws.com/front/product/hero.png" class="w-100" style="max-width: 500px;">
	<h1 class="text-5xl leading-none font-extrabold text-gray-900 tracking-tight mb-4 mt-10">Personal Teacher</h1>
	<p class="text-2xl tracking-tight mb-10">Belajar dengan guru hebat (bot) <b>gratis</b></p>
	<a href="https://t.me/guru_private_bot" class="mt-4 relative sm:mt-0 sm:h-auto sm:ml-4 block w-full sm:w-auto border border-transparent px-6 py-3 text-base font-semibold leading-snug bg-gray-900 text-white rounded-md shadow-md hover:bg-gray-800 focus:outline-none focus:bg-gray-800 transition ease-in-out duration-150 hover:bg-gray-600">
        Mulai Gratis
    </a>
  </div>
</template>

<script>
export default {}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
