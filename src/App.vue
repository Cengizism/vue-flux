<template>
	<!-- eslint-disable -->
	<div id="app">
		<div class="container mx-auto mb-4">

			<div class="block sm:block md:block lg:flex">
				<div class="lg:w-3/4 px-2 mb-4">
					<vue-flux
						v-if="rendered"
						ref="slider"
						:options="vfOptions"
						:images="vfImages"
						:transitions="vfTransitions"					>

						<template v-slot:preloader>
							<flux-preloader />
						</template>

					</vue-flux>
				</div>

			</div>

		</div>
	</div>
</template>

<script>
	/* eslint-disable */
	import {
		VueFlux,
		FluxPreloader,
	} from '@/components';

	export default {
		name: 'App',

		components: {
			VueFlux,
			FluxPreloader,
		},

		data: () => ({
			source: 'http://localhost:3000/images',
			rendered: true,
			transitions: ['fade'],
			selectedTransition: 'fade',
			vfOptions: {
				infinite: true,
				autohideTime: 0,
				autoplay: true,
				enableGestures: true,
				delay: 20000,
				aspectRatio: '3:4',
				bindKeys: true,
				allowFullscreen: true,
				lazyLoadAfter: 5,
			},
			vfImages: [],
		}),

		computed: {
			vfTransitions() {
				return [this.selectedTransition];
			},
		},

		created() {
			fetch(this.source)
				.then(async response => {
					const data = await response.json();

					if (!response.ok) {
						const error = 
							(data && data.message) || response.statusText;
						return Promise.reject(error);
					}

					this.loadImages(data);
				})
				.catch(error => {
					this.errorMessage = error;
					console.error("There was an error fetching images!", error);
				});
		},

		methods: {
			loadImages(list) {
				let srcs = [];

				for (let i = 0; i < list.length; i++) {
					srcs.push(`slides/${list[i]}`);
				}

				this.vfImages = [];

				let index, src;

				for (let i = 0; i < list.length; i++) {
					index = Math.floor(Math.random() * srcs.length);

					src = srcs.splice(index, 1)[0];

					this.vfImages.push(src);
				}
			},

			addImage(url, author, location) {
				this.vfImages.push(url);
			},

			showNext(transition) {
				this.$refs.slider.show('next', transition);
			},
		}
	};
</script>

<style lang="scss">
	html {
		height: 100%;
		width: 100%;
	}

	.vue-flux {
		min-height: 100%;
		min-width: 100%;
		width: auto;
		height: 100%;
		position: fixed;
		top: 0;
		left: 0;
	}
</style>
