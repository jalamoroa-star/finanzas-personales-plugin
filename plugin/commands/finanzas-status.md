---
description: Muestra el estado de los 3 proyectos financieros y un resumen rapido del patrimonio
argument-hint: [detallado]
allowed-tools: [Bash]
---

# Comando: /finanzas-status

## Que hace
Muestra en 30 segundos el estado completo de tus finanzas personales.

## Pasos

1. Llama a check_proyectos_status para ver que backends estan activos
2. Para cada backend activo, recoge el dato clave:
   - IJ activo: llama a ij_portfolio_resumen (valor total del portfolio)
   - CB: obtener saldos SIEMPRE via `curl -s http://localhost:8001/api/cuentas/saldos`
     (NO usar el MCP sqlite-cuentas, apunta a un fichero invalido)
     Si el curl falla (puerto cerrado), leer el ultimo snapshot de:
     backend/data/processed/patrimonio_snapshots.json (campo "liquidez")
   - TE activo: llama a te_depositos (suma de depositos activos)
3. Presenta un resumen en este formato exacto:



4. Si se pasa el argumento detallado, anadir debajo:
   - Top 3 posiciones del portfolio
   - Saldo por cuenta bancaria
   - Depositos con TAE y vencimiento

## Calculo del patrimonio total
- El valor de CB es SOLO liquidez bancaria (NO incluir portfolio de IJ)
- El valor de IJ es el portfolio de inversiones
- El valor de TE es la suma de depositos contratados
- TOTAL = CB (liquidez) + IJ (portfolio) + TE (depositos)
- Nunca sumes el portfolio de IJ al total de CB para evitar doble contabilizacion

## Nota
Si algun backend esta OFFLINE, mostrar el dato como N/D y recordar
al usuario que puede arrancarlo con su .bat correspondiente.