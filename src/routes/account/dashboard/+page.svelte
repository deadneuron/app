<script lang="ts">
	import Highcharts from 'highcharts';
	import { Chart } from '@highcharts/svelte'; // Chart is also exported by default
	import type { PageData } from '../$types';
	import FeedCondensed from '$lib/feedCondensed.svelte';
	export let data: PageData;

	let { articles, profile } = data;

	let embeddings = articles.map((article) => {
		return JSON.parse(article.embedding);
	});

	let meanEmbedding = [];
	for (let i = 0; i < embeddings[0].length; i++) {
		let num = 0;
		for (let j = 0; j < embeddings.length; j++) {
			num += embeddings[j][i];
		}
		meanEmbedding.push(num / embeddings.length);
	}

	let allEmbeddings = embeddings.map((e) => {
		return {
			type: 'scatter',
			color: '#aaa',
			opacity: 0.5,
			marker: {
				symbol: 'diamond',
				radius: 3
			},
			data: e
		};
	});

	allEmbeddings.push({
		type: 'column',
		opacity: 1,
		color: '#000000',
		borderColor: '#000000',
		borderRadius: 0,
		data: meanEmbedding
	});

	let options = {
		title: {
			text: ''
		},
		yAxis: {
			title: {
				enabled: false
			}
		},
		xAxis: {
			visible: false
		},
		plotOptions: {
			series: {
				states: {
					hover: {
						enabled: false
					},
					inactive: {
						enabled: false
					}
				}
			}
		},
		tooltip: { enabled: false },
		legend: { enabled: false },
		credits: { enabled: false },
		series: allEmbeddings
	};
</script>

<div class="wrapper">
	<!-- <div class="menu">
		<nav class="categories">
			<span class="active">@{profile?.username}</span>
			<span>Settings</span>
		</nav>
	</div> -->

	<div class="grid">
		<div class="col">
			<h3 class="minion">Embedding Fingerprint</h3>
			<div class="embeddings">
				<p>
					All research articles on Wits End are represented with a 256 dimensional vector embedding.
					These embeddings are used to measure article similarity and to make recommendations for
					new papers based on the unique collection of research articles you have saved to your
					account.
				</p>
				<div id="chart-container">
					<Chart {options} highcharts={Highcharts} />
				</div>
			</div>

			<h3 class="minion">Personal Info</h3>
			<div class="personal-info">
				<form>
					<div class="input-group">
						<div>
							<label for="first_name">first name</label>
							<input placeholder="first name" name="first_name" value={profile?.first_name} />
						</div>
						<div>
							<label for="last_name">last name</label>
							<input placeholder="last name" name="last_name" value={profile?.last_name} />
						</div>
					</div>
					<div class="input-group">
						<div>
							<label for="username">username</label>
							<input placeholder="username" name="username" value={profile?.username} />
						</div>
						<div>
							<label for="email">email</label>
							<input placeholder="email" name="email" value={profile?.email} />
						</div>
					</div>
					<label for="interests">interests</label>
					<textarea
						placeholder="Research, Summary, Bio, etc..."
						name="interests"
						rows="8"
						value={profile?.bio}
					/>

					<button>Save</button>
				</form>
			</div>
		</div>
		<div class="tree col">
			<h3 class="minion">Saved Articles</h3>
			<FeedCondensed {data} />
		</div>
	</div>
</div>

<style lang="scss">
	.menu {
		border-bottom: 1px solid #ddd;

		.categories {
			padding-bottom: 2rem;

			h2 {
				display: inline-block;
				font-size: 1.2rem;
				font-family: 'Open Sans';
				text-transform: uppercase;
				margin-right: 2rem;
			}
			span {
				font-family: 'Open Sans';
				font-size: 1.2rem;
				font-weight: 500;
				text-transform: uppercase;
				text-decoration: none;
				margin-right: 2rem;
				color: #aaa;

				&.active,
				&:hover {
					color: red;
					cursor: pointer;
				}
			}
			@media screen and (max-width: 600px) {
				display: none;
			}
		}

		@media screen and (max-width: 600px) {
			grid-column: auto;
		}
	}

	.grid {
		display: grid;
		grid-template-rows: auto auto;
		grid-template-columns: 3fr 1fr;
		grid-gap: 2rem;

		.col {
			padding: 1rem;
			padding-left: 0;
			border-right: 1px solid #ddd;

			&:last-child {
				border: none;
			}

			.personal-info {
				margin-bottom: 2rem;

				form {
					.input-group {
						display: grid;
						grid-template-columns: 1fr 1fr;
						grid-gap: 2rem;
						margin-bottom: 2rem;

						input {
							padding: 1rem;
							width: 100%;
							border: 1px solid #ddd;
						}
					}
					textarea {
						width: 100%;
						padding: 1rem;
						border: 1px solid #ddd;
						margin-bottom: 2rem;
					}
					button {
						background: white;
						border: 1px solid #ddd;
						padding: 1rem 2rem;
						transition: all 0.2s ease;

						&:hover {
							border: 1px solid #666;
						}
					}
				}
			}
			.embeddings {
				margin-bottom: 2rem;
				#chart-container {
					margin-left: -1rem;
					margin-bottom: 2rem;

					.chart-title {
						margin-left: 1rem;
						margin-bottom: 1rem;
					}
				}
			}
		}

		.tree {
			.minion {
				font-family: 'Open Sans';
				text-transform: uppercase;
				font-size: 1.4rem;
				line-height: 1.4rem;
				color: #666;
				padding-bottom: 1rem;
				margin-bottom: 1rem;
				border-bottom: 1px solid #ddd;
			}
		}
	}
</style>
