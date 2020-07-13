# Guía de programación en Como-Quiero

Esta es la guía de uso para seguir los estándares de programación en Como-Quiero.

- Tanto el código como los comentarios deben estar en **Inglés**
- El IDE a usar debe ser **Visual Studio Code**
- Aunque encuentres lugares del código donde no se cumplan, debes seguir los estándares aquí descritos.

## Visual Studio Code

Hemos probado muchos IDE's gratuitos y este es el que mejor ha funcionado con nuestro stack, te recomendamos usarlo para poder unificar todos los plugins que usamos.

Puedes instalarlos a mano o por linea de compando:

### FRONT
code --install-extension octref.**vetur**
code --install-extension dbaeumer.**vscode-eslint**

### PHP
code --install-extension bmewburn.**vscode-intelephense-client**
code --install-extension TysonAndre.**php-phan**
code --install-extension kokororin.**vscode-phpfmt**
code --install-extension calebporzio.**simple-php-cs-fixer**

### GIT SEARCH
code --install-extension eamodio.gitlens

## Javascript

### Consideraciones generales:
- Los nombres de los archivos se escritos en **kebab-case**
- Todos los archivos deben estar en typescript **.ts**

### Librerías

#### Axios

La librería para hacer peticiones HTTP es axios. No se deben usar peticiones nativas fetch o xmlhttprequest

#### jQuery

Aunque es posible que jQuery todavía se esté usando en algun lugar del código, ya **NO** está perimitido.
En cambio puedes buscar métodos equivalentes en **lodash** importados individualmente.

### Typescript

#### Consideraciones generales:
- NO está perimitido el uso de "var"
- Se debe tipar todo lo posible
- Todo objecto que tenga relación con una base de datos debe estar tipado

Si no lo sabes utilizar muy bien no te preocupes [leete el resumen de typescript](typescript.md), es muy fácil de usar en proyectos que ya están construidos.

## Vue

### Librerías

#### Vuetify
Vuetify es la librería de estilos, como un Bootstrap con decenas de componentes listos para usar.
Cualquier componente en el código que empieze con la nomenclatura **v-** es un componente Vuetify
https://vuetifyjs.com/en/components/api-explorer

#### vue-async-computed
Esta librería sirve para usar componentes asíncronos, prácticamente todas las peticiones a la Api se realizan de esta manera tanto en componentes Vue como en los **store**.

### Estructura

La estructura esencial de los proyectos Vue es la siguiente:

```
src
├── assets              #Archivos de código de uso global
│   ├── css
│   ├── images
│   ├── js
│   └── json
├── components          #Componentes Vue
│   ├── footer.vue
│   ├── toolbar.vue
│   └── ...
├── layouts             #Layouts Vue
│   ├── default.vue
│   ├── error.vue
│   └── ...
├── libraries           #Librerías que requieren configuración
│   ├── axios.ts
│   ├── vue.ts
│   └── ...
├── pages               #Pages Vue
│   ├── about.vue
│   ├── index.vue
│   └── ...
├── plugins             #Plugins ejecutados por nuxt
│   ├── i18n.ts
│   ├── router.ts
│   └── ...
├── store               #Manejo de datos globales de múltiples componentes
│   ├── items.ts
│   ├── recipes.ts
│   └── ...
├── docker-compose.yml  #Conf. del levantamiento del proyecto en Dockers
├── package.json        #Scripts de levantar el proyecto, tests, deploy..
├── serverless.yml      #Configuración del deploy a aws
└── types.ts            #Tipados para importar en typescript
```

### store

En Como-Quiero **NO** usamos **Vuex**. No está diseñado ni para usar todo el potencial de Typescript ni para realmente usarlo de manera modular.

En cambio usamos componentes Vue que mentienen toda la reactividad pero sin una vista asociada.
Para las peticiones a la Api se utiliza **asycComputed** con el objetivo de cargar de manera reactiva contenido asíncrono.

### Ejemplo de archivo .vue

En el proyecto te vas a encontrar componentes Vue de este estilo.
Es importante que entiendas todo lo que está ocurriendo aquí a la perfección:
```
<template>
  <div class="component--example">
    <div v-for="recipeId in recipes" :key="recipeId">
      <recipe v-bind="{recipeId}">
        <div v-if="recipeNameError" class="recipename-missing">{{ recipeNameError }}</div>
        <div v-else-if="recipeName">{{ recipeName }}</div>
        <div
          v-else
          class="recipename-loading"
        >{{ $t("recipe_name_loading") || "cargando nombre de receta" }}...</div>
      </recipe>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "@/libraries/vue";
import { i18n } from "@/plugins/i18n";

import request from "@/assets/js/requests/request";
import locales from "@/store/locales";

import { Recipe } from "@/types";

const recipe = Vue.extend({
  name: "Recipe",
  props: {
    recipeId: Number
  },
  data() {
    const data = {};
    return data as typeof data & {
      recipe?: Recipe;
    };
  },
  computed: {
    lang: () => locales.lang,
    recipeName(): string {
      const recipe = this.recipe;

      if (!recipe || !recipe.translation) {
        return "";
      }

      return recipe.translation.name;
    },
    recipeNameError(): string | undefined {
      const recipe = this.recipe;
      const recipeName = this.recipeName;
      if (recipe && !recipeName) {
        return "" + i18n.t("recipe_error_not_name_found") || "nombre no encontrado";
      }
    }
  },
  asyncComputed: {
    recipe(): Promise<Recipe> {
      const lang = this.lang;
      const recipeId = this.recipeId;

      return request.get("recipes/" + recipeId + "?lang=" + lang, "Array");
    }
  }
});

export default Vue.extend({
  name: "Recipes",
  components: {
    recipe
  },
  data() {
    const data = {};
    return data as typeof data & {
      recipes?: Recipe[];
    };
  },
  asyncComputed: {
    recipesIds(): Promise<Recipe[]> {
      return request.get("recipes?pluck=id", "Array");
    }
  }
});
</script>

<style>
.component--example .recipename-missing {
  color: red;
}
.component--example .recipename-loading {
  color: grey;
}
</style>
```

### Nuxt

## BACK-END

El backend se usa siempre en forma de Api, NO hay renderización de contenido (de eso ya se encarga Nuxt).
Su funcionamiento en general debe ser muy estandarizado y simple.

### Node

Los nuevos desarrollos Backend se están realizando en Node, pero todavía se encuentran en proceso de cambio.

### PHP Laravel/Lumen

Lumen es un framewrok PHP más ligero y simplificado de Laravel, pensada para ser usada solo como Api.

#### Consideraciones generales

- Los nombres de los archivos se escritos en **PascalCase**
- Dado que no vamos a continuar con PHP no hoay que tipar el codigo.

#### Estructura

La estructura esencial es la siguiente:

```
comoquiero
├── app
│   ├── Http
│   │   └── Controllers                 #Métodos a los que llama cada ruta de la Api
│   │       ├── RecipesController.php
│   │       ├── UsersController.php
│   │       ├── ...
│   │       └── Controller.php          #Métodos globales
│   ├── User.php                        #Modelos donde se relaciona la Api con la tabla de BD
│   ├── Recipe.php
│   └── ...
├── bootstrap                           #Archivo de carga de todo el framewrok
│   └── app.php
├── config                              #Configuracioes de la Base de Datos, logs, etc..
│   ├── database.php
│   ├── logging.php
│   └── ...
├── database
│   └── migrations                      #Estructura de las tablas de BD
│       ├── 2017_01_01_000000_create_recipes_table.php
│       ├── 2017_01_01_000000_create_users_table.php
│       ├── ...
│       └── Migration.php               #Métodos globales
├── routes                              #Enrutamientos de la Api a los Controllers
│   └── web.php
├── docker-compose.yml                  #Conf. del levantamiento del proyecto en Dockers
├── package.json                        #Scripts de levantar el proyecto, tests, deploy..
└── serverless.yml                      #Configuración del deploy a aws
```

### API

Aquí se describe el estandar para las consultas a las Api GET.

#### select
Atributos que se quieren obtener, pueden estar separados con comas. Si se omite se traerán todos.
`&select=id,name`

#### where
Se puede filtrar por cualquier atributo y con formato MySql LIKE **&name=platan%**
También se pueden hacer peticiones en formato eloquent con encodeURI() `where=[['name','LIKE','platan%']]` o
`orWhere=[['name','LIKE','platan%'],['name','LIKE','manzan%']]`

#### with
Con with se pueden traer relaciones `&with=ingredients,translations`

#### groupBy
`&groupBy=id`

#### groupBy
`&orderBy=id`

#### limit
`&limit=10`

#### offset
`&offset=10`

#### aggr
first, count, sum, max.. `&aggr=first`

### pluck
Para obetener el array con el valor de una sola columna `&pluck=id`