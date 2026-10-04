# Operación diaria de Pi-hole (referencia)

Los comandos se ejecutan dentro del contenedor. Desde el host Proxmox, anteponer `pct exec <ID_CT> -- `. Sustituir los marcadores por los valores reales de cada instalación.

## Consultar

```bash
pihole-FTL --config dhcp            # ajustes de DHCP
pihole-FTL --config dhcp.hosts      # reservas (MAC,IP,nombre)
cat /etc/pihole/dhcp.leases         # concesiones activas
dig +short @IP_PIHOLE cloudflare.com  # prueba de resolución
```

## Cambiar con seguridad

```bash
# 1) Copia previa con motivo y fecha
cp /etc/pihole/pihole.toml /etc/pihole/pihole.toml.bak-<motivo>-<fecha>

# 2) Cambio (provoca una recarga de FTL y un microcorte de DNS)
pihole-FTL --config dhcp.active true|false

# 3) Verificación
dig +short @IP_PIHOLE cloudflare.com
```

## Añadir una reserva

1. Leer el valor actual completo de `dhcp.hosts`.
2. Añadir la entrada `MAC,IP,nombre`.
3. Escribir de nuevo el valor entero con `pihole-FTL --config dhcp.hosts '[ ... ]'`.
4. Verificar con una renovación en el cliente.

## Copia de configuración

```bash
pihole-FTL --teleporter    # genera un .zip con listas, ajustes, DHCP y reservas
```

La exportación se automatiza una vez por semana desde el host y se incluye en la copia de configuración.

## Volver al DHCP del router

1. Activar el servidor DHCP del router.
2. `pihole-FTL --config dhcp.active false` en el contenedor.
