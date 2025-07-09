# Sveltoast: Elegant & Modern Toasts for Svelte 5 & SvelteKit ✨

**Sveltoast** is a super lightweight, highly optimized and customizable toast notification library built specifically for Svelte 5 and SvelteKit applications.

## 🎨 Styling

Styled with Tailwind CSS v4 for easy customization.

## 🚀 Quick Start

### Installation

```bash
npm install sveltoast
```

### Usage

1. **Import and use in your Svelte component:**

```ts
<script>
	import Sveltoast, { toast } from 'sveltoast';
</script>

<Sveltoast />

<button onclick={() => toast.success('Your changes have been saved!')}> Show Success </button>
<button onclick={() => toast.error('Failed to submit form. Please try again.')}>
	Show Error
</button>
```

2. **Place `<Sveltoast />` once at the top level** (e.g., in `src/routes/+layout.svelte`):

```svelte
<script>
	import Sveltoast from 'sveltoast';
</script>

<Sveltoast />
```

## 📚 API Reference

### `toast` Object

The `toast` object provides five methods for different notification types:

- `toast.success(message, options?)`
- `toast.error(message, options?)`
- `toast.info(message, options?)`
- `toast.warning(message, options?)`
- `toast.processing(message?, options?)`

#### Parameters

- **`message` (string)**: The content to display. For `toast.processing`, you can pass `null` or `undefined` or nothing at all to show a default "Please wait a moment..." message.
- **`options` (object, optional)**: Customize the toast behavior:
  - `duration` (number): Auto-dismiss time in seconds. Default: `10`. Set to `Infinity` for persistent toasts to let the user dismiss it manually.
  - `position` (string): Override default position for this toast.

#### Position Options

- `'tr'` (top-right, **default**)
- `'tc'` (top-center)
- `'tl'` (top-left)
- `'br'` (bottom-right)
- `'bc'` (bottom-center)
- `'bl'` (bottom-left)

### `<Sveltoast />` Component Props

- **`duration` (number, optional)**: Default auto-dismiss duration in seconds. Default: `10`
- **`position` (string, optional)**: Default screen position. Default: `'tr'`

#### Example: Setting Global Defaults

```svelte
<script>
	import Sveltoast from 'sveltoast';
</script>

<Sveltoast duration={5} position="tc" />
```

## 💡 Advanced Usage

### Handling Long-Running Operations

`toast.processing` is ideal for background tasks. By default, processing toasts don't auto-dismiss:

```ts
<script>
	import { toast } from 'sveltoast';

	async function submitForm() {
		toast.processing('Sending your data...');

		try {
			await new Promise((resolve) => setTimeout(resolve, 3000));
			toast.success('Form submitted successfully!');
		} catch (error) {
			toast.error('An error occurred during submission.');
		}
	}
</script>

<button onclick={submitForm}>Submit</button>
```

**Key behavior for `processing` toasts:**

- Remain visible until another toast is triggered or closed by the user
- New processing toasts replace existing ones
- Use `null`/`undefined` message for default "Please wait..." text

### Overriding Defaults Per-Toast

```ts
<script>
	import { toast } from 'sveltoast';
</script>

<button onclick={() => toast.success('2 second toast', { duration: 2 })}> Short Success </button>

<button onclick={() => toast.error('Top-left error', { position: 'tl' })}>
	Positioned Error
</button>

<button onclick={() => toast.info('Persistent info', { duration: Infinity })}>
	Persistent Info
</button>
```

### Automatic Position Inheritance

Follow-up toasts after `processing` toasts inherit their position for smooth visual flow:

```ts
<script>
	import { toast } from 'sveltoast';

	async function processAndNotify() {
		toast.processing(null, { position: 'bc' });

		try {
			await new Promise((resolve) => setTimeout(resolve, 2500));
			toast.success('Completed successfully!'); // Inherits bottom-center position
		} catch (e) {
			toast.error('Operation failed!'); // Also inherits bottom-center position
		}
	}
</script>

<button onclick={processAndNotify}>Start Processing</button>
```

## 🤝 Contributing

Contributions welcome! Report bugs, suggest features to help improve Sveltoast.
