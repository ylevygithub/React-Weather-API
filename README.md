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

