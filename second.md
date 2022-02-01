### Le fichier ressemble dès à présent a cela : 
```javascript
import React from 'react';
import './App.css';

import 'weather-icons/css/weather-icons.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import Weather from './app_component/weather.component';

//https://api.openweathermap.org/data/2.5/weather?q=London,uk&appid={API key}
const API_key = 'Votre_api_key'

class App extends React.Component {
  constructor() {
    super();
    this.state = {};
    this.getWeather();
  }

  getWeather = async() => {
    const api_call = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=London,uk&appid=${API_key}`
    );

    const response = await api_call.json();

    console.log(response);
  }

  render() {
    return (
      <div className="App">
        <Weather />
      </div>
    )
  }
}

export default App;
```
### Vous devriez avoir une ligne qui ressemble a cela dans la console :
```
{coord: {…}, weather: Array(1), base: 'stations', main: {…}, visibility: 10000, …}base: "stations"clouds: {all: 20}cod: 200coord: {lon: -0.1257, lat: 51.5085}dt: 1643646327id: 2643743main: {temp: 280.46, feels_like: 277.11, temp_min: 279.36, temp_max: 281.35, pressure: 1026, …}name: "London"sys: {type: 2, id: 2019646, country: 'GB', sunrise: 1643614837, sunset: 1643647626}timezone: 0visibility: 10000weather: Array(1)0: {id: 801, main: 'Clouds', description: 'few clouds', icon: '02d'}length: 1[[Prototype]]: Array(0)wind: {speed: 5.66, deg: 300, gust: 13.89}[[Prototype]]: Object
```
Et maintenant, nous allons récupérer ces vrais valeurs pour ensuite les afficher.

Nous allons commencer par la **city** et la **country** que nous allons rajouter au state de la class.
dans le return nous allons passer des propriétées a notre component **Weather**.
Pour ensuite recuperer les **props** dans le fichier jsx pour afficher a la place de London, 
la bonne city et la bonne country.

### App.js ressemble désormais a cela :
```javascript
import React from 'react';
import './App.css';

import 'weather-icons/css/weather-icons.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import Weather from './app_component/weather.component';

//https://api.openweathermap.org/data/2.5/weather?q=London,uk&appid={API key}
const API_key = 'Votre_api_key'

class App extends React.Component {
  constructor() {
    super();
    this.state = {
      city: undefined,
      country: undefined
    };
    this.getWeather();
  }

  getWeather = async() => {
    const api_call = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=London,uk&appid=${API_key}`
    );

    const response = await api_call.json();

    console.log(response);

    this.setState({
      city: response.name,
      country: response.sys.country
    })
  }

  render() {
    return (
      <div className="App">
        <Weather city={this.state.city} country={this.state.country} />
      </div>
    )
  }
}

export default App;
```
### Et le fichier weather.component.jsx ressemble a cela :
```javascript
import React from "react";

const Weather = (props) => {
  return (
    <div className="container">
      <div className="cards">
        <h1>{props.city}, {props.country}</h1>
        <h5 className="py-4">
          <i className="wi wi-day-sunny display-1" />
        </h5>
        <h1 className="py-2">25&deg;</h1>

        {/** show max and min temp */}
        {minmaxTemp(24, 19)}

        {/** Description of the weather */}
        <h4 className="py-3">Slow Rain</h4>
      </div>
    </div>
  )
}

function minmaxTemp(min, max) {
  return (
    <h3>
      <span className="px-4">{min}&deg;</span>
      <span className="px-4">{max}&deg;</span>
    </h3>
  )
}

export default Weather;
```
*Nous allons maintenant faire la même chose avec l'icon, la temperature actuelle, la température 
minimal, maximal et la description.*

Avant de vous laisser faire cela par vous même, je vais quand même vous donner des petits indices 
assez compliqué a deviner seul.
Pour les températures que vous allez récupérer de l'objet *response*, elles sont de type Kelvin et 
nous les voullons en degrés celsius.
Pour ce faire, vous allez devoir créer une méthode de conversion qui va faire un calcul, que nous 
allons appeler **calCelsius**.
### La voici :
```javascript
  calCelsius(temp) {//Kelvin type to celsius
    let cell = Math.floor(temp - 273.15)
    return cell;
  }
```
Ensuite pour la description, cherchez bien dans l'objet.

Et enfin pour l'icon, je vous donne ce lien : [https://openweathermap.org/weather-conditions]
Cela peut être assez compliqué pour certains donc je vous donne un autre indice. Vérifiez *l'id* que 
vous recevrez dans l'objet, et par rapport a cela, affichez l'icon correspondant.

Voici les différents icons que nous attendrons a la fin :
**Thunderstorm, Drizzle, Rain, Snow, Atmosphere, Clear, Clouds.**


........


Lorsque vous aurez fini, le rendu devrait ressembler a cela :

### App.js :
```javascript
import React from 'react';
import './App.css';

import 'weather-icons/css/weather-icons.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import Weather from './app_component/weather.component';

//https://api.openweathermap.org/data/2.5/weather?q=London,uk&appid={API key}
const API_key = 'Votre_api_key'

class App extends React.Component {
  constructor() {
    super();
    this.state = {
      city: undefined,
      country: undefined,
      icon: undefined,
      main: undefined,
      celsius: undefined,
      temp_max: undefined,
      temp_min: undefined,
      description: "",
      error: false
    };
    this.getWeather();
    this.weatherIcon = {
      Thunderstorm: "wi-thunderstorm",
      Drizzle: "wi-sleet",
      Rain: "wi-storm-showers",
      Snow: "wi-snow",
      Atmosphere: "wi-fog",
      Clear: "wi-day-sunny",
      Clouds: "wi-day-fog"
    }
  }

  calCelsius(temp) {//Kelvin type to celsius
    let cell = Math.floor(temp - 273.15)
    return cell;
  }

  get_WeatherIcon(icons, rangeId) {
    switch(true) {
      case rangeId >=200 && rangeId <= 232:
        this.setState({icon: this.weatherIcon.Thunderstorm})
        break
      case rangeId >=300 && rangeId <= 321:
        this.setState({icon: this.weatherIcon.Drizzle})
        break
      case rangeId >=500 && rangeId <= 531:
        this.setState({icon: this.weatherIcon.Rain})
        break
      case rangeId >=600 && rangeId <= 622:
        this.setState({icon: this.weatherIcon.Snow})
        break
      case rangeId >=701 && rangeId <= 781:
        this.setState({icon: this.weatherIcon.Atmosphere})
        break
      case rangeId === 800:
        this.setState({icon: this.weatherIcon.Clear})
        break
      case rangeId >=801 && rangeId <= 804:
        this.setState({icon: this.weatherIcon.Clouds})
        break
        default:
          this.setState({icon: this.weatherIcon.Clouds})
    }
  }

  getWeather = async() => {
    const api_call = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=London,uk&appid=${API_key}`
    );

    const response = await api_call.json();

    console.log(response);

    this.setState({
      city: response.name,
      country: response.sys.country,
      celsius: this.calCelsius(response.main.temp),
      temp_max: this.calCelsius(response.main.temp_max),
      temp_min: this.calCelsius(response.main.temp_min),
      description: response.weather[0].description,
    })

    this.get_WeatherIcon(this.weatherIcon, response.weather[0].id)
  }

  render() {
    return (
      <div className="App">
        <Weather
          city={this.state.city}
          country={this.state.country}
          temp_celsius={this.state.celsius}
          temp_max={this.state.temp_max}
          temp_min={this.state.temp_min}
          description={this.state.description}
          weatherIcon={this.state.icon} />
      </div>
    )
  }
}

export default App;
```

Ensuite ce que l'on veut avoir, ce sont des champs nous permettant de choisir la ville et le pays dont 
nous voulons connaitre la température.
Pour ce faire, dans le dossier **app_component**, nous allons créer un fichier **form.component.jsx** 
pour créer notre formulaire.
Nous voulons avoir un champ *input city*, un champ *input country* et un champ *boutton* pour valider 
les champs.


### Le fichier devrait ressembler a cela a la fin du compte :
```javascript
import React from "react";

const Form = props => {
  return (
    <div className="container">
      <div className="row">
        <div className="col-md-3 offset-md-2">
          <input type="text" className="form-control" name="city" autoComplete="off"/>
        </div>
        <div className="col-md-3">
          <input type="text" className="form-control" name="country" autoComplete="off"/>
        </div>
        <div className="col-md-3 mt-md-0 text-md-left">
          <button className="btn btn-warning">Get Weather</button>
        </div>
      </div>
    </div>
  )
}

export default Form;
```
Et dans App.js faite un **import de Form** comme avant pour **Weather**.
Et au niveau du return vous devez mettre le Form juste **au dessus** du Weather.

Ensuite nous allons ajouter du style personnalisé a tout cela.
En créant le fichier **form.style.css** toujours dans le même dossier.

Vous pouvez voir le rendu final dans le dossier rendu_final.

Merci a tous pour votre intérêt pour ce workshop.
