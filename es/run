## REQUISITOS

Es enecsario tener instalados los siguientes paquetes:

- node + npm
- docker + docker-compose

## Primera vez solo

### MySql

1. Pedir un archivo actualizado **comoquierodb2.mysql**

2. Subir el la base de datos al docker mysql
  
Opción 1:
Levantar un docker phpmyadmin
```
npm run phpmyadmin
```

Opción 2:
Ejecutar el mysql en el docker por linea de comando
```
docker cp comoquierodb2.sql comoquiero_mysql_1:/comoquierodb2.sql
docker exec -it comoquiero_mysql_1 bash
mysql -uroot -proot comoquierodb2 < /comoquierodb2.sql
```

## 

Api php + nuxt:
`npm run dev`

Api2 Node (en otra pestaña):
`cd api-v2 && npm run dev`

### Helpers

Parar todos los dockers
`npm run docker-stop-all`
