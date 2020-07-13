
# Typescript

Siempre que sea posible, todo se va a escribir en Typescript, si no lo conoces mucho no te preocupes porque es muy fácil de usar en proyectos que ya están armados:

## 1. No está perimitido el uso de "var"

En cambio se usan:
- const: variables "constantes" que no se van a reasignar.
- let: del verbo **let**, variables que se van a volver a reasignar.

Como norma general todas las variables son **const** hasta que typescript te alerte de que estamos reasignando una variable, en ese caso la cambiamos a **let**

## 2. Es obligatorio tipar todo

*Esto es realmente importante en el caso de Vue por typescript puede perderse y dejar de encontrar elementos dentro del componente.

Ejemplo de los tipados nativos más comunes
```
let str: string;
let num: number;
let bool: boolean
let atrArr: string[];
let numArr: string[];
let strOrNum: string | number;

function fill() {
  str = "abc";
  num = 123;
  bool = true;
  strArr = ["a", "b", "c"];
  numArr = [1, 2, 3];
  strOrNum = "abc" || 123;
}
```

**NO** hay que tipar cuando una variable ya se define con un valor intrínseco:
```
let str = "";
let num = 0;
let bool = false;
let atrArr = [""];
let numArr = [0];

function fill() {
  str = "abc";
  num = 123;
  bool = true;
  strArr = ["a", "b", "c"];
  numArr = [1, 2, 3];
}
```

## 3. Tablas de la base de datos

Todo objecto que tenga relación con una base de datos debe estar tipado.
Por defecto todos los tipos de las tablas de la base de datos se van a poder importar **from "@/types";**

```
import { Recipe } from "@/types";

function getRating(recipe: Recipe): number {
  return Recipe.rating;
}
```

## 4. Otros valores

### null / undefined

Los valores null y undefined sirven para añadir un tipo cuando no se ecuentra un valor.

```
function getRating(recipe: Recipe | undefined): number | undefined {
  if (!recipe) {
    return;
  }
  return Recipe.rating;
}
```

Para acortar usamos **?** después de la declaración de una variable/atributo

```
function getRating(recipe?: Recipe): number | undefined {
  if (!recipe) {
    return;
  }
  return Recipe.rating;
}
```

### any

El único momento donde está permitido usar **any** es en objectos de los que no dispongamos su tipado o para objetos que no podemos conocer su valor. Así evitaremos que salga la alerta de typescript.

```
const options: any = {
  defaultAttribute: 123
}
if (extraOption) {
  options.extra = true;
}
libraryWithoutTypescriptSupport(options);
```

### void

El tipo **void** es usado para definir que una funcion no devuelve nada:
```
function add(obj): void {
  obj.num++;
}
```

### never

Este tipado puede aparecer cuando según typescript el valor es imposible pero por lgún motivo necesitamos filtrarlo igualmente.
Por ejemplo la variable str nunca debería poder undefined:
```
function(str: string) {
  if ("undefined" == typeof str) {
    return str;
  }
  [...]
}
```