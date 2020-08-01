+++
title = "Code 01 - Migrando de ES5"
tags = [
	"code",
	"work",
	"javascript",
	"esnext",
]
date = "2019-08-27"
menu = "main"
description = "Tutorial de como convertir un paquete de NPM que esta usando JavaScript ES5 a ES6"
+++

Hoy me gustaría dedicarle algo de tiempo a alguno de mis antiguos mini proyectos en JavaScript y qué mejor tarea que mover el uso de Object Prototype a Class en [rates.do](https://github.com/tavomoya/rates.do).

## Acerca de rates.do 💰
Este es un pequeño paquete npm que nos permite obtener la tasa de cambio (de dólares y euros) desde los principales bancos de la República Dominicana. Este utiliza un API público alojado en esta dirección http://api.marcos.do/.

He utilizado este paquete principalmente en trabajos universitarios hace unos años, específicamente con estos que están fuertemente ligados con actividad financiera que dependa de un manejo multimoneda.

## Lo Que Hay 💻
```javascript
var request = require('request');
var q = require('q');

// Mixin Constructor
function Rates () {

  // Define API Path
  this.apiPath = 'http://api.marcos.do';

};

// Get rates from all banks
Rates.prototype.getAllRates = function () {
  var deferred = q.defer();
  var _this = this;

  request.get(_this.apiPath+'/rates',
  function (err, res){
    if (err) {
      deferred.reject(err);
    };

    if (!res) {
      deferred.reject({
        msg: 'An unexpected error occured while fetching the rates'
      });
    } else {
      deferred.resolve({
        rates: res.body,
        status: res.statusCode
      });
    };
  });

  return deferred.promise;
};

//Get rates from the Central Bank
Rates.prototype.centralBankRate = function () {
  var deferred = q.defer();
  var _this = this;

  request.get(_this.apiPath+'/central_bank_rates',
  function (err, res){
    if (err) {
      deferred.reject(err);
    };

    if (!res) {
      deferred.reject({
        msg: 'An unexpected error occured while fetching the rates'
      });
    } else {
      deferred.resolve({
        rates: res.body,
        status: res.statusCode
      });
    };
  });

  return deferred.promise;

};

module.exports = Rates;
```
Actualmente el paquete consta de un sólo archivo llamado _index.js_. Aquí creo una función llamada _Rates_ que funciona como un “constructor” donde defino la propiedad _apiPath_. Luego utilizando Object Prototype adjunto dos funciones a mi objeto _Rates_, una se llama _getAllRates_ y la otra _centralBankRate_.

Otras cosas a destacar son las dependencias de éste paquete, [request](https://www.npmjs.com/package/request) la cual utilizo para hacer peticiones HTTP hacia la API pública ya mencionada y [q](https://www.npmjs.com/package/q) para trabajar con promises.

## Nuevas Dependencias 🆕
En la nueva versión del paquete la única dependencia que tendré será [axios](https://www.npmjs.com/package/axios) para trabajar con las peticiones HTTP en vez de request. La razón del cambio es principalmente porque _request_ no trabaja con promises de forma nativa sino con callbacks o streams de datos, sin embargo _axios_ si es basado en promises además de lo ligero que es y lo familiarizado que ya estoy con él usándolo en otro proyecto.

Para la nueva versión tampoco es necesario el uso de _q_ ya que pienso utilizar _async/await_ que ya son nativos en JavaScript.

## Nuevo Código 🆕

```javascript
'use strict';

const axios = require('axios');

class Rates {

	constructor() {
		this.api = 'http://api.marcos.do';
	}

	async GetAllRates() {
		try {
			const res = await axios.get(`${this.api}/rates`);
			return res.data;
		} catch (err) {
			console.log('err=> ', err);
			return new Error(`There was an error trying to fetch all rates => ${err}`);
		}
	};

	async GetCentralBankRates() {
		try {
			const res = await axios.get(`${this.api}/central_bank_rates`);
			return res.data;
		} catch (err) {
			console.log('err=> ', err);
			return new Error(`There was an error trying to fetch bank rates => ${err}`);
		}
	}

}

module.exports = Rates;
```

Este es el producto final y aqui una lista de los cambios claves:

* Uso del keyword **Class** para crear la clase
* Uso de **async** para hacer que las funciones devuelvan una promesa
* Uso de **try/catch** para manejo de errores dentro de las funciones
* Uso de **const** en vez de **var**
* Uso de **axios** para las peticiones HTTP
* Uso de [**template literals**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) en vez de concatenar strings

## Nota Final 📝
Todo el código descrito en este artículo se encuentra en este [branch](https://github.com/tavomoya/rates.do/tree/es-migration) en el repositorio de GitHub de rates.do. 

Enjoy 😄