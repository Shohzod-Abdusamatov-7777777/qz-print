<script setup>
import { ref } from "@vue/reactivity";
import qz from "qz-tray";

const printerList = ref([]);

function print() {
	qz.websocket
		.connect()
		.then(() => {
			return qz.printers.find("XP-58");
		})
		.then((printer) => {
			let config = qz.configs.create(printer);
			return qz.print(config, [
				{
					type: "pixel",
					format: "html",
					flavor: "plain",
					data: "<h1>Hello JavaScript!</h1>",
				},
			]);
		})
		// .then(() => {
		// 	return qz.websocket.disconnect();
		// })
		// .then(() => {
		// 	// process.exit(0);
		// })
		.catch((err) => {
			console.log(err);
			// process.exit(1);
		});
}
</script>

<template>
	<button @click="print()">print</button>
	<h2>All printers</h2>
	<ul>
		<li v-for="(printer, i) in printerList" :key="i">
			{{ printer }}
		</li>
	</ul>
</template>
