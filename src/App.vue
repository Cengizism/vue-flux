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
	import * as components from '@/components';

	export default {
		name: 'App',

		components: {
			...components,
		},

		data: () => ({
			rendered: true,
			transitions: ['fade'],
			selectedTransition: 'fade',
			vfOptions: {
				infinite: true,
				autohideTime: 0,
				autoplay: true,
				enableGestures: true,
				delay: 10000,
				aspectRatio: '4:3',
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
			this.loadImages();
		},

		methods: {
			loadImages() {
				let srcs = [];
				for (let i = 1; i <= 32; i++) {
					srcs.push(
						`slides/${i.toString().padStart(2, '0')}.jpg`
						);
				}

				this.vfImages = [];

				let index, src;

				for (let i = 0; i < 32; i++) {
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
