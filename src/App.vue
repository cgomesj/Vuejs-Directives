<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Built-in Directives</h1>
        <p v-text="'Señor Bob'"></p>
        <p v-html="'<strong>Señor Bob</strong>'"></p>
      </div>
    </div>

    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Custom Directives</h1>
        <p v-highlight.delayed="'gray'">This is nice with color</p>
        <p
          v-local-highlight.delayed.blink="localDirectiveProps"
        >This is a local directive for blinking colors</p>
      </div>
    </div>

    <hr />

    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Directives Exercise</h1>
        <button v-my-on:click="clicked" class="btn btn-primary">v-my-on custom directive</button>
      </div>

      <div class="w-100 my-2"></div>

      <div
        class="ml-3"
        style="width: 100px; height: 100px; background-color: lightgreen"
        v-my-on:mouseenter="mouseEnter"
        v-my-on:mouseleave="mouseLeave"
      ></div>
    </div>
  </div>
</template>

<script>
import { setTimeout, setInterval } from "timers";
export default {
  directives: {
    "local-highlight": {
      bind(el, binding, vnode) {
        var delay = 0;

        if (binding.modifiers["delayed"]) {
          delay = 1000;
        }

        if (binding.modifiers["blink"]) {
          let mainColor = binding.value.mainColor;
          let secondColor = binding.value.secondColor;
          let currentColor = mainColor;

          setTimeout(() => {
            setInterval(() => {
              currentColor == secondColor
                ? (currentColor = mainColor)
                : (currentColor = secondColor);

              el.style.color = currentColor;
            }, binding.value.blinkDelay);
          }, delay);
        } else {
          setTimeout(() => {
            el.style.color = binding.value.mainColor;
          }, delay);
        }
      }
    },

    "my-on": {
      bind(el, binding) {
        // el.onclick = function() {
        // binding.value();
        // };

        const type = binding.arg;
        const fn = binding.value;
        el.addEventListener(type, fn);
      }
    }
  },

  data() {
    return {
      localDirectiveProps: {
        mainColor: "red",
        secondColor: "green",
        blinkDelay: 300
      }
    };
  },

  methods: {
    clicked() {
      alert("Button was clicked!");
    },

    mouseEnter() {
      console.log("Mouse entered");
    },

    mouseLeave() {
      console.log("Mouse left");
    }
  }
};
</script>

<style>
</style>
