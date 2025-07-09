<script module lang="ts">
	const T = $state({
		visible: false,
		status: null as string | null,
		message: '',
		timeout: -1,
		duration: 10,
		position: {
			default: 'tr',
			custom: null as string | null | undefined
		}
	});

	export const toast = new Proxy(
		{},
		{
			get(_, status) {
				return (message: string, options?: { duration?: number; position?: string }) => {
					if (T.timeout) clearTimeout(T.timeout);
					T.status = status.toString();
					T.message = typeof message == 'string' ? message : 'Please wait a moment ...';
					// If timeout callback is still scheduled and No new custom position is specified, Preserve the custom position context for better UX
					T.position.custom = options?.position ?? (T.timeout == -1 ? T.position.custom : null);
					T.visible = true;

					if (T.status != 'processing' && (options?.duration ?? T.duration) != Infinity)
						T.timeout = setTimeout(
							() => ((T.visible = false), (T.position.custom = null), (T.timeout = -1)),
							(options?.duration ?? T.duration) * 1000
						);
				};
			}
		}
	) as {
		[K in 'success' | 'error' | 'info' | 'warning' | 'processing']: (
			message: K extends 'processing' ? string | null | undefined : string,
			options?: K extends 'processing'
				? { position?: 'tr' | 'tc' | 'tl' | 'br' | 'bc' | 'bl' }
				: { duration?: number; position?: 'tr' | 'tc' | 'tl' | 'br' | 'bc' | 'bl' }
		) => void;
	};
</script>

<script lang="ts">
	let {
		duration,
		position
	}: {
		/**
		 * Auto-dismiss duration in seconds
		 * @default 10
		 */
		duration?: number;

		/**
		 * Position on screen
		 * @default 'tr' (top-right)
		 */
		position?: 'tr' | 'tc' | 'tl' | 'br' | 'bc' | 'bl';
	} = $props();

	if (duration) T.duration = duration;
	if (position) T.position.default = position;
</script>

{#if T.visible}
	{#if T.status == 'processing'}
		<div
			data-position={T.position.custom ?? T.position.default}
			class="group fixed z-100 flex max-w-max min-w-[92%] overflow-hidden rounded-lg bg-gray-50 shadow-sm ring-3 ring-gray-100 transition-all duration-500 after:absolute after:inset-0 after:-z-10 after:w-full data-[position=bc]:bottom-4 data-[position=bc]:left-1/2
			data-[position=bc]:-translate-x-1/2 data-[position=bl]:bottom-4
			data-[position=bl]:left-4 data-[position=br]:right-4
			data-[position=br]:bottom-4 data-[position=tc]:top-4 data-[position=tc]:left-1/2
			data-[position=tc]:-translate-x-1/2 data-[position=tl]:top-4
			data-[position=tl]:left-4 data-[position=tr]:top-4 data-[position=tr]:right-5
			sm:min-w-lg dark:bg-gray-800
			
			dark:ring-gray-500
			after:dark:bg-gray-500/10
			starting:-translate-y-20
			starting:scale-95 starting:opacity-0 data-[position=bc]:starting:translate-y-20 data-[position=bl]:starting:translate-y-20 data-[position=br]:starting:translate-y-20"
		>
			<div class="grid w-13.5 shrink-0 place-items-center bg-gray-500">
				<svg
					class="size-6 animate-spin rounded-full fill-gray-50 ring-6 ring-white/30"
					viewBox="0 0 24 24"
				>
					<circle class="stroke-gray-500 opacity-50" cx="12" cy="12" r="10" stroke-width="4"
					></circle>
					<path
						class="opacity-75"
						d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
					></path>
				</svg>
			</div>

			<div class="grow px-3.5 py-2">
				<span class="font-bold text-gray-500 uppercase dark:text-gray-400"> Processing </span>
				<p class="animate-pulse text-sm text-gray-600 dark:text-gray-300">
					{T.message}
				</p>
			</div>
			<button
				onclick={() => (T.visible = false)}
				class="my-auto mr-1 ml-auto inline-flex cursor-pointer items-center rounded-lg bg-gray-200/80 p-1 text-gray-400 hover:bg-gray-200 hover:text-gray-900 dark:bg-gray-700 dark:hover:bg-gray-600 dark:hover:text-white"
			>
				<svg class="size-4 fill-current" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
						clip-rule="evenodd"
					></path>
				</svg>
			</button>
		</div>
	{/if}

	{#if T.status == 'success'}
		<div
			data-position={T.position.custom ?? T.position.default}
			class="group fixed z-100 flex max-w-max min-w-[92%] overflow-hidden rounded-lg bg-emerald-50 shadow-sm ring-3 ring-emerald-100 transition-all duration-500 after:absolute after:inset-0 after:-z-10 after:w-full data-[position=bc]:bottom-4 data-[position=bc]:left-1/2
			data-[position=bc]:-translate-x-1/2 data-[position=bl]:bottom-4
			data-[position=bl]:left-4 data-[position=br]:right-4
			data-[position=br]:bottom-4 data-[position=tc]:top-4 data-[position=tc]:left-1/2
			data-[position=tc]:-translate-x-1/2 data-[position=tl]:top-4
			data-[position=tl]:left-4 data-[position=tr]:top-4 data-[position=tr]:right-5
			sm:min-w-lg dark:bg-gray-800
			
			dark:ring-emerald-500
			after:dark:bg-emerald-500/10
			starting:-translate-y-20
			starting:scale-95 starting:opacity-0 data-[position=bc]:starting:translate-y-20 data-[position=bl]:starting:translate-y-20 data-[position=br]:starting:translate-y-20"
		>
			<div class="grid w-13.5 shrink-0 place-items-center bg-emerald-500">
				<svg
					class="size-6 rounded-full fill-current text-white ring-6 ring-white/30"
					viewBox="0 0 512 512"
					><path
						d="M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zM369 209L241 337c-9.4 9.4-24.6 9.4-33.9 0l-64-64c-9.4-9.4-9.4-24.6 0-33.9s24.6-9.4 33.9 0l47 47L335 175c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9z"
					/></svg
				>
			</div>

			<div class="grow px-3.5 py-2">
				<span class="font-bold text-emerald-500 uppercase dark:text-emerald-400">Success</span>
				<p class="text-sm text-emerald-600 dark:text-emerald-300">
					{T.message}
				</p>
			</div>
			<button
				onclick={() => (T.visible = false)}
				class="my-auto mr-1 ml-auto inline-flex cursor-pointer items-center rounded-lg bg-emerald-200/50 p-1 text-emerald-400 hover:bg-emerald-200/60 hover:text-emerald-500 dark:bg-gray-700 dark:hover:bg-emerald-700/40 dark:hover:text-emerald-400"
			>
				<svg class="size-4 fill-current" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
						clip-rule="evenodd"
					></path>
				</svg>
			</button>
		</div>
	{/if}

	{#if T.status == 'error'}
		<div
			data-position={T.position.custom ?? T.position.default}
			class="group fixed z-100 flex max-w-max min-w-[92%] overflow-hidden rounded-lg bg-red-50 shadow-sm ring-3 ring-red-100 transition-all duration-500 after:absolute after:inset-0 after:-z-10 after:w-full data-[position=bc]:bottom-4 data-[position=bc]:left-1/2
			data-[position=bc]:-translate-x-1/2 data-[position=bl]:bottom-4
			data-[position=bl]:left-4 data-[position=br]:right-4
			data-[position=br]:bottom-4 data-[position=tc]:top-4 data-[position=tc]:left-1/2
			data-[position=tc]:-translate-x-1/2 data-[position=tl]:top-4
			data-[position=tl]:left-4 data-[position=tr]:top-4 data-[position=tr]:right-5
			sm:min-w-lg dark:bg-gray-800
			
			dark:ring-red-500
			after:dark:bg-red-500/10
			starting:-translate-y-20
			starting:scale-95 starting:opacity-0 data-[position=bc]:starting:translate-y-20 data-[position=bl]:starting:translate-y-20 data-[position=br]:starting:translate-y-20"
		>
			<div class="grid w-13.5 shrink-0 place-items-center bg-red-500">
				<svg
					class="size-6 rounded-full fill-current text-white ring-6 ring-white/30"
					viewBox="2 2 20 20"
				>
					<path
						d="m11.5 20l4.86-9.73H13V4l-5 9.73h3.5V20M12 2c2.75 0 5.1 1 7.05 2.95C21 6.9 22 9.25 22 12s-1 5.1-2.95 7.05C17.1 21 14.75 22 12 22s-5.1-1-7.05-2.95C3 17.1 2 14.75 2 12s1-5.1 2.95-7.05C6.9 3 9.25 2 12 2Z"
					></path>
				</svg>
			</div>

			<div class="grow px-3.5 py-2">
				<span class="font-bold text-red-500 uppercase dark:text-red-400"> Error </span>
				<p class="text-sm text-red-600 dark:text-red-300">
					{T.message}
				</p>
			</div>
			<button
				onclick={() => (T.visible = false)}
				class="my-auto mr-1 ml-auto inline-flex cursor-pointer items-center rounded-lg bg-red-200/50 p-1 text-red-400 hover:bg-red-200/60 hover:text-red-500 dark:bg-gray-700 dark:hover:bg-red-700/40 dark:hover:text-red-400"
			>
				<svg class="size-4 fill-current" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
						clip-rule="evenodd"
					></path>
				</svg>
			</button>
		</div>
	{/if}

	{#if T.status == 'info'}
		<div
			data-position={T.position.custom ?? T.position.default}
			class="group fixed z-100 flex max-w-max min-w-[92%] overflow-hidden rounded-lg bg-blue-50 shadow-sm ring-3 ring-blue-100 transition-all duration-500 after:absolute after:inset-0 after:-z-10 after:w-full data-[position=bc]:bottom-4 data-[position=bc]:left-1/2
			data-[position=bc]:-translate-x-1/2 data-[position=bl]:bottom-4
			data-[position=bl]:left-4 data-[position=br]:right-4
			data-[position=br]:bottom-4 data-[position=tc]:top-4 data-[position=tc]:left-1/2
			data-[position=tc]:-translate-x-1/2 data-[position=tl]:top-4
			data-[position=tl]:left-4 data-[position=tr]:top-4 data-[position=tr]:right-5
			sm:min-w-lg dark:bg-gray-800
			
			dark:ring-blue-500
			after:dark:bg-blue-500/10
			starting:-translate-y-20
			starting:scale-95 starting:opacity-0 data-[position=bc]:starting:translate-y-20 data-[position=bl]:starting:translate-y-20 data-[position=br]:starting:translate-y-20"
		>
			<div class="grid w-13.5 shrink-0 place-items-center bg-blue-500">
				<svg
					class="size-6 rounded-full fill-current text-white ring-6 ring-white/30"
					viewBox="0 0 717 717"
					><path
						d="M358 0c198 0 359 161 359 359S556 717 358 717S0 557 0 359S160 0 358 0zm-4 132c-10 12-15 26-15 41c0 12 4 22 10 29c7 7 17 11 27 11c14 0 27-6 37-18c9-11 14-27 14-44c0-10-3-19-10-26s-17-11-27-11c-13 0-26 6-36 18zm-91 207l12 13c9-10 18-18 24-23c6-4 11-6 15-6c3 0 6 1 8 3c1 3 2 6 2 11c0 25-3 41-16 101s-20 104-20 133c0 11 2 19 6 24c3 5 9 9 17 9c13 0 32-10 54-27c22-18 44-43 68-75l-13-11c-8 10-16 17-22 22c-6 4-11 8-15 8c-3 0-6-3-8-5c-1-3-2-7-2-13c0-4 1-17 5-40c3-23 2-23 8-57c1-10 4-24 7-41c8-51 13-82 13-92c0-9-3-17-6-22c-4-5-10-7-17-7c-12 0-28 9-50 25c-22 17-44 40-70 70z"
					></path></svg
				>
			</div>

			<div class="grow px-3.5 py-2">
				<span class="font-bold text-blue-500 uppercase dark:text-blue-400"> Info </span>
				<p class="text-sm text-blue-600 dark:text-blue-300">
					{T.message}
				</p>
			</div>
			<button
				onclick={() => (T.visible = false)}
				class="my-auto mr-1 ml-auto inline-flex cursor-pointer items-center rounded-lg bg-blue-200/50 p-1 text-blue-400 hover:bg-blue-200/60 hover:text-blue-500 dark:bg-gray-700 dark:hover:bg-blue-700/40 dark:hover:text-blue-400"
			>
				<svg class="size-4 fill-current" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
						clip-rule="evenodd"
					></path>
				</svg>
			</button>
		</div>
	{/if}

	{#if T.status == 'warning'}
		<div
			data-position={T.position.custom ?? T.position.default}
			class="group fixed z-100 flex max-w-max min-w-[92%] overflow-hidden rounded-lg bg-yellow-50 shadow-sm ring-3 ring-yellow-100 transition-all duration-500 after:absolute after:inset-0 after:-z-10 after:w-full data-[position=bc]:bottom-4 data-[position=bc]:left-1/2
			data-[position=bc]:-translate-x-1/2 data-[position=bl]:bottom-4
			data-[position=bl]:left-4 data-[position=br]:right-4
			data-[position=br]:bottom-4 data-[position=tc]:top-4 data-[position=tc]:left-1/2
			data-[position=tc]:-translate-x-1/2 data-[position=tl]:top-4
			data-[position=tl]:left-4 data-[position=tr]:top-4 data-[position=tr]:right-5
			sm:min-w-lg dark:bg-gray-800
			
			dark:ring-yellow-500
			after:dark:bg-yellow-500/10
			starting:-translate-y-20
			starting:scale-95 starting:opacity-0 data-[position=bc]:starting:translate-y-20 data-[position=bl]:starting:translate-y-20 data-[position=br]:starting:translate-y-20"
		>
			<div class="grid w-13.5 shrink-0 place-items-center bg-yellow-500">
				<svg
					class="size-6 rounded-full fill-current text-white ring-6 ring-white/30"
					viewBox="0 0 32 32"
					><path
						d="M16 2C8.3 2 2 8.3 2 16s6.3 14 14 14s14-6.3 14-14S23.7 2 16 2zm-1.1 6h2.2v11h-2.2V8zM16 25c-.8 0-1.5-.7-1.5-1.5S15.2 22 16 22s1.5.7 1.5 1.5S16.8 25 16 25z"
					/></svg
				>
			</div>

			<div class="grow px-3.5 py-2">
				<span class="font-bold text-yellow-500 uppercase dark:text-yellow-400"> Warning </span>
				<p class="text-sm text-yellow-600 dark:text-yellow-300">
					{T.message}
				</p>
			</div>
			<button
				onclick={() => (T.visible = false)}
				class="my-auto mr-1 ml-auto inline-flex cursor-pointer items-center rounded-lg bg-yellow-200/50 p-1 text-yellow-400 hover:bg-yellow-200/60 hover:text-yellow-500 dark:bg-gray-700 dark:hover:bg-yellow-700/40 dark:hover:text-yellow-400"
			>
				<svg class="size-4 fill-current" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
						clip-rule="evenodd"
					></path>
				</svg>
			</button>
		</div>
	{/if}
{/if}
