<template>
<div class="modal fixed w-full h-full top-0 left-0 flex items-center justify-center">
	<div class="modal-overlay absolute w-full h-full bg-gray-900 opacity-50"></div>
	
	<div class="modal-container bg-gray-900 w-11/12 md:max-w-md mx-auto rounded shadow-lg z-50 overflow-y-auto">
	
	<div class="modal-close absolute top-0 right-0 cursor-pointer flex flex-col items-center mt-4 mr-4 text-white text-sm z-50">
		<svg class="fill-current text-white" xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18">
		<path d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"></path>
		</svg>
		<span class="text-sm">(Esc)</span>
	</div>

	<!-- Add margin if you want to see some of the overlay behind the modal-->
	<div class="modal-content py-4 text-left px-6">
		<!--Title-->
		<p class="text-1xl font-bold mb-3 text-center text-white">{{title}}</p>
		<p class="text-center text-white">{{content}}</p>
		<!--Footer-->
		<div class="flex justify-end pt-2">
			<button v-on:click="onPressOk" class="px-4 bg-transparent p-3 rounded-lg text-indigo-500 hover:bg-gray-100 hover:text-indigo-400 mr-2">{{okTitle}}</button>
		</div>
		
	</div>
	</div>
</div>
</template>
<script>
export default {
	props:["title", "content", "image", "onPressOk", "okTitle"]
}
</script>


<style>
.modal {
	z-index:9;
	transition: opacity 0.25s ease;
}
body.modal-active {
	overflow-x: hidden;
	overflow-y: visible !important;
}
</style>
