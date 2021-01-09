<template>
	<div class="container">
		<div class="flex">
			<h2 class="name">{{ name }}</h2>
			<div class="card">
				<transition name="slide-fade">
					<img
						v-show="activeItem === 'home'"
						src="./assets/aquila.png"
						alt=""
					/>
				</transition>
				<transition name="slide-fade">
					<div v-show="activeItem === 'about'" class="about">
						<div class="about-content">
							<h2>About</h2>
							<br />
							<p>
								A Fashion Model with 5 years’ experience in industrial projects,
								successfully modeled for top brands and possesses the specific
								attributes that many designers and fashion directors look for.
							</p>
						</div>
					</div>
				</transition>
				<transition name="slide-fade">
					<div v-show="activeItem === 'gallery'" class="gallery">
						<transition-group name="fade" tag="div" class="slider">
							<div v-for="i in [currentIndex]" :key="i">
								<img :src="currentImg" />
							</div>
						</transition-group>
						<a class="prev" @click="prev" href="#">&#10094; Previous</a>
						<a class="next" @click="next" href="#">Next &#10095; </a>
					</div>
				</transition>
				<transition name="slide-fade">
					<div v-show="activeItem === 'contact'" class="contact">
						<img src="./assets/contact.jpg" alt="" />
						<a :href="'mailto:' + email" class="mail">
							Shoot a Mail
						</a>
					</div>
				</transition>
			</div>
			<div class="navbar">
				<ul>
					<li class="navlink">
						<a href="" @click.prevent="change('home')">HOME</a>
					</li>
					<li class="navlink">
						<a href="" @click.prevent="change('about')">ABOUT</a>
					</li>
					<div class="avi">
						<img src="./assets/avi.jpg" alt="" />
					</div>
					<li class="navlink">
						<a href="" @click.prevent="change('gallery')">GALLERY</a>
					</li>
					<li class="navlink">
						<a href="" @click.prevent="change('contact')">CONTACT</a>
					</li>
				</ul>
			</div>
		</div>
		<div class="social">
			<ul>
				<li>
					<a :href="instagram" class="social__link">
						<img src="./assets/instagram.svg" alt="" />
					</a>
				</li>
				<li>
					<a :href="twitter" class="social__link">
						<img src="./assets/twitter-sign.svg" alt="" />
					</a>
				</li>
				<li>
					<a :href="snapchat" class="social__link">
						<img src="./assets/snapchat.svg" alt="" />
					</a>
				</li>
				<li>
					<a :href="'mailto:' + email" class="social__link">
						<img src="./assets/email.svg" alt="" />
					</a>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
	export default {
		name: "App",
		components: {},
		data() {
			return {
				api: "https://hirng-x2021.glitch.me/api",
				name: "",
				firstName: "",
				lastName: "",
				twitter: "",
				instagram: "",
				snapchat: "",
				email: "",

				activeItem: "home",

				images: [
					"https://res.cloudinary.com/jphil/image/upload/q_34/v1610112813/aquila/gall3_kbtxg9.jpg",
					"https://res.cloudinary.com/jphil/image/upload/q_30/v1610112795/aquila/gall4_lkmxc5.jpg",
					"https://res.cloudinary.com/jphil/image/upload/q_30/v1610112764/aquila/gall2_ytspq0.jpg",
					"https://res.cloudinary.com/jphil/image/upload/q_30/v1610112753/aquila/gall1_ozsgjl.jpg",
				],
				timer: null,
				currentIndex: 0,
			};
		},
		mounted() {
			fetch(this.api)
				.then((response) => response.json())
				.then((data) => {
					this.name = data.name;
					this.firstName = this.name.split(" ")[0];
					this.lastName = this.name.split(" ")[1];
					this.twitter = "https://www.twitter.com/" + data.social_media.twitter;
					this.instagram =
						"https://www.instagram.com/" + data.social_media.instagram;
					this.snapchat =
						"https://www.snapchat.com/" + data.social_media.snapchat;
					this.email = data.social_media.email;
				});

			this.startSlide();
		},
		methods: {
			change(menu) {
				this.activeItem = menu;
			},
			startSlide() {
				this.timer = setInterval(this.next, 4000);
			},

			next() {
				this.currentIndex += 1;
			},
			prev() {
				this.currentIndex -= 1;
			},
		},
		computed: {
			currentImg: function() {
				return this.images[Math.abs(this.currentIndex) % this.images.length];
			},
		},
	};
</script>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap");

	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		font-family: sans-serif;
		font-family: "Roboto", sans-serif;
	}

	img {
		width: 100%;
		height: 100%;
	}

	a {
		text-decoration: none;
		color: inherit;
	}

	ul,
	li {
		padding-inline-start: 0;
		margin-block-start: 0;
		margin-block-end: 0;
		list-style: none;
	}

	.container {
		position: relative;
		width: 100%;
		min-height: 100vh;
		display: flex;
		flex-flow: row;
		justify-content: center;
		align-items: center;
		padding: 2rem;
		font-size: 20px;
		background: #cb2964;
		overflow: hidden;
		/* background: linear-gradient(to right, #bd0f4d, #cb2964); */
	}

	.container:before {
		content: "";
		position: absolute;
		width: 50%;
		height: 100%;
		top: 0;
		left: 0;
		background: #bd0f4d;
		z-index: 1;
		text-align: center;
	}

	.name {
		position: relative;
		width: 100%;
		margin-left: 40px;
		z-index: 2;
		text-align: center;
		color: #fff;
		letter-spacing: 15px;
	}

	.flex {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.card {
		position: relative;
		width: 500px;
		height: 70vh;
		margin: 1rem auto;
		background: #cc2a66;
		overflow: hidden;
		z-index: 2;
		-webkit-box-shadow: -4px 3px 17px 0px rgba(32, 50, 50, 0.66);
		-moz-box-shadow: -4px 3px 17px 0px rgba(32, 50, 50, 0.66);
		box-shadow: -4px 3px 17px 0px rgba(32, 50, 50, 0.66);
	}

	.about {
		width: 100%;
		height: 100%;
		padding: 2rem;
		background: url("./assets/avi.jpg") no-repeat;
		background-size: cover;
		background-position: top center;
		color: #fff;
		display: flex;
		flex-flow: column wrap;
		justify-content: center;
		align-items: center;
	}

	.about-content {
		position: relative;
		z-index: 2;
		line-height: 2rem;
		font-weight: 600;
	}

	.about:after {
		position: absolute;
		display: block;
		content: "";
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: radial-gradient(#0c0b0c36, #0c0809);
	}

	.gallery {
		width: 100%;
		height: 100%;
		overflow: hidden;
	}

	.contact {
		width: 100%;
		height: 100%;
	}

	.contact:after {
		position: absolute;
		display: block;
		content: "";
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: radial-gradient(#0c0b0c36, #0c0809);
	}

	.mail {
		position: absolute;
		display: block;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		color: #fff;
		border: solid 2px #fff;
		padding: 1rem 2rem;
		border-radius: 5px;
		z-index: 3;
		transition: all 0.3s ease;
	}

	.mail:hover {
		background: #cb2964d0;
	}

	/* Transition*/

	.fade-enter-active,
	.fade-leave-active {
		transition: all 0.9s ease;
		overflow: hidden;
		visibility: visible;
		position: absolute;
		width: 100%;
		opacity: 1;
	}

	.fade-enter,
	.fade-leave-to {
		visibility: hidden;
		width: 100%;
		opacity: 0;
	}

	.slider img {
		height: 100%;
		width: 100%;
	}

	.prev,
	.next {
		cursor: pointer;
		position: absolute;
		top: 40%;
		width: auto;
		padding: 16px;
		color: white;
		font-weight: bold;
		font-size: 18px;
		transition: 0.7s ease;
		text-decoration: none;
		user-select: none;
	}

	.next {
		right: 0;
	}

	.prev {
		left: 0;
	}

	.prev:hover,
	.next:hover {
		background-color: #bd0f4d;
	}

	.slide-fade-enter-active {
		transition: all 0.8s ease;
	}
	.slide-fade-leave-active {
		transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
	}
	.slide-fade-enter, .slide-fade-leave-to
	/* .slide-fade-leave-active below version 2.1.8 */ {
		transform: translateX(10px);
		opacity: 0;
	}

	.bounce-enter-active {
		animation: bounce-in 0.5s;
	}
	.bounce-leave-active {
		animation: bounce-in 0.5s reverse;
	}
	@keyframes bounce-in {
		0% {
			transform: scale(0);
		}
		50% {
			transform: scale(1.5);
		}
		100% {
			transform: scale(1);
		}
	}

	.navbar {
		position: relative;
		z-index: 2;
		width: auto;
		margin: auto;
	}

	.navbar ul {
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: justify;
		margin-left: 60px;
		color: #fff;
		font-weight: 600;
	}

	.navlink {
		margin: 20px;
	}

	.avi {
		width: 90px;
		height: 90px;
		/* max-width: 120px;
		max-height: 120px; */
		border-radius: 50%;
		overflow: hidden;
	}

	.social {
		position: relative;
		justify-self: flex-end;
		right: -25%;
		z-index: 3;
	}

	.social__link {
		display: block;
		height: 40px;
		width: 30px;
		margin: 1rem 0;
		transition: all 0.3s ease;
	}

	.social__link:hover {
		transform: scale(1.5);
	}

	@media (max-width: 500px) {
		.card {
			width: 250px !important;
			height: 50vh !important;
		}

		.navlink {
			font-size: 14px !important;
			margin: 10px 5px !important;
		}
	}

	@media (max-width: 700px) {
		.card {
			width: 350px;
			height: 60vh;
		}

		.mail {
			width: max-content;
		}

		.navbar ul {
			margin-left: 0;
		}

		.navlink {
			font-size: 16px;
			margin: 10px;
		}

		.avi {
			display: none;
		}
	}

	@media (max-width: 900px) {
		.container {
			flex-direction: column;
			align-items: center;
		}

		.social {
			position: static;
		}
		.social ul {
			display: flex;
			flex-flow: row wrap;
		}

		.social__link {
			margin: 10px;
		}
	}

	@media (max-width: 1200px) {
		.social {
			right: -15%;
		}
	}
</style>
