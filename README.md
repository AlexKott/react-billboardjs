# react-billboardjs

<img src="https://img.shields.io/badge/build-passing-brightgreen.svg"/>
<img src="https://img.shields.io/badge/coverage-100%25-brightgreen.svg"/>
<img src="https://img.shields.io/badge/license-MIT-blue.svg"/>

React component for the billboard.js charting library

*This is based on [react-c3js](https://github.com/bcbcarl/react-c3js), with modifications specific to billboard and enhancements*

## Table of contents
* [Installation](#installation)
* [Usage](#usage)
* [Props](#props)
  * [className](#classname)
  * [isPure](#ispure)
  * [style](#style)
  * [unloadBeforeLoad](#unloadbeforeload)
* [Managing the internal chart](#managing-the-internal-chart)
  * [loadData](#loaddata)
  * [redraw](#redraw)
  * [unloadData](#unloaddata)
  * [Chart instance](#chart-instance)
* [Development](#development)

## Installation

```
$ npm install react-billboardjs --save
```

## Usage

```javascript
import React, {Component} from 'react';

// component and styles
import BillboardChart from 'react-billboardjs';
import 'react-billboardjs/lib/billboard.css';

const CHART_DATA = {
  columns: [
    ['data1', 30, 20, 50, 40, 60, 50],
    ['data2', 200, 130, 90, 240, 130, 220],
    ['data3', 300, 200, 160, 400, 250, 250]
  ],
  type: 'line'
};

class LineChart extends Component {
  render() {
    return (
      <BillboardChart data={CHART_DATA}/>
    );
  }
}
```

Make sure to include the provided CSS file to ensure that all appropriate styles for billboard are included as well. The example above is if you are using `webpack` or a similar bundler, but the styles are global so bring them in however is best for your application.

## Props

All top-level properties available on the [`billboard.js` options](https://naver.github.io/billboard.js/release/latest/doc/) are passable as props, so for more detail about each of those props please check there. There are also a few additional props specific to the component:
* className `{string}`
* isPure `{boolean}`
* style `{Object}`
* unloadBeforeLoad `{boolean}`

#### className

An additional `className` that is passed to the element that the chart is rendered into.

```javascript
<BillboardChart
  className="fancy"
  ...
/>
```

#### isPure

Are the prop values passed based on a shallow-equal comparison of props and context. This can prevent unnecessary re-renders when set to `true`, but expects any prop changes to be new objects (meaning arrays / objects that are mutated will not trigger a render).

```javascript
<BillboardChart
  isPure
  ...
/>
```

#### style

An additional `style` object that is passed to the element that the chart is rendered into.

```javascript
const STYLE = {
  display: 'inline-block'
};

<BillboardChart
  style={STYLE}
  ...
/>
```

One caveat to keep in mind is that there are two styles that will always be applied from `billboard.js` even if the properties are included in the `style` object:

* `max-height` (dynamically calculated based on the height of the container)
* `position` (set to `relative`)

If you want either of these to apply to the chart, the easiest way to accomplish this is to have a standard `<div>` that wraps the chart that you can apply these styles to.

#### unloadBeforeLoad

Should the current data be unloaded before the new data will be loaded.

```javascript
<BillboardChart
  unloadBeforeLoad
  ...
/>
```

## Managing the internal chart

If you capture the `ref` of the chart, you will gain access to the instance, which allows you to use both the component methods and `billboard.js` native methods.

```javascript
class Chart extends PureComponent {
  getRef = (ChartInstance) => {
    this.chartInstance = ChartInstance;
  };

  render() {
    return (
      <BillboardChart
        data={...}
        ref={this.getRef}
      />
    );
  }
}
```

#### loadData

Loads new data into the chart (equivalent to the native [Chart.load](https://naver.github.io/billboard.js/release/latest/doc/Chart.html#load) method).

```javascript
this.chartInstance.loadData({
  columns: [
    ['data1', 100, 50]
  ]
});
```

#### redraw

Forces a redraw of the chart.

```javascript
this.chartInstance.redraw();
```

#### unloadData

Loads new data into the chart (equivalent to the native [Chart.unload](https://naver.github.io/billboard.js/release/latest/doc/Chart.html#unload) method).

```javascript
this.chartInstance.unloadData({
  ids: ['data1']
});
```

#### Chart instance

If you want to access the native `billboard.js` chart instance, it is available on the `chart` property of the `ref`.

```javascript
this.chartInstance.chart.defocus('data1');
```

## Development

Standard stuff, clone the repo and `npm install` dependencies. The npm scripts available:
* `build` => run webpack to build development `dist` file with NODE_ENV=development
* `build:minifed` => run webpack to build production `dist` file with NODE_ENV=production
* `copy:css` => copy the `billboard.css` file from `billboard.js` package to `src`
* `dev` => run webpack dev server to run example app (playground!)
* `lint` => run ESLint against all files in the `src` folder
* `lint:fix` => run `lint` with `--fix` applied
* `prepublish` => runs `compile-for-publish`
* `prepublish:compile` => run `lint`, `test`, `transpile:es`, `transpile:lib`, `build`, and `build:minified` scripts
* `test` => run AVA test functions with `NODE_ENV=test`
* `test:coverage` => run `test` but with `nyc` for coverage checker
* `test:watch` => run `test`, but with persistent watcher
* `transpile:lib` => run babel against all files in `src` to create files in `lib`
* `transpile:es` => run babel against all files in `src` to create files in `es`, preserving ES2015 modules (for [`pkg.module`](https://github.com/rollup/rollup/wiki/pkg.module))