# Migración del DHCP del router a Pi-hole

## Objetivo

Que Pi-hole sea el único servidor DHCP de la red para que cada cliente aparezca por nombre en las consultas DNS y las reservas queden en una configuración exportable.

## Antes de empezar

- [ ] Inventario de dispositivos con IP estática y su dirección actual.
- [ ] Rango DHCP elegido de forma que **no incluya** ninguna IP estática existente.
- [ ] Copia del fichero de configuración de Pi-hole (`pihole.toml`) con fecha.
- [ ] Acceso físico o por consola al servidor por si algo falla.
- [ ] Procedimiento de vuelta atrás escrito.

## Pasos

1. Configurar en Pi-hole: rango (`RANGO_DHCP`), máscara, puerta de enlace (`IP_ROUTER`) y tiempo de concesión.
2. Añadir las reservas de los equipos de infraestructura con formato `MAC,IP,nombre`.
3. **Desactivar el DHCP del router.**
4. Activar el DHCP en Pi-hole.
5. Renovar la concesión en un cliente y comprobar que recibe como DNS la IP del Pi-hole.
6. Revisar las concesiones y las consultas DNS por nombre.
7. Revisar las aplicaciones que usan IPs de dispositivos (domótica, TV, cámaras) y reservar las que hayan cambiado.

## Vuelta atrás

1. Reactivar el servidor DHCP del router.
2. Desactivar el DHCP en Pi-hole.

Los dispositivos conservan su IP hasta que renueven.

## Problemas encontrados

- Equipos con varias IP estáticas dentro del rango previsto: se movió el inicio del rango.
- Dispositivos domóticos que cambiaron de IP al renovar: se añadieron reservas por MAC.
- Cada cambio de configuración provoca un microcorte de DNS: se hace en momentos tranquilos y se verifica después.
