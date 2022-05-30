<template>
	<div class="metrics">
		<div id="con" class="container">
			<div class="card">
				<div class="card-header">
					<h3>Игровые показатели</h3>
				</div>
				<div class="card-body">
					<table class="table table-striped">
						<thead>
							<tr>
								<td>
									<b>Название</b>
								</td>
								<td>
									<b>Коэффициент важности</b>
								</td>
							</tr>
						</thead>
						<tbody>
							<tr v-for="m in this.sortable">
								<td>{{ inter[m[0]] }}</td>
								<td>{{ m[1] }}</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import db from '@/firebase.js';

let metricsRef = db.ref('stats');

export default {
	name: 'metrics',
	firebase: {
		metrics: metricsRef,
	},
	data() {
		return {
			metrics: [],
			metr: {},
			sortable: [],
			inter: {
				assists: 'Ассисты',
				conceded: 'Пропущеные голы',
				goals: 'Забитые голы',
				missedPasses: 'Неточные пасы',
				passes: 'Пасы',
				redCard: 'Красные карточки',
				rounds: 'Обводки',
				saves: 'Спасения ворот',
				shots: 'Удары',
				shotsOnTarget: 'Удары в створ',
				tackles: 'Отборы',
				yellowCard: 'Желтые карточки',
			},
		};
	},
	methods: {
		remove: function(training) {
			trainingsRef.child(training['.key']).remove();
		},
	},
	created() {
		db.ref('stats').once('value', (snapshot) => {
			this.metr = snapshot.val();

			for (var m in this.metr) {
				this.sortable.push([m, this.metr[m]]);
			}

			this.sortable.sort(function(a, b) {
				return b[1] - a[1];
			});

            console.log(this.sortable)

			// [["bike", 60], ["motorbike", 200], ["car", 300],
			// ["helicopter", 400], ["airplane", 1000], ["rocket", 28800]]
		});
	},
};
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
