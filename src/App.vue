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
				<!-- {{ query }} -->
			</div>

			<div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
				<div class="location-info">
					<div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
					<div class="date">
						{{ dateBuilder() }},<br />
						{{ sunRise() }}
					</div>
				</div>

				<div class="weather-info">
					<div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
					<div class="type">{{ weather.weather[0].main }}</div>
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

<style>
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,500,700,900');
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}

body {
	font-family: 'montserrat', sans-serif;
}

#app {
	background-image: url('./assets/cold.png');
	background-position: bottom;
	background-size: cover;
	transition: 0.4;
}

#app.warm {
	background-image: url('./assets/warm.png');
}

#app.fair {
	background-image: url('./assets/fair.png');
}

#app.night {
	background-image: url('./assets/night.png');
}

main {
	background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0.75));
	min-height: 100vh;
	padding: 25px;
}

.search-box .search-bar {
	appearance: none;
	background-color: rgba(255, 255, 255, 0.5);
	border-radius: 0 16px 0px 16px;
	border: none;
	box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
	color: #333;
	display: block;
	font-size: 20px;
	margin-bottom: 30px;
	outline: none;
	padding: 15px;
	transition: all 0.4s;
	width: 100%;
}

.search-box .search-bar:focus {
	background-color: rgba(255, 255, 255, 0.75);
	border-radius: 16px 0px 16px 0px;
	box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
}

.location-info .location {
	color: white;
	font-size: 32px;
	font-weight: 500;
	text-align: center;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-info .date {
	color: white;
	font-size: 20px;
	font-style: italic;
	font-weight: 300;
	text-align: center;
}

.weather-info {
	text-align: center;
}

.weather-info .temp {
	display: inline-block;
	padding: 10px 25px;
	color: white;
	font-size: 100px;
	font-weight: 900;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.25);
	border-radius: 16px;
	margin: 30px 0px;
	box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-info .type {
	color: white;
	font-size: 48px;
	font-size: 700;
	font-style: italic;
	text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
