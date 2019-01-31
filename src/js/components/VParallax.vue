<template>
    <div :class="outerClasses" class="parallax-outer">
        <div
            :class="innerClasses"
            :style="`background-image: url('${ background }')`"
            class="background"
            ref="image"
        />
        <slot />
    </div>
</template>

<script>
    export default {
        name: 'v-parallax',

        props: {
            outerClasses: {
                type: String,
                required: false,
            },
            innerClasses: {
                type: String,
                required: false,
            },
            background: {
                type: String,
                default: '',
            },
            speed: {
                type: Number,
                default: 0,
            }
        },

        data() {
            return {
                image_visible: false,
            };
        },
        mounted() {
            if (!this.speed) {
                return this.showImage();
            }
            this.scrollImage();
            window.addEventListener('scroll', () => {
                this.scrollImage();
            });

        },

        methods: {
            /**
             * showImage
             *
             * alters image brightness filter so there is no flicker of movement when the component is loaded
             *
             * @returns {boolean}
             */
            showImage() {
                this.$refs.image.style.webkitFilter = 'brightness(0.8)';
                return this.image_visible = true;
            },

            /**
             * ScrollImage
             *
             * works out the vertical `background-position` as a percentage and applies it to the image
             */
            scrollImage() {
                const imageBounds = this.$refs.image.getBoundingClientRect();

                // Distance to the top of the image
                const distanceToTop = imageBounds.top;
                // Distance to the bottom of the image
                const distanceToBottom = imageBounds.bottom;
                // Height of the image
                const imageHeight = imageBounds.height;
                // Height of the window
                const windowHeight = window.innerHeight;
                // Distance the user has to scroll from when the top of the image appears
                // to when the bottom of the image disappears
                const onScreenDistance = windowHeight + imageHeight;
                // Distance between the bottom of the screen and the top if the image
                const distanceFromWindow = distanceToTop - windowHeight;
                // Boolean - determines if the image is on screen or not.
                const onScreen = distanceFromWindow < 0 && distanceToBottom > 0;

                if (onScreen) {
                    // This be some crazy maths
                    // It works out the percentage of how far through the image you have scrolled
                    // From the top of the image coming onto the page (0%)
                    // To the bottom of the image scrolling off (100%).
                    // Then it takes 100 away from that.
                    let backgroundPositionY = ((onScreenDistance - distanceToBottom) * 100) / onScreenDistance - 100;
                    // It then multiplies that percentage by a negative value of speed specified (e.g. 0.2)
                    backgroundPositionY = backgroundPositionY * -this.speed;
                    // And set's that value as the image's background position
                    this.$refs.image.style.backgroundPosition = `center ${ backgroundPositionY }%`;
                }
                if (!this.image_visible) {
                    this.showImage()
                }
            }
        },
    };
</script>

<style scoped>
    .parallax-outer {
        position: relative;
    }

    .background {
        filter: brightness(0);
        background-size: 120%;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        transition: filter 0.5s;
    }
</style>
