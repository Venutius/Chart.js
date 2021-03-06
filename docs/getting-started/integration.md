# Integration

Chart.js can be integrated with plain JavaScript or with different module loaders. The examples below show how to load Chart.js in different systems.

## Script Tag

```html
<script src="path/to/chartjs/dist/Chart.js"></script>
<script>
    var myChart = new Chart(ctx, {...});
</script>
```

## Common JS

```javascript
var Chart = require('chart.js');
var myChart = new Chart(ctx, {...});
```

## Bundlers (Webpack, Rollup, etc.)

```javascript
import Chart from 'chart.js';
var myChart = new Chart(ctx, {...});
```

**Note:** Moment.js is installed along Chart.js as dependency. If you don't want to use Momemt.js (either because you use a different date adapter or simply because don't need time functionalities), you will have to configure your bundler to exclude this dependency (e.g. using [`externals` for Webpack](https://webpack.js.org/configuration/externals/) or [`external` for Rollup](https://rollupjs.org/guide/en#peer-dependencies)).

```javascript
// Webpack
{
    externals: {
        moment: 'moment'
    }
}
```

```javascript
// Rollup
{
    external: {
        ['moment']
    }
}
```

## Require JS

**Important:** RequireJS [can **not** load CommonJS module as is](https://requirejs.org/docs/commonjs.html#intro), so be sure to require one of the UMD builds instead (i.e. `dist/Chart.js`, `dist/Chart.min.js`, etc.).

```javascript
require(['path/to/chartjs/dist/Chart.min.js'], function(Chart){
    var myChart = new Chart(ctx, {...});
});
```

**Note:** starting v2.8, Moment.js is an optional dependency for `Chart.js` and `Chart.min.js`. In order to use the time scale with Moment.js, you need to make sure Moment.js is fully loaded **before** requiring Chart.js. You can either use a shim:

```javascript
require.config({
    shim: {
        'chartjs': {
            deps: ['moment']    // enforce moment to be loaded before chartjs
        }
    },
    paths: {
        'chartjs': 'path/to/chartjs/dist/Chart.min.js',
        'moment': 'path/to/moment'
    }
});

require(['chartjs'], function(Chart) {
    new Chart(ctx, {...});
});
```

or simply use two nested `require()`:

```javascript
require(['moment'], function() {
    require(['chartjs'], function(Chart) {
        new Chart(ctx, {...});
    });
});
```
