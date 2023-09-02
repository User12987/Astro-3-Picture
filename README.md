# Astro - 3 - Picture
Astro 3 Picture Component

## Usege:

1. Copy component to your Astro project
2. Create const with image

```astro
import img from 'way-to-image'
```

3. Use Picture component like this:
```astro
 <Picture
	src={img}
	alt={'image description'}
	sizes={[250, 500, 750, 1024]}
	media={'(max-width: 750px) 100vw, 250px'}
	quality={'height'}
	loading={'eager'}
	class:list={'some-class'}
/>
```
Done.

Thats will create a `<picture>...</picture>` tag with all needle properties.
	So, final result will be like this:
```html
<picture>
	<source
		type="image/avif"
		srcset="
			/_way_to_img w=250 h=250 f=avif 250w,
			/_way_to_img w=500 h=500 f=avif 500w,
			/_way_to_img w=750 h=750 f=avif 750w,
			/_way_to_img w=1024 h=1024 f=avif 1024w
		"
		sizes="(max-width: 767px) 100vw, 512px" />
	<source
		type="image/webp"
		srcset="
			/_way_to_img w=250 h=250 f=webp 250w,
			/_way_to_img w=500 h=500 f=webp 500w,
			/_way_to_img w=750 h=750 f=webp 750w,
			/_way_to_img w=1024 h=1024 f=webp 1024w
		"
		sizes="(max-width: 767px) 100vw, 512px" />
	<img
		src="/_way_to_img w=250 h=250 f=jpeg"
		decoding="async"
		loading="eager"
		alt="image description"
		width="512"
		class="some-class"
	/>
</picture>
```

Best results I have with next`astro.config.mjs` setup:
```
export default defineConfig( {
	image: {
		service: squooshImageService()
	},
} )
```
