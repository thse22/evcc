<template>
	<div class="group round-box pt-4 px-4 pb-1">
		<dl class="row" data-testid="generalconfig-title">
			<dt class="col-sm-4">Title</dt>
			<dd class="col-sm-8">
				{{ title || "---" }}
				<a
					href="#"
					class="ms-2 d-inline-block text-muted"
					@click.prevent="openModal('titleModal')"
				>
					edit
				</a>
				<TitleModal ref="titleModal" @changed="load" />
			</dd>
		</dl>
		<dl class="row" data-testid="generalconfig-password">
			<dt class="col-sm-4">Password</dt>
			<dd class="col-sm-8">
				*******
				<a
					href="#"
					class="ms-2 d-inline-block text-muted"
					@click.prevent="openModal('passwordModal')"
					>edit</a
				>
			</dd>
		</dl>
		<dl class="row" data-testid="generalconfig-telemetry">
			<dt class="col-sm-4">Telemetry</dt>
			<dd class="col-sm-8">
				{{ telemetryEnabled ? "on" : "off" }}
				<a
					href="#"
					class="ms-2 d-inline-block text-muted"
					@click.prevent="openModal('globalSettingsModal')"
				>
					change
				</a>
			</dd>
		</dl>
		<dl class="row" data-testid="generalconfig-experimental">
			<dt class="col-sm-4">Experimental</dt>
			<dd class="col-sm-8">
				{{ hiddenFeatures ? "on" : "off" }}
				<a
					href="#"
					class="ms-2 d-inline-block text-muted"
					@click.prevent="openModal('globalSettingsModal')"
				>
					change
				</a>
			</dd>
		</dl>
		<dl class="row wip">
			<dt class="col-sm-4">API-Key</dt>
			<dd class="col-sm-8">
				*******
				<a href="#" class="ms-2 d-inline-block text-muted" @click.prevent="todo">show</a>
			</dd>
		</dl>
		<dl class="row wip">
			<dt class="col-sm-4">Sponsoring</dt>
			<dd class="col-sm-8">
				<span class="text-primary"> valid </span>
				<a href="#" class="ms-2 d-inline-block text-muted" @click.prevent="todo">change</a>
			</dd>
		</dl>
		<dl class="row wip">
			<dt class="col-sm-4">Server</dt>
			<dd class="col-sm-8">
				http://evcc.local:7070
				<a href="#" class="ms-2 d-inline-block text-muted" @click.prevent="todo">edit</a>
			</dd>
		</dl>
		<dl class="row wip">
			<dt class="col-sm-4">Update Interval</dt>
			<dd class="col-sm-8">
				30s
				<a href="#" class="ms-2 d-inline-block text-muted" @click.prevent="todo">edit</a>
			</dd>
		</dl>
	</div>
</template>

<script>
import Modal from "bootstrap/js/dist/modal";
import TitleModal from "./TitleModal.vue";
import api from "../../api";
import settings from "../../settings";

export default {
	name: "GeneralConfig",
	data() {
		return {
			title: "",
		};
	},
	components: { TitleModal },
	emits: ["site-changed"],
	async mounted() {
		await this.load();
	},
	computed: {
		telemetryEnabled() {
			return settings.telemetry === true;
		},
		hiddenFeatures() {
			return settings.hiddenFeatures === true;
		},
	},
	methods: {
		async changed() {
			this.$emit("site-changed");
			this.load();
		},
		async load() {
			try {
				let res = await api.get("/config/site");
				this.title = res.data.result.title;
			} catch (e) {
				console.error(e);
			}
		},
		todo() {
			alert("not implemented");
		},
		openModal(id) {
			const $el = document.getElementById(id);
			if ($el) {
				Modal.getOrCreateInstance($el).show();
			} else {
				console.error(`modal ${id} not found`);
			}
		},
	},
};
</script>

<style scoped>
.group {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
	margin-bottom: 5rem;
}
.wip {
	opacity: 0.2;
	display: none !important;
}
dt {
	margin-bottom: 0.5rem;
}
dd {
	margin-bottom: 1rem;
}
</style>
