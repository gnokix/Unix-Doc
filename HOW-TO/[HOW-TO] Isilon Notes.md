cmd commands

## Status | Performance | Alerts | Jobs
isi status
isi status -q
isi status -n 8

## Stadisticas de performance

```sh
isi statistics
isi statistics system --nodes --interval=2
isi statistics heat --long | head -20
isi statistics heat --classes=read,write
isi_for_array "isi statistics drive | head -5"
```

## Estadísticas por nodo:
```sh
isi statistics system --nodes –top
```

## Load Average de nodo:
```sh
isi_for_array -n 14 top
```

## Estadísticas de cliente:
```sh
isi statistics client --remote_addrs=10.20.1.34 --classes=read --interval=5
isi statistics client --classes=read --interval=5
isi statistics client --protocols smb2 --nodes=all
```

## Manejo de devices
```sh
isi devices
```

## Correr comando en todos los nodos
```sh
isi_for_array "< command to run in all nodes >"
```

## Stado caches
```sh
isi_cache_stats [-v]
```

## Jobs
```sh
isi job events list
isi_job_d status
```

## Características de hardware:
```sh
isi_for_array -n 14 isi_hw_status |head -12
```

## Wizard
```sh
isi config
```
