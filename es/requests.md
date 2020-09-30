# Requests
Los métodos `/assets/request/` sirven para hacer peticiones a las Api haciendo uso de los cache local (LocalForage) y de servidor (Cloudfront).
Se dividen en:
- `import request from "@/assets/js/requests/request";`
- `import http from "@/assets/js/requests/http";`
- `import storage from "@/assets/js/requests/storage";`

Piden siempre el tipo que debe retornar `"Number" | "String" | "Object" | "Array" | "Boolean"` para capturar errores en las respuestas de las Api.

## @/assets/js/requests/request
Los métodos de `requests/request` buscan primero en cache local de las últimas 2 horas y si no existe hace una peticion axios.

### pub()
Petición sin enviar el header Authorization. (Mismo resultado para cualquier usuario)
```
pub(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean"): Promise<any>
```
ej: `request.pub("recipes", "Array");`

### get()
Petición enviando el header Authorization. (Resultado personalizado).
```
get(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean", headers?: any): Promise<any>
```
ej: `request.get("recipes", "Array");`

### stale()
Devuelve el resultado del cache, y si el cache caducó hace una petición axios. Retorna por segunda vez un resultado si el contenido cambió.
```
stale(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean", headers?: any): Promise<any>
```
ej: `request.stale("recipes", "Array");`

## @/assets/js/requests/http
Los métodos de `requests/http` ignoran el cache local y hacen una peticion axios.

### get()
Petición enviando el header Authorization. (Resultado personalizado)
```
get(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean", headers?: any): Promise<any>
```
ej: `http.get("recipes", "Array");`

### stale()
Devuelve el resultado del cache y al mismo tiempo hace una petición axios. Retorna por segunda vez un resultado si el contenido cambió.
```
stale(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean", headers?: any): Promise<any>
```
ej: `http.stale("recipes", "Array");`

## @/assets/js/requests/storage
Los métodos de `requests/storage` solo buscan la peticion en el cache local.

### get()
Busca la data en el cache local
```
get(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean", headers?: any): Promise<any>
```
ej: `storage.get("recipes", "Array");`

### set()
Guarda la data en el cache local
```
set(key: string, type: "Number" | "String" | "Object" | "Array" | "Boolean", headers?: any): Promise<any>
```
ej: `storage.set("recipes", []);`
