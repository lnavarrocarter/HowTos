Vue.js 2.0
==========

Mis notas sobre Vue.js 2.0

[[TOC]]

## La Estructura de Vue

Así se inicia Vue:

    var app = new Vue({

        // El elemento del DOM sobre el cual Vue tiene control
        el: '#app',

        // Los datos que usa la instancia de Vue
        data: { 
            names: ['Tom', 'Mary', 'Peter'] 
        }

    });

