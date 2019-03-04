### Static map

```jsx
import React from 'react';
import MapGL from '@urbica/react-map-gl';

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  latitude={37.78}
  longitude={-122.41}
  zoom={11}
/>;
```

### Interactive map

```jsx
import React from 'react';
import MapGL from '@urbica/react-map-gl';

initialState = {
  viewport: {
    latitude: 37.78,
    longitude: -122.41,
    zoom: 11
  }
};

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  onViewportChange={viewport => setState({ viewport })}
  {...state.viewport}
/>;
```

### Using JSON Map Style

```jsx
import React from 'react';
import MapGL from '@urbica/react-map-gl';

initialState = {
  mapStyle: null,
  viewport: {
    latitude: 37.78,
    longitude: -122.41,
    zoom: 11
  }
};

const mapStyleUrl = `https://api.mapbox.com/styles/v1/mapbox/light-v9?access_token=${MAPBOX_ACCESS_TOKEN}`;

if (!state.mapStyle && setState) {
  fetch(mapStyleUrl)
    .then(response => response.json())
    .then(mapStyle => setState({ mapStyle }));
}

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle={state.mapStyle}
  accessToken={MAPBOX_ACCESS_TOKEN}
  onViewportChange={viewport => setState({ viewport })}
  {...state.viewport}
/>;
```

### Events

`mapbox-gl` emit [events](https://www.mapbox.com/mapbox-gl-js/api/#events) in response to user interactions or changes in state.

You can find full list of supported props in [eventProps](https://github.com/urbica/react-map-gl/blob/master/src/components/MapGL/eventProps.js).

Here is an example for `onClick` prop.

```jsx
import React from 'react';
import MapGL from '@urbica/react-map-gl';

initialState = {
  viewport: {
    latitude: 37.78,
    longitude: -122.41,
    zoom: 11
  }
};

const onClick = event => {
  const { lngLat } = event;

  const newVewport = {
    ...state.viewport,
    latitude: lngLat.lat,
    longitude: lngLat.lng
  };

  setState({ viewport: newVewport });
};

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  onClick={onClick}
  onViewportChange={viewport => setState({ viewport })}
  {...state.viewport}
/>;
```

### Getting Mapbox GL JS Map Instance

#### Using `MapContext`

To access Mapbox GL JS Map instance you can use `MapContext.Consumer` component.

```jsx
import React from 'react';
import MapGL, { MapContext } from '@urbica/react-map-gl';

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  latitude={37.78}
  longitude={-122.41}
  zoom={11}
>
  <MapContext.Consumer>
    {map => {
      map.setPaintProperty('water', 'fill-color', '#fdbdba');
      return;
    }}
  </MapContext.Consumer>
</MapGL>;
```

#### Using Ref

You can also call `getMap()` method on the `MapGL` [ref](https://reactjs.org/docs/refs-and-the-dom.html).

```jsx
import React from 'react';
import MapGL from '@urbica/react-map-gl';

class MyMapGL extends React.PureComponent {
  constructor(props) {
    super(props);
    this._map = React.createRef();
  }

  componentDidMount() {
    const map = this._map.current.getMap();
    map.once('load', () => {
      map.setPaintProperty('water', 'fill-color', '#db7093');
    });
  }

  render() {
    return <MapGL {...this.props} ref={this._map} />;
  }
}

<MyMapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  latitude={37.78}
  longitude={-122.41}
  zoom={11}
/>;
```