# faust-web-component

This package provides two [web components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components) for embedding interactive [Faust](https://faust.grame.fr) snippets in web pages.

- `<faust-editor>` displays an editor (using [CodeMirror 6](https://codemirror.net/)) with executable, editable Faust code, along with some bells & whistles (controls, block diagram, plots) in a side pane.
This component is ideal for demonstrating some code in Faust and allowing the reader to try it out and tweak it themselves without having to leave the page. (For more extensive work, it also includes a button to open the code in the Faust IDE.)

- `<faust-widget>` just shows the controls and does not allow editing, so it serves simply as a way to embed interactive DSP.

These components are built on top of [faustwasm](https://github.com/grame-cncm/faustwasm) and [faust-ui](https://github.com/Fr0stbyteR/faust-ui) packages.

## Build Instructions

Clone this repository, then run:

```shell
npm install
npm run build
```

This will generate `dist/faust-web-component.js`, which you can use with a `<script>` tag.

## Example Usage

```html
<p><em>Here's an embedded editor!</em></p>

<faust-editor>
<!--
import("stdfaust.lib");
ctFreq = hslider("cutoffFrequency",500,50,10000,0.01);
q = hslider("q",5,1,30,0.1);
gain = hslider("gain",1,0,1,0.01);
process = no.noise : fi.resonlp(ctFreq,q,gain);
-->
</faust-editor>

<p><em>And here's a simple DSP widget!</em></p>

<faust-widget>
<!--
import("stdfaust.lib");
ctFreq = hslider("[0]cutoffFrequency",500,50,10000,0.01) : si.smoo;
q = hslider("[1]q",5,1,30,0.1) : si.smoo;
gain = hslider("[2]gain",1,0,1,0.01) : si.smoo;
t = button("[3]gate") : si.smoo;
process = no.noise : fi.resonlp(ctFreq,q,gain)*t <: dm.zita_light;
-->
</faust-widget>

<script src="faust-web-component.js"></script>
```

## NPM package 

 A [npm package](https://www.npmjs.com/package/@grame/faust-web-component) can be used with the CDN link: https://cdn.jsdelivr.net/npm/@grame/faust-web-component@0.2.3/dist/faust-web-component.js (possibly update the version number).
 
 Here is an HTML example using this model:
 
```html
<p><em>Here's an embedded editor!</em></p>

<faust-editor>
<!--
import("stdfaust.lib");

vol = hslider("volume [unit:dB]", -10, -96, 0, 0.1) : ba.db2linear : si.smoo;
freq1 = hslider("freq1 [unit:Hz]", 1000, 20, 3000, 1);
freq2 = hslider("freq2 [unit:Hz]", 200, 20, 3000, 1);

process = vgroup("Oscillator", os.osc(freq1) * vol, os.osc(freq2) * vol);
-->
</faust-editor>

<script src="https://cdn.jsdelivr.net/npm/@grame/faust-web-component@0.2.3/dist/faust-web-component.js"></script>
```

## Demo

A concrete use-case can be seen in the this [updated version](https://ijc8.me/faustdoc/) of the Faust documentation site.

## TODO

Several steps needs to be done before official release:

- audio input via file (including some stock signals)
- greater configurability via HTML attributes
