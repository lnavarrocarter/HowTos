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