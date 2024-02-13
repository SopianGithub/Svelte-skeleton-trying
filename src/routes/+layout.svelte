<script lang="ts">
	import '../app.postcss';
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
	import Sidebar from '$lib/Navigation/Sidebar.svelte';
	import { initializeStores, Drawer, getDrawerStore } from '@skeletonlabs/skeleton';
	import Themes from '$lib/utils/themes.svelte';
	import Profile from '$lib/utils/profile.svelte';
	// import { page } from '$app/stores';

	// Highlight JS
	import hljs from 'highlight.js/lib/core';
	import 'highlight.js/styles/github-dark.css';
	import { storeHighlightJs } from '@skeletonlabs/skeleton';
	import xml from 'highlight.js/lib/languages/xml'; // for HTML
	import css from 'highlight.js/lib/languages/css';
	import javascript from 'highlight.js/lib/languages/javascript';
	import typescript from 'highlight.js/lib/languages/typescript';

	hljs.registerLanguage('xml', xml); // for HTML
	hljs.registerLanguage('css', css);
	hljs.registerLanguage('javascript', javascript);
	hljs.registerLanguage('typescript', typescript);
	storeHighlightJs.set(hljs);

	// Floating UI for Popups
	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';
	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });

	initializeStores();

	const drawerStore = getDrawerStore();

	function drawerOpen(): void {
		drawerStore.open({});
	}

	// $: classesSidebar = $page.url.pathname === '/' ? 'w-0 lg:w-64' : 'w-0 lg:w-64';
	$: classesSidebar = 'w-0 lg:w-64';

	let isDropdownOpen: boolean = false;
	const handleDropdownClick = () => {
		isDropdownOpen = !isDropdownOpen // togle state on click
	}
</script>

<!-- App Shell -->
<AppShell slotSidebarLeft="bg-surface-500/5 {classesSidebar}">
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar gridColumns="grid-cols-3" slotDefault="place-self-center" slotTrail="place-content-end">
			<svelte:fragment slot="lead">
				<button on:click={drawerOpen} class="lg:hidden btn btn-sm mr-4">
					<span>
						<svg viewBox="0 0 100 80" class="fill-token w-4 h-4">
							<rect width="100" height="20" />
							<rect y="30" width="100" height="20" />
							<rect y="60" width="100" height="20" />
						</svg>
					</span>
				</button>
				<strong class="text-xl uppercase">Skeleton</strong>
			</svelte:fragment>
			<svelte:fragment slot="trail">
				<div class="max-w-screen-xl flex flex-wrap gap-3 items-center justify-between p-4">
					<div>
						<Themes />
					</div>
					<Profile />
				</div>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>

	<svelte:fragment slot="sidebarLeft">
		<Drawer>
			<h2 class="p-4">Navigation</h2>
			<hr />
			<Sidebar />
		</Drawer>
		<div id="sidebar" class="hidden lg:block">
			<Sidebar />
		</div>
	</svelte:fragment>
	

	<!-- Page Route Content -->
	<slot />

	<svelte:fragment slot="footer">Footer</svelte:fragment>
</AppShell>
