# Buscaminas 🚩 💣

> Nota: para este implementar este juego necesitás:
>
> * Saber fundamentos de programación y conocer el lenguaje Gobstones, dos temas que podés aprender en [**Mumuki**](https://mumuki.io)
> * Usar la herramienta para Proyectos **Gobstones Web** y Vestimentas, que podés aprender con este [mini tutorial](https://gobstones.github.io/gobstones-web/#/code?url=https%3A%2F%2Fgobstones.github.io%2Fproyectos-jr%2Fproyectos%2FCap.2%2F2.1.2.Bolitas%2C%20caramelos%20o%20tomates.gbp)

## Objetivos

¡Hora de hacer nuestro primer juego! Queremos programar el clásico buscaminas (así que si nunca lo jugaste antes te recomendamos que empieces por hacerlo y aprender sus reglas 😛)

¡Pero no tan rápido! Veamos algunos requerimientos mínimos de nuestro juego:

###  1. Tablero inicial

El juego iniciará con un tablero con _minas_, totalmente _tapadas_:

<img src="https://github.com/flbulgarelli/buscaminas-gobstones/raw/master/Captura1.png" alt style="max-width: 40rem">

Como ves, vamos a dejar siempre las últimas dos filas del tablero para mostrar estadísticas: la cantidad de _banderas_ (identificadas con banderas) y la cantidad de minas.

### 2. Un programa interactivo

¿Y qué hace nuestro programa? ¡Dejarnos jugar! La idea es que usando el teclado podamos recorrer el tablero e intentar encontrar las minas sin que exploten. En concreto, el objetivo es que podamos presionar las siguientes teclas:

* `arriba`, `abajo`, `izquiera`, `derecha`: para permitirnos desplazar en ese sentido
* `barra espaciadora`: para destapar una celda (⚠️ ¡Cuidado! Si hay una mina va a explotar)
* `enter`: para colocar una bandera (porque creemos que allí hay una mina :wink:)

Lo que acá vamos a necesitar no es un programa común sino uno interactivo, que se defina así (que ya dejamos definido por vos en `Buscaminas.gps`):

```gobstones
interactive program {
  INIT -> {
    PrepararTablero()
  }
  K_ARROW_LEFT -> {
    MoverHacia(Oeste)
  }
  K_ARROW_UP -> {
    MoverHacia(Norte)
  }
  K_ARROW_DOWN -> {
    MoverHacia(Sur)
  }
  K_ARROW_RIGHT -> {
    MoverHacia(Este)
  }
  K_SPACE -> {
    DestaparSiNoHayBandera()
  }
  K_ENTER -> {
    PonerOSacarBandera()
  }
}
```

### 3. Preparando el tablero

Cuando el juego arranca, debe dejar listo para jugar el tablero, mediante el procedimiento `PrepararTablero()`, que vas a tener que completar vos. Un tablero debe tener todas las celdas tapadas, minas escondidas debajo de algunas de ellas, y las _pistas_ (numeritos del 1 al 8) que nos ayudarán a encontrar las minas.


### 4. Destapando las minas

¡Ahora comienza la acción! 💪  moviéndonos con las flechas del teclado, vamos a recorrer el tablero, y destapar alguna celda, usando la `barra espaciadora`:

* si tenés buena suerte, se va a destapar un área grande en la que no hay bombas;
* si no tenés tanta suerte, se va a destapar una pista, que te va a dar decir cuantas minas hay a su alrededor;
* y si tenés mala suerte (😭) explotará una bomba y terminará el juego.

<img src="https://github.com/flbulgarelli/buscaminas-gobstones/raw/master/Captura2.png" alt style="max-width: 40rem">

Todo esto lo tenés que implementar en el procedimiento `DestaparSiNoHayBandera()`.

> Tip a la hora de jugar: obviamente al principio vas a tener que jugar al azar, después te va a convenir destapar usando las pistas 😉

### 4. Colocando banderas

¿Y si ya me convencí de que una celda hay mina? ¡Poné una bandera! Para eso tenés que implementar `PonerOSacarBandera`:

* Si hay una bandera la saca;
* y si no la hay, la pone.

Ojo: no te olvides de actualizar los contadores de banderas.

<img src="https://github.com/flbulgarelli/buscaminas-gobstones/raw/master/Captura3.png" alt style="max-width: 40rem">

### 5. Ganando el juego

¿Y cómo gano el juego? Encontrando todas las minas y señalándolas con banderas. Así que tené en cuenta que `PonerOSacarBandera` y `DestaparSiNoHayBandera` deberán chequear también que hayas puesto todas las banderas bien.

<img src="https://github.com/flbulgarelli/buscaminas-gobstones/raw/master/Captura4.png" alt style="max-width: 40rem">

### 6. ¿Y las bolitas?

Aunque vos veas minas, banderas y pistas, acordate que en Gobstones sólo podes poner y sacar bolitas en el tablero... ¿o no?

```gobstones
procedure SacarBandera() {
  Sacar(Rojo)
}
```

Como ves, si bien no podemos, por ejemplo, sacar banderas, sí podemos representarlo mediante bolitas. En otras palabras, además de programar los algoritmos tenés que pensar cómo representar las minas, banderas, pistas, huecos (celdas donde no hay nada) y las celdas tapadas. Nosotros te sugerimos la siguiente representación:

* Una bandera se representa con una bolita roja.
* Un celda tapada se representa con una bolita azul.
* Un hueco se representa con una celda sin ninguna otra bolita.
* Un mina se representa con una bolita negra.
* Las pistas (número del 1 al 8) se representan con 1 a 8 bolitas verdes.

<img src="https://github.com/flbulgarelli/buscaminas-gobstones/raw/master/Captura5.png" alt style="max-width: 40rem">

Para darte una mano con esta representación, en `Buscaminas.gbs` ya dejamos algunas funciones auxiliares que te pueden servir como punto de partida:

```gobstones
function hayPista() {
  return(hayBolitas(Verde))
}

function hayBandera() {
  return (nroBolitas(Rojo) == 1)
}

function hayMina() {
  return (nroBolitas(Negro) ==1)
}

function estaTapada() {
  return (nroBolitas(Azul) == 1)
}

function colorMarca() {
  return (Rojo)
}

function hayLineaAl(dir) {
  Mover(dir)
  return(hayLinea())
}

function hayLinea() {
  return (nroBolitas(Rojo) ==2)
}

procedure MoverHacia(dir) {
  if(puedeMover(dir) && not hayLineaAl(dir)) {
    Mover(dir)
  }
}
```

### 7. Arrancando fácil

Entrá [acá](https://gobstones.github.io/gobstones-web/#/code?github=flbulgarelli/buscaminas-gobstones) y empezá a programar 😀 .

⚠️ ¡Ojo, no te olvides de guardar lo que vas haciendo!

##  Créditos


Este ejercicio está basado en el trabajo de Evelyn Torres, Joaquín Córdova Maruskiewicz, Damián Tapia, Hernán Iñiguez y Jonathan Ibarra.

Ver: https://github.com/sagrado-corazon-alcal/buscaminas-gobstones
