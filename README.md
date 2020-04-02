# vue-parallax-background
**Made for user in Vue 2.x** 

A lightweight component, whose only dependencies are Vue.js and core.js! Provides parallax effect for components
with both `mousemove`, and `scroll` events.

 
## Installation
`npm i vue-parallax-background` or `yarn add vue-parallax-background`

### Usage
**The background images are retrieved from the `assets/img` directory.**

#### Basic usage
The component can be used to setup a background image that responds to scroll events:
```
<template>
    <parallax-background
      :bg-img="bgImg" 
      :friction="friction"
      fixed // Optional fixed attribute to set position to fixed
      speedFactor="0.8"
      event-type="scroll"
      height="70"
    />
    ...
</template>
<script>
import ParallaxBackground from "@/components/ParallaxBackground/ParallaxBackground";
export default {
  name: "SomeComponent",
  components: { ParallaxBackground },
  data: () => ({
    bgImg: "geo building.jpg",
    friction: 1 / 1000
  })
};
</script>
```

#### Mouse movement
It can respond to mouse movement over the component itself:
```
<template>
    <parallax-background
      class="parallaxBg"
      :bg-img="bgImg"
      :friction="friction"
      speedFactor="0.8"
      event-type="mousemove"
      height="70"
    />
</template>
<script>
import ParallaxBackground from "@/components/ParallaxBackground/ParallaxBackground";
export default {
  name: "LandingPage",
  components: { ParallaxBackground },
  data: () => ({
    bgImg: "geo building.jpg",
    friction: 1 / 1000
  })
};
</script>
```

#### Gradients
You can pass in gradients to be used alone, or to be layered on-top of your image! Just put the CSS as a string into an array
and pass them into the component with property binding:
```
<template>
    <parallax-background
      class="parallaxBg"
      :bg-img="bgImg"
      :friction="friction"
      speedFactor="0.8"
      :gradients="gradients"
      event-type="mousemove"
      height="70"
    />
</template>
<script>
import ParallaxBackground from "@/components/ParallaxBackground/ParallaxBackground";
export default {
  name: "LandingPage",
  components: { ParallaxBackground },
  data: () => ({
    bgImg: "geo building.jpg",
    friction: 1 / 1000,
    gradients: [
      "radial-gradient(closest-side,rgba(52, 121, 182, 0.2), rgba(8, 8, 8, 0.4))",
      "linear-gradient(to bottom, rgba(52, 121, 182, 0.4), rgb(8, 8, 8))"
    ]
  })
};
</script>
```

#### Pass content into the component
You can use the `content` slot to pass content into the component that will be layered on top of the image:
```
<template>
    <parallax-background
      class="parallaxBg"
      :bg-img="bgImg"
      :friction="friction"
      speedFactor="0.8"
      :gradients="gradients"
      event-type="mousemove"
      height="70"
    >
        <template v-slot:content>
            <article style="padding-top: 10em;">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
                eiusmod tempor incididunt ut labore et dolore magna aliqua. Eget sit
                amet tellus cras adipiscing enim. Nunc lobortis mattis aliquam
                faucibus purus in massa. Rutrum quisque non tellus orci ac auctor. A
                lacus vestibulum sed arcu. Enim ut tellus elementum sagittis vitae
                et. Dui id ornare arcu odio.
            </article>
        </template>
</template>
<script>
import ParallaxBackground from "@/components/ParallaxBackground/ParallaxBackground";
export default {
  name: "LandingPage",
  components: { ParallaxBackground },
  data: () => ({
    bgImg: "geo building.jpg",
    friction: 1 / 1000,
    gradients: [
      "radial-gradient(closest-side,rgba(52, 121, 182, 0.2), rgba(8, 8, 8, 0.4))",
      "linear-gradient(to bottom, rgba(52, 121, 182, 0.4), rgb(8, 8, 8))"
    ]
  })
};
</script>
```
 
### Props
| Prop        	| Type    	| Description                                                                                                                              	|
|-------------	|---------	|------------------------------------------------------------------------------------------------------------------------------------------	|
| bgImg       	| String  	| Name of the image to lookup including the file type. Must be located in @/assets/img                                                     	|
| breakpoint  	| String  	| Used to control breakpoints in the page, which handles the scroll listener adding and removal for page resizing                          	|
| direction   	| String  	| Determines which direction the parallax should work on; moves the image "up" or "down"                                                   	|
| eventType   	| String  	| Can be either "mousemove" or "scroll". Determines what event the parallax effect works with                                              	|
| fixed       	| Boolean 	| If present, determines if the background image is fixed or not                                                                           	|
| friction    	| Number  	| Friction co-efficient. Ex) 1/1000. Used by mousemove events, determines how responsive the image is to mouse movement                    	|
| gradients   	| Array   	| An array of CSS gradients, as strings. These can be used without a background image, or in conjunction with one                          	|
| height      	| String  	| Height of the containing element for the image. Pass in the raw number, and it is converted to the "vh" equivalent                       	|
| speedFactor 	| Number  	| Used with the scroll event, determines how quickly the image moves in response to page scrolling. Higher numbers equate to more movement 	|
