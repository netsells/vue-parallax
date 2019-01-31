# Vue Parallax
A simple vue component to add parallax background images.
## Installation
```bash
npm install @netsells/vue-parallax
``` 
or
```bash
yarn add @netsells/vue-parallax
``` 
## Basic Usage
```javascript
<v-parallax :background="[IMAGE_URL]" :speed="[PARALLAX_SPEED]">
    [INNER_HTML_CONTENT]
</v-parallax>
```

## Props
| Prop | Required | Type | Description |
| --- | --- | --- | --- |
| `background` | Required | String | Should be a URL of an image |
| `speed` | Required | Number | Should be a number between 0 and 1 |
| `outerClasses` |  | String | Additional classes to add to the outer `div` |
| `innerClasses` |  | String | Additional classes to add to the `div` with the `background-image` |

