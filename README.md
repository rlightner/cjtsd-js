# Compact JSON Time Series Data library for Javascript

This is the Javascript library for handling [Compact JSON Time Series Data (CJTSD)](https://github.com/james-hu/cjtsd-js/wiki/Compact-JSON-Time-Series-Data) data in either a browser or a Node.js environment. API documentation is available [here](cjtsd.md).

## Node.js
```
npm install cjtsd
```

```
var cjtsd = require("cjtsd");
```

## Browser
Something similar to this:
```
<script src="http://cdnjs.cloudflare.com/ajax/libs/moment.js/2.10.6/moment.min.js"></script>
<script src="https://npmcdn.com/cjtsd/dist/cjtsd.min.js"></script>
```


## Example - with C3.js

```javascript
chart1 = c3.generate({
  bindto: '#chart1',
  data: {
    x: 'time',
    xFormat: '%Y-%m-%d %H:%M',
    columns: [
    ]
  },
  axis: {
    x: {
      type: 'timeseries',
      tick: {
        format: '%Y-%m-%d %H:%M'
      }
    }
  }
});

chart2 = c3.generate({
  bindto: '#chart2',
  data: {
    x: 'time',
    xFormat: '%Y-%m-%d',
    columns: [
    ]
  },
  axis: {
    x: {
      type: 'timeseries',
      tick: {
        format: '%Y-%m-%d'
      }
    },
    y: {
      tick: {
        format: d3.format(".2%")
      }
    },
    y2:{
      show: true,
      tick: {
        format: d3.format(",")
      }
    }
  }
});
```

```javascript
$.ajax(query)
  .done(function(result){
    if (!result.error){
      var data = cjtsd.from(result);
      var timeCol = cjtsd.getFormattedTimestamps(data, 'YYYY-MM-DD', 'time');
      var dataCol = cjtsd.prepend(data.n, '# of something');
      chart1.load({
        columns: [
          timeCol,
          dataCol
        ]
      });
    }
  });

  $.ajax(query)
    .done(function(result){
      if (!result.error){
        var data = cjtsd.from(result);
        var timeCol = cjtsd.getFormattedTimestamps(data, 'YYYY-MM-DD', 'time');
        var rateCol = cjtsd.prepend(data.a, 'Rate');
        var totalCol = cjtsd.prepend(data.c, 'Count');
        chart2.load({
          columns: [
            timeCol,
            rateCol,
            totalCol
          ],
          axes: {
            'Rate': 'y',
            'Count': 'y2'
          },
          type: 'line',
          types:{
            'Count': 'bar'
          }
        });
      }
    });

```

[![Codeship Status for james-hu/cjtsd-js](https://codeship.com/projects/763e7040-3e27-0133-9aa2-5a0949beaeb8/status?branch=master)](https://codeship.com/projects/102720)
