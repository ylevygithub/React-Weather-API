# React-Weather-API
Nous allons apprendre comment implémenter une api de météo via Reactjs en utilisant des composants jsx, bootstrap, du css et la construction d'une classe proprement.

Installez nodejs sur [nodejs.org]
Selectionnez la version **LTS**

Et puis dans le repo github vous allez taper cette commande :
*npx create-react-app weather-app*

Cela va vous créer votre app web dans le dossier *weather-app*.
Entrez dans le dossier *weather-app*.

Dans le dossier *src*, créer un dossier **app_component** et dans celui ci, un fichier **weather.component.jsx**.
Nous allons ensuite dans ce fichier, créer ce que l'on appelle, un *stateless functional component*.

### Contenue : 
```javascript
import React from "react";

const Weather = () => {
    return (
        <div className="container">
            <h1>Weather App</h1>
        </div>
    )
}

export default Weather;
```
### On va ensuite importer cette fonction *Weather* dans **App.js** et l'appeler.
```javascript
import React from 'react';
import './App.css';
import Weather from './app_component/weather.component';

function App() {
  return (
    <div className="App">
      <Weather />
    </div>
  );
}

export default App;
```
On va ensuite utiliser **bootstrap**. Pour ce faire, nous allons l'installer :
```
npm i bootstrap
```
Nous allons ensuite linker bootstrap a notre fichier App.js
On va pouvoir appeler plusieurs classes dont *py-4* qui va spécifier un top et bottom padding 
(mettre de l'espace au dessus et en dessous du champ).
On va ensuite vouloir avoir des icons dans notre projet. Pour ce faire, nous allons utiliser un projet 
github : [https://github.com/erikflowers/weather-icons].

Vous allez donc faire : 
```
npm i git@github.com:erikflowers/weather-icons.git
```
On va pouvoir ensuite importer le fichier css de ce repo qui va nous permettre d'afficher les icons 
de la météo que l'on veut.

### Notre fichier App.js ressemble enfin a cela :
```javascript
import React from 'react';
import './App.css';

import 'weather-icons/css/weather-icons.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import Weather from './app_component/weather.component';

function App() {
  return (
    <div className="App">
      <Weather />
    </div>
  );
}

export default App;
```
### Et notre weather.component.jsx ressemble a cela :
```javascript
import React from "react";

const Weather = () => {
    return (
        <div className="container">
            <div className="cards">
                <h1>London</h1>
                <h5 className="py-4">
                    <i className="wi wi-day-sunny display-1" />
                </h5>
            </div>
        </div>
    )
}

export default Weather;
```
Après cela nous allons afficher la température en degré.
Ainsi que la température minimal et maximal avec une nouvelle fonction *minmaxTemp* qui prendra en 
parametre la *température minimal et maximal*.
Ensuite nous allons mettre *la description de la température*.

Tout cela est tout d'abord codé en dure pour que vous compreniez un peu comment cela fonctionne avant 
d'avoir des valeurs dynamiques que l'on va récupérer depuis une api.

Après ces dernières modifications, le fichier **weather.component.jsx** ressemble maintenant a cela : 
```javascript
import React from "react";

const Weather = () => {
  return (
    <div className="container">
      <div className="cards">
        <h1>London</h1>
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
Vous allez maintenant vous rendre dans le site de [https://openweathermap.org/api] ou vous allez devoir 
vous inscrire pour récupérer votre *clé API* pour ensuite pouvoir afficher des données dynamiques.
Une fois que cela sera fait, vous allez vous rendre dans la catégorie API keys pour récupérer votre clé.
Copiez la et mettez la dans une constante dans votre fichier **App.js**. 
Ensuite rendez vous sur la page API du site et cliquez sur **Current Weather Data** sur le bouton doc 
pour découvrir la documentation de l'api.

Ensuite rendez vous dans la partie : *Examples of API calls*:
Et récupérez le 2e exemple.

On va enfin remplacer la fonction **App** par une class **App**. Comme ca nous pourrons jouer avec les états 
de nos valeurs dynamiquement.

### Le fichier App.js ressemble maintenant a cela :
```javascript
import React from 'react';
import './App.css';

import 'weather-icons/css/weather-icons.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import Weather from './app_component/weather.component';

//https://api.openweathermap.org/data/2.5/weather?q=London&appid={API key}
const API_key = 'Votre_api_key'

class App extends React.Component {
  constructor() {
    super();
    this.state = {};
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
On va donc maintenant créer une méthode dans cette classe, que l'on va appeler *getWeather*.
Cette méthode va être asynchrone et va nous permettre de récupérer les valeurs associées a l'exemple 
que nous avions copier tout a l'heure. Pour rappel : *api.openweathermap.org/data/2.5/weather?q=London&appid={API key}*

Dans cette méthode asynchrone nous allons avoir une constante **api_call** qui va fetch l'exemple de call 
api avec un *await*, ce qui signifie que l'on attend une donnée quand elle est prête a être reçu.
L'url que nous allons mettre à l'intérieur ne vas pas se mettre dans une simple ou double quote comme 
vous avez l'habitude de le faire. Mais va se mettre dans des backsticks : ``, ce qui va nous permettre 
de faire appel a notre constante API_key a l'intérieur de celles ci autrement ca ne fonctionnera pas.
Ce qui veut dire que l'on peut rajouter des variables a l'intérieur.

On va ensuite créer une constante **response** qui comme son nom l'indique va nous donner une réponse 
a cette appel en lui donnant comme valeur la premiere constante en format json.

Pour être sure que l'on a bien recu les données on va print dans la console la réponse.

On va enfin rajouter la méthode dans notre constructor et voir le rendu ensemble.

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
