<template>
	<div
		id="app"
		:class="
			typeof weather.main != 'undefined' && weather.main.temp > 18
				? 'warm'
				: '' || (typeof weather.main != 'undefined' && weather.main.temp > 10)
				? 'fair'
				: ''
		"
	>
		<main>
			<div class="search-box">
				<input
					type="text"
					class="search-bar"
					placeholder="Search..."
					v-model="query"
					@keypress="fetchWeather"
				/>
			</div>

			<div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
				<div class="location-info">
					<div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
					<div class="date">
						{{ dateBuilder() }}
					</div>
				</div>

				<div class="weather-info">
					<div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
					<div class="type">{{ weather.weather.description }}</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
export default {
	name: 'app',
	data() {
		return {
			api_key: '47401237b8b1fcc1802bc0660be93625',
			url_base: 'https://api.openweathermap.org/data/2.5/',
			query: '',
			weather: {},
		};
	},
	methods: {
		async fetchWeather(event) {
			if (event.key == 'Enter') {
				try {
					let response = await fetch(
						`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
					);
					let json = await response.json();
					this.weather = await json;
				} catch (error) {
					console.log(`Error ${error}`);
				}
			}
		},
		setResults(results) {
			this.weather = results;
		},
		dateBuilder() {
			let d = new Date();
			let months = [
				'Janury',
				'February',
				'March',
				'April',
				'May',
				'June',
				'July',
				'August',
				'September',
				'October',
				'November',
				'December',
			];
			let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

			let day = days[d.getDay()];
			let date = d.getDate();
			let month = months[d.getMonth()];
			let year = d.getFullYear();

			return `${day}, ${date} ${month} ${year}`;
		},
		sunRise() {
			let unix_timestamp = this.weather.sys.sunrise;
			// Create a new JavaScript Date object based on the timestamp
			// multiplied by 1000 so that the argument is in milliseconds, not seconds.
			var date = new Date(unix_timestamp * 1000);
			// Hours part from the timestamp
			var hours = date.getHours();
			// Minutes part from the timestamp
			var minutes = '0' + date.getMinutes();
			// Seconds part from the timestamp
			var seconds = '0' + date.getSeconds();

			// Will display time in hour:minute:second format
			var formattedTime = hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);

			console.log(formattedTime);
		},
		sunSet() {
			let unix_timestamp = this.weather.sys.sunset;
			// Create a new JavaScript Date object based on the timestamp
			// multiplied by 1000 so that the argument is in milliseconds, not seconds.
			var date = new Date(unix_timestamp * 1000);
			// Hours part from the timestamp
			var hours = date.getHours();
			// Minutes part from the timestamp
			var minutes = '0' + date.getMinutes();
			// Seconds part from the timestamp
			var seconds = '0' + date.getSeconds();

			// Will display time in hour:minute:second format
			var formattedTime = hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);

			console.log(formattedTime);
		},
	},
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,500,700,900');
@import './assets/scss/app.scss';
</style>
