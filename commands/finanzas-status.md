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
   - CB activo: llama a cb_saldos (saldo total en cuentas)
   - TE activo: llama a te_depositos (suma de depositos activos)
3. Presenta un resumen en este formato exacto:



4. Si se pasa el argumento detallado, anadir debajo:
   - Top 3 posiciones del portfolio
   - Saldo por cuenta bancaria
   - Depositos con TAE y vencimiento

## Nota
Si algun backend esta OFFLINE, mostrar el dato como N/D y recordar
al usuario que puede arrancarlo con su .bat correspondiente.