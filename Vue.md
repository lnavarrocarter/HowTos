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
`data` debemos devolver un valor utilizando `return()`. Esto es diferente en la
instancia principal de Vue, donde data no es una función sino un objeto.

Aquí hay un [fiddle](https://jsfiddle.net/mnavarrocarter/qjcLu3y4/2/) con
el experimento. Un par de propiedades, estilos con Bulma y un poco de validación
de respuesta con `fetch()`, y listo!

## Propiedades de Componentes

Algo que no he mencionado son las propiedades de los componentes. Supongamos
que tenemos un componente de mensaje con un título y un mensaje. Usando Bulma,
nuestra plantilla sería así:

    Vue.component('message', {
        // Estas son las propiedades que nuestro componente aceptará
        props: ['title', 'body'],

        // Y luego la plantilla
        template: `
            <article class="message">
              <div class="message-header">
                <p>{{title}}</p>
                <button class="delete" aria-label="delete"></button>
              </div>
              <div class="message-body">
                {{body}}
              </div>
            </article>
        `
    });

    const app = new Vue({
        el: '#app'
    });

Luego, nuestro markup sólo tiene que hacer lo siguiente:

    <div id="app">
        <div class="container">
            <message title="Bienvenido" body="Esto es un componente de mensaje hecho con Vue.js!"></message>
        </div>
    </div>

La ventaja de esto es que si cambio el Markup, el contenido del componente cambia.

## Eventos Personalizados

Cuando desarrollamos en Vue, debemos entender que componentes son instancias
separadas de la instancia principal de Vue. Esto significa que sus propiedades,
métodos y referencias son diferentes. Pero ¿cómo lo hacemos para que ambas
interactúen entonces? Es ahí donde entran los eventos personalizados.

Supongamos que tenemos un componente llamado modal:

    Vue.component('modal', {
        template: `
            <div class="modal is-active">
              <div class="modal-background"></div>
              <div class="modal-content">
                <div class="box">
                    <slot></slot>
                </div>
              </div>
              <button class="modal-close is-large" v-on:click="$emit('close')" aria-label="close"></button>
            </div>
        `
    });

Este componente es un componente de Bulma, is está listo para usar. Lo que
tenemos que fijarnos es que al final, en el botón de cierre hay un `v-on:click="$emit('close')"`.
Lo que hace esta función es emitir un evento personalizado, que luego puede
ser escuchado por nuestra instancia `root` de Vue de la siguiente forma en
nuestro markup:

    <div id="app" class="container">
       <modal v-show="showModal" v-on:close="showModal = false">
            Hola soy un mensaje muy bonito.
       </modal>
       <button class="button is-primary is-large" v-on:click="showModal = true">Show Modal</button>
    </div>

En nuestro elemento modal, definido en nuestro root, estamos realizando una
acción al momento de cerrar el modal. Lo que hacemos es que cuando el evento
`close` es emitido, entonces actualizamos la propiedad **showModal** en `root`,
lo que va a esconder el modal. Esta es la forma de comunicar dos instancias
de Vue.

## Comunicación entre componentes Hermanos

Hay una técnica en Vue para crear una especie de instancia de Vue, como un
canal de eventos donde podemos escuchar por y emitir diferentes eventos en
Vue, para que luego otros componentes los usen. Así no estamos limitados a 
comunicación padre-hijo, sino entre componentes hermanos que pertencen a
la misma instancia principal de Vue.

Así es como definimos una instancia de eventos de Vue:

    window.Event = new Vue();

    Vue.component('algo', {
        template: `
            <button v-on:click="doSomething"></button>
        `,
        methods: {
            doSomething() {
                // En vez de emitir un evento con this.$emit, lo emitimos a una instancia evento de Vue
                Event.$emit('applied');
            }
        }
    });

    new Vue({
        el: '#app',
        
        data: {
            someData: false
        },
        
        created() {
            // Esta es la forma para escuchar eventos en Vue.
            Event.$on('applied', () => alert('Algún mensaje'));
        }

    });

Una pequeña técnica es que podemos usar sintaxis de clase ES6 para hacer un
wrapper a los métodos `$emit` y `$on`. Algo así en un archivo aparte para 
ser compilado con Webpack:
    
    window.Event = new Class {

        constructor() {
            this.vue = new Vue();
        }

        fire(event, data = null) {
            this.vue.$emit(event, data);
        }

        listen(event, callback) {
            this.vue.$on(event, callback);
        }
        
    } 

Luego, podemos usar `Event.fire('un-evento', datos)` y `Event.listen('un-evento', callback)`.