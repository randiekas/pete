<template>
	<div class="py-4 flex justify-between flex-col flex-1 h-screen">
	<Modal 
		v-if="popup" 
		:title="popupTitle"
		:content="popupContent"
		:image="popupImage"
		:onPressOk="handleTutupPopup"
		okTitle="Ok"/>
	<div class="max-w-8xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
    <div class="pb-2">
		<!-- untuk indikator posisi materi -->
		<nav class="flex items-center text-sm font-medium">
			<div 
				v-for="(item, index) in materi" 
				:key="index"
				:class="`${posisi>=index?'bg-gradient-to-br from-purple-500 to-indigo-500':'bg-gray-500'} h-2 w-full rounded-sm m-1`"/>
		</nav>
		<!-- untuk navigasi suaran dan close ikon -->
		<nav class="flex items-center text-sm font-medium justify-between mt-2">
			<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="fill-current text-white feather feather-x"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
			<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-volume-2 text-white"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"></path></svg>
		</nav>
    </div>
		<!-- untuk judul -->
		<h2 v-if="materi[posisi].judul!=''" class="inline-block text-2xl font-extrabold text-gray-100 tracking-tight mb-2">{{materi[posisi].judul}}</h2>
		<!-- untuk gambar / lottie animation -->
		<div class="bg-white" v-if="materi[posisi].lottie!=''||materi[posisi].gambar!=''">
			<lottie-animation 
				v-if="materi[posisi].lottie!=''"
				:path="materi[posisi].lottie"
				:loop="false"
				:height="200"/>
			<img 
				class="max-h-52 mx-auto"
				v-if="materi[posisi].gambar!=''"
				:src="materi[posisi].gambar"/>
		</div>
		<!-- untuk isi konten -->
		<div class="text-gray-300">
			<!-- untuk konten tipe materi -->
			<div v-if="materi[posisi].tipe==='materi'">
				<div class="mt-2"
					v-for="(item, index) in materi[posisi].konten" 
					:key="index" >
					<p v-html="item" v-if="posisiKonten>=index"></p>
				</div>
			</div>
			<!-- untuk konten tipe tes -->
			<div v-else>
				<div class="mt-2"
					v-for="(item, index) in materi[posisi].konten" 
					:key="index" >
					<p v-html="item.soal" v-if="posisiKonten>=index"></p>
					<p class="text-center my-2"><small>Select the correct answer</small></p>
					<div class="flex">
						<div
							v-on:click="handleJawabTes('benar')"
							:class="`${jawabanSiswa==='benar'?'bg-gradient-to-br from-purple-500 to-indigo-500':'bg-gray-600'} w-1/2 text-center rounded mr-2 py-12 font-medium pointer`">
							Benar
						</div>
						<div 
							v-on:click="handleJawabTes('salah')"
							:class="`${jawabanSiswa==='salah'?'bg-gradient-to-br from-purple-500 to-indigo-500':'bg-gray-600'} w-1/2 text-center rounded ml-2 py-12 font-medium pointer`">
							Salah
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
	<!-- aksi untuk tipe konten materi -->
	<div v-if="materi[posisi].tipe==='materi'" style="display:contents">
		<!-- untuk tombol next ke konten berikut nya -->
		<p v-if="isKontenEnd()" class="pointer mt-2 text-purple-500 font-medium px-4 py-4 text-center" v-on:click="handleKontenBerikutnya">
			Tap untuk melanjutkan
		</p>
		<!-- untuk tombol next ke materi berikut nya -->
		<a v-else-if="!isMateriEnd()" href="http://t.me/guru_private_bot" target="_blank" class="bg-gradient-to-br from-purple-500 to-indigo-500 text-white bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 my-2 mx-8 rounded-full text-center">
			Kembali ke chat
		</a>
		<button v-else v-on:click="handleMateriBerikutnya" class="bg-gradient-to-br from-purple-500 to-indigo-500 text-white bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 my-2 mx-8 rounded-full">
			Next
		</button>
	</div>
	<!-- aksi untuk tipe konten kuis -->
	<div v-else style="display:contents">
		<button v-on:click="handleCekJawabanSiswa()" :class="`${jawabanSiswa!=''?'bg-gradient-to-br from-purple-500 to-indigo-500':'bg-gray-600'} text-white text-white font-bold py-2 px-4 my-2 mx-8 rounded-full`">
			Cek Jawaban
		</button>
	</div>

	
</div>
</template>

<script>
import LottieAnimation from "lottie-vuejs/src/LottieAnimation.vue"; // import lottie-vuejs
export default {
	data(){
		return {
			popup:false,
			jawabanSiswa:'',
			posisi:0,
			posisiKonten:0,
			materi:[]
		}
	},
	async asyncData({ $axios, $content, params }) {
		// params.slug
		const materi = await $axios.$get(`/_content/bab/${params.topik}.json`)
		console.log(materi)
		return {
			materi
		}
    },
	components: {
		LottieAnimation
	},
	methods:{
		isKontenEnd(){
			return this.posisiKonten<this.materi[this.posisi].konten.length-1
		},
		isMateriEnd(){
			return this.posisi<this.materi.length-1
		},
		handleKontenBerikutnya(){
			if(this.isKontenEnd()){
				this.posisiKonten	= this.posisiKonten+1
			}	
		},
		handleMateriBerikutnya(){
			if(this.isMateriEnd()){
				this.posisiKonten	= 0
				this.posisi			= this.posisi+1
			}else{
				alert("materi tidak ada")
			}	
		},
		handleJawabTes(jawabanSiswa){
			this.jawabanSiswa=jawabanSiswa
		},
		handleCekJawabanSiswa(){
			if(this.jawabanSiswa!=""){
				if(this.jawabanSiswa===this.materi[this.posisi].konten[0].jawaban){
					// alert("benar")
					this.popupTitle		= "Jawaban Benar"
					this.popupContent	= "Yeay, jawaban anda benar"
					this.popupImage		= ""
				}else{
					// alert("salah")
					this.popupTitle		= "Oops! Jawaban Salah"
					this.popupContent	= "Silahkan Coba Lagi"
					this.popupImage		= ""
					
				}
				this.popup	= true
			}
			
		},
		handleTutupPopup(){
			this.popup	= !this.popup
			// jika jawaban benar, arahkan ke page selanjutnya
			if(this.jawabanSiswa===this.materi[this.posisi].konten[0].jawaban){
				this.jawabanSiswa=''
				this.handleMateriBerikutnya()
			}
		}
	}
}
</script>

<style>

</style>
