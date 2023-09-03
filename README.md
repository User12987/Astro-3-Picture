# Astro - 3 - Picture component

Astro v. 3 has no more Picture component. So, I did it and share with you. As they sas in documentation,

> Currently, the built-in assets feature does not include a <Picture /> component. Instead, you can generate images or custom components using getImage() that use the HTML image attributes srcset and sizes or the <picture> tag for art direction or to create responsive images.

## Usage:

1. Copy Picture component to your Astro project.
2. Create const with image like this in your page:

```astro
---
import img from 'way-to-image'
---
```

Attention! In img const we have img itself (not just path to img).

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
	anyOtherProps={'any other props'}
/>
```

Done.

Thats will create a `<picture>...</picture>` tag with all needle properties to get [responsive images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images#art_direction).
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
		sizes="(max-width: 767px) 100vw, 512px"
	/>
	<source
		type="image/webp"
		srcset="
			/_way_to_img w=250 h=250 f=webp 250w,
			/_way_to_img w=500 h=500 f=webp 500w,
			/_way_to_img w=750 h=750 f=webp 750w,
			/_way_to_img w=1024 h=1024 f=webp 1024w
		"
		sizes="(max-width: 767px) 100vw, 512px"
	/>
	<img
		src="/_way_to_img w=250 h=250 f=jpeg"
		decoding="async"
		loading="eager"
		alt="image description"
		width="512"
		class="some-class"
		anyOtherProps="any other props"
	/>
</picture>
```

I had check it with LightHouse and it works fine.

## Params

### src

Type: `object` (img) - imported image itself. Required. See usage for details.

Use can get it like this:

```astro
import img from 'way-to-image'
```

### sizes

Type: `string` - sizes of images which will be generated.
For example, if min possible result img size is 250px, and max possible result img size is 1024px, you can set sizes like this:

```astro
sizes={[250, 500, 750, 1024]}
```

It will generate 4 images with sizes 250, 500, 750, 1024px for different screens, DPI, etc.

### media

Type: `string` - media query for sizes. For example, if default picture size should be 250px for big screens, but 100% width for small screens (with width less than 750px), you can set media like this:

```astro
media={'(max-width: 750px) 100vw, 250px'}
```

So,

-   for screen width <= 750, picture will have 100% width
-   for other screens, picture will be 250px width

### quality

Type: `string` or `number` - quality (compression) of result images. Possible values: 'low' | 'medium' | 'high' | number. Default: 'medium'.

-   'low' - 50% quality
-   'medium' - 75% quality
-   'high' - 90% quality
-   number - 0-100% quality

## I have heif, not avif img in result. Why?

It because Astro v. 3 has no longer use squooshImageService() as default image service. But you can set it up in your `astro.config.mjs` file like this:

```
export default defineConfig( {
	image: {
		service: squooshImageService()
	},
} )
```

## Result images are square. Why?

Result images will have 1:1 aspect ratio. If your images have different aspect ratio, you will need to update a bit Picture component.
