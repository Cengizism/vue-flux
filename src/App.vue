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
						:transitions="vfTransitions"
						:captions="vfCaptions"
					>

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
	import { VcParagraph } from 'vue-cosk';

	export default {
		name: 'App',

		components: {
			...components,
			VcParagraph,
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
				delay: 4000,
				aspectRatio: '4:3',
				bindKeys: true,
				allowFullscreen: true,
				lazyLoadAfter: 5,
			},
			vfImages: [],
			vfCaptions: [],
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
					srcs.push(`slides/${i.toString().padStart(2, '0')}.jpg`);
				}

				this.vfImages = [];
				this.vfCaptions = [];

				let index, src;

				for (let i = 0; i < 32; i++) {
					index = Math.floor(Math.random() * srcs.length);

					src = srcs.splice(index, 1)[0];

					this.vfImages.push(src);
					this.vfCaptions.push(src);
				}
			},

			addImage(url, author, location) {
				this.vfImages.push(url);
				this.vfCaptions.push(location +' - '+ author);
			},

			showNext(transition) {
				this.$refs.slider.show('next', transition);
			},
		}
	};
</script>

<style lang="scss">
	.test {
		.flux-image {
			border: 1px solid white;
			border-collapse: collapse;
		}
	}

	.vue-flux {
		box-shadow: 0 0 12px 2px rgba(34,36,38,.85);
	}

	.flux-parallax {
		display: flex;
		position: relative;
		font-size: 3rem;
		color: white;
		font-weight: bold;
		justify-content: center;
		align-items: center;
		text-shadow:
			-2px -2px 0 black,
			2px -2px 0 black,
			-2px  2px 0 black,
			2px  2px 0 black;
	}

	p {
		margin: 24px 0;
	}
</style>
