Diseño Orientado a Objetos Práctico en Ruby
===========================================
por Sandi Metz

## Diseño Orientado a Objetos

El mundo es procedural (tiempo corre y eventos pasan, uno por uno) pero
también es orientado a objetos (objetos interactúan e influencian a otros).

Las aplicaciones son *objetos* que pasan *mensajes* entre sí. Los objetos se
comunican creando puentes o canales entre ellos, llamados *dependencias*. 
El cambio es difícil cuando los objetos en la aplicación *saben mucho*. 
Es decir, cuando no siguen el PRU (Principio de Responsabilidad Única).

La importancia de ser un buen diseñador radica en la verdad inmutable de que
el software *va a cambiar*. Un buen diseño prepara al software para recibir
cambios, pero no anticipandolos (como adivinando el futuro) sino haciendo 
el software flexible. Diseño es el **arte de componer código** para que eso
pase.

Algunos principios: 

1. SOLID: **S**ingle Responsibilty, **O**pen-Closed, **L**iskov 
Substitution, **I**nterface Segregation, **D**ependency Inversion
2. DRY: **D**on't **R**epeat **Y**ourself. 
3. KISS: **K**eep **i**t **S**imple, **S**tupid.

Hay evidencia empírica que muestra la efectividad de estos principios.

El diseño, además de **Principios**, involucra **Patrones**. Patrones son
soluciones estándar para ciertos problemas específicos. Algo así como
técnicas.

Aplicaciones mal diseñadas se autodestruyen con el tiempo: "Si, puedo añadir
ese *feature*, pero va a **romper** todo." Se puede sobrediseñar también: "No
puedo añadir ese *feature*, no fue diseñado para eso."

DOO falla cuando el acto de diseñar está separado del acto de programar. 
(Agile)[http://agilemanifesto.org/] VS BUFD (Big Upfront Desing). El diseño
requiere feedback constante y evoluciona con el feedback. BUFD te da sentido
de control, pero es una ilusión. Independiente de las espicificaciones
iniciales, el software termina siendo diferente: el diseño se resiste al
encapsulamiento en una toma de requerimientos. BUFD pone a programadores y
clientes en una relación de enemigos.

Agile funciona porque reconoce que la certeza no se puede tener antes de
que la aplicación sea desarrollada. Es flexible al cambio. No significa que
no considera el diseño, sino que se enfoca en otros términos. BUFD considera
un diseño absoluto, pero Agile un diseño más holístico: cómo integrar y pensar
las partes de tu aplicación. Diseño es aplicar las técnicas de la teoría en 
el momento que la práctica lo requiere.

## Diseñando Clases con Responsabilidades Únicas

Programamos para que el código sea fácil de cambiar. Fácil de cambiar significa
que los cambios no tienen efectos colaterales inesperados, que cambios pequeños
en los requerimientos corresponden a pequeños cambios en código, que el código
que existe es fácil de reutilizar y que la forma más fácil de cambiar es
añadir código que es fácil de cambiar en sí mismo.

Entonces, nuestro código debe ser TRUE: 

- **T**ransparente: Las consecuencias del cambio deberían ser obvias en el
código que está cambiando y en el código distante que depende de él.
- **R**azonable: el costo del cambio debe ser proporcional al beneficio de
cambiarlo.
- **U**sable: El código existente debería ser usado en nuevos e inesperados
contextos.
- **E**jemplar: El código debería inspirar a otros a perpetuar estas
cualidades.

Veamos el ejemplo de una aplicación que calcula la proporción de movimiento
de una bicicleta de acuerdo a sus cambios. Entonces, en PHP:

    class Cambio {

        public $plato
        
        public $piñon

        public function __construct($plato, $piñon)
        {
            $this.plato = $plato;
            $this.piñon = $piñon;
        }

        public function proporcion()
        {
            $this.plato / $this.piñon;
        }

        echo new Cambio(52, 11)->proporcion()   // 4.727272727273
        echo new Cambio(30, 27)->proporcion()   // 1.111111111111

    }

Si añadieramos a esto el calcular las pulgadas de cambio, deberíamos poner
el diámetro de la rueda. Pero la rueda es un objeto distinto al cambio. Es
aquí cuando diseñar nuestra aplicación entra en juego. Quizás necesitamos
otra clase.

Para determinar PRU, debemos tratar a la clase como una persona y hacerle
preguntas: "Señor Cambio, ¿cuantos dientes tiene su plato? ¿cuantos dientes
tiene su piñon? ¿cuál es su proporción?" Si introdujeramos una rueda, no 
suena muy bien decir "Señor Cambio ¿cuál es el diámetro de su rueda?". Ahí
es dónde necesitamos otra clase.

Además, una clase debería tener una responsabilidad fácil y corta de describir.
Si se necesita usar un "y" o un "o" es un indicador de que hay que crear
otra clase.

### Pensando en comportamiento, no datos
Para escribir código que se adapta al cambio, se debe pensar en términos de
comportamiento, y no datos. Por eso, cada propiedad debería tener "mutators",
o "getters and setters" (métodos que asignan o obtienen el valor de una 
propiedad en una clase).

    class Cambio {

        private $plato
        
        private $piñon

        public function __construct($plato, $piñon)
        {
            $this->plato = $plato;
            $this->piñon = $piñon;
        }
        
        public function setPlato($plato)
        {
            $this->plato = $plato;
        }

        public function getPlato()
        {
            return $this->plato;
        }

        // And so on...

        public function proporcion()
        {
            $this->plato / $this->piñon;
        }

        echo new Cambio(52, 11)->proporcion()   // 4.727272727273
        echo new Cambio(30, 27)->proporcion()   // 1.111111111111

    }

Siempre es buena práctica esconder las propiedades de tu clase con un private.
Así, otras clases ganan acceso a las propiedades por tus getters o setters,
sólo cuando se están comunicando con tu clase. Esto significa cambiar de
datos a comportamiento.

### Escondiendo estructuras de datos complejas

Evitar usar acceder a objetos como `$sum = value[1] + value [2]`. Esto es
código mal referenciado. Es mejor definir qué es `value[1]` y `value[2]` en
una propiedad o algo. Ponerles un nombre.

### Extrayendo responsabilidades de métodos

Cuando un método hace más de una cosa, debería separarse. Lo mismo una clase.
Si un método hace una iteración y otro una suma, hay que separar esas dos
acciones.

### Isolando responsabilidades extras en clases

Cuando una clase parece que tuviera más responsabilidades que las que debería
tener, debemos pensar en hacer más clases.