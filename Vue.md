Vue.js 2.0
==========

Mis notas sobre Vue.js 2.0

[[TOC]]

## La Estructura y Sintaxis de Vue

Así se inicia Vue:

    <!-- No te olvides de colocar el CDN de Vue! -->
    <script src="https://unpkg.com/vue"></script>

    <script>
    var app = new Vue({

        // El elemento del DOM sobre el cual Vue tiene control
        el: '#app',

        // Los datos que usa la instancia de Vue
        data: { 
            names: ['Tom', 'Mary', 'Peter'] 
        },

        // Un método a ejecutarse luego de que la instancia de Vue está lista.
        // A veces es muy útil que sea una llamada Ajax.
        mounted() {
            // Tu código aquí 
        },

        // Métodos que se usan en esta instancia de Vue
        methods: {
            methodOne() {
                // Código
            },
            methodTwo() {
                // Código
            }
        }

    });
    </script>

En estricto rigor, una instancia de Vue es lo que se conoce como un
componente. Por lo tanto, todos los componentes Vue en Laravel pueden ser
usados de esa forma.

## Directivas Vue

Supongamos que queremos "amarrar" (bind) un valor del DOM a una propiedad
de nuestra instancia de Vue (los valores contenidos en el objeto data).
Eso se puede hacer de la siguiente manera, tomando el ejemplo anterior:

    <div id="app">
        <ul>
            <li v-for="name in names" v-text="name"></li>
        </ul>
    </div>

En el ejemplo anterior, las directivas son las propiedades HTML que comienzan
con `v-`. `v-for` realiza un *for loop* a nuestra propiedad de Vue llamada
**names**. `v-text` fija el valor del HTML de nuestro elemento a la propiedad
**name**.

Directivas hay de todos tipos, incluso para *event listeners*. Si tengo un
botón en el DOM, por ejemplo:
    
    <div id="app">
        <ul>
            <li v-for="name in names" v-text="name"></li>
        </ul>

        <!-- Este boton ejecutará el método uno de tu instancia de Vue -->
        <button v-on:click="methodOne"></button>
    </div>

Existen muchas directivas que se pueden ocupar. Para ello, la documentación
de Vue es extensa y detallada.

## Amarramiento de Atributos

Para elementos a los cuales quieras amarrar un atributo que ya existe, puedes
hacerlo utilizando la directiva `v-bind`. Por ejemplo:

    <div id="app">
        <input type="text" v-bind:placeholder="placeText">
    </div>

Esto va a amarrar el valor del placeholder de tu input a la propiedad **placeText**
de tu instancia de Vue.

## Amarramiento de Clases

Con Vue puedes añadir clases de manera condicional. Esto es un poco complejo
de entender, pero tiene sentido.

Supongamos que tenemos una clase llamada `.is-red`.
    
    <style>
        .is-red {
            background-color: red;
        }
    </style>

Luego, supongamos que en nuestro DOM tenemos un botón al cual queremos aplicar
una clase luego de que ha sido clickeado.

    <div id="app">
        <button :class="{ 'is-red': isRed }" @click="toggleClass"></button>
    </div>

El texto entre corchetes es un condicional Vue. Básicamente dice que
aplicaremos la clase `is-red` dependiento del valor de la propiedad Vue
**isRed**. Si la propiedad es falsa, entonces la clase no se aplica, pero 
si la propiedad es verdadera, entonces se aplica la clase.

También hay una directiva para un evento click que va a ejecutar la función
toggleClass. Veamos como se ve todo esto en Vue.

    <script>
    var app = new Vue({

        // El elemento del DOM sobre el cual Vue tiene control
        el: '#app',

        // Los datos que usa la instancia de Vue
        data: { 
            isRed: false 
        },
        
        // Métodos que se usan en esta instancia de Vue
        methods: {
            toggleClass() {
                // Recuerda incluir this para referirte a tus propiedades
                this.isRed = true;
            }
        }

    });
    </script>

Eso debería funcionar.

## Propiedades Calculadas

Hay veces en que no queremos utilizar nuestras propiedades de Vue así como
vienen de nuestra llamada Ajax o nuestro DOM, sino que queremos hacer algunas
modificaciones primero. Para eso existen las propiedades calculadas.

Se agregan en el objeto `computed` de tu instancia de Vue.

    <script>
        var app = new Vue({

            // El elemento del DOM sobre el cual Vue tiene control
            el: '#app',

            // Los datos que usa la instancia de Vue
            data: { 
                text: 'Hello world' 
            },

            // Aquí están las propiedades calculadad
            computed: {
                backwardsText() {
                    this.text.split('').reverse().join('');
                }
            }

        });
        </script>

Entonces, la propiedad de Vue **backwardsText** será el string **text** al
revés, cuando se llame del dom.

La ventaja de esto es que es reactivo. Si text cambia, la propiedad calculada
también lo hará. ¡Este es el poder de Vue!

## Componentes en Vue

Un componente es, en pocas palabras, un bloque HTML personalizado hecho 
en Vue. Por ejemplo, si queremos crear un componente que obtenga mis
repositorios de GitHub, y los muestre todos en el DOM de mi sitio, debemos
hacer lo siguiente:

Lo primero que debemos entender, es que para nuestras propiedades en el objeto
`data` debemos devolver un valor utilizando return(). Esto es diferente en la
instancia principal de Vue, donde data no es una función sino un objeto.

Aquí hay un [fiddle](https://jsfiddle.net/mnavarrocarter/qjcLu3y4/2/) con el experimento.

<script async src="//jsfiddle.net/mnavarrocarter/qjcLu3y4/embed/js,html,css,result/dark/"></script>

## 