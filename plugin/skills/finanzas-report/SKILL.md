---
description: Genera un informe consolidado del estado financiero personal completo
argument-hint: [semanal|mensual|completo]
allowed-tools: [mcp__estado-proyectos__check_proyectos_status, mcp__estado-proyectos__ij_portfolio_resumen, mcp__estado-proyectos__ij_portfolio_posiciones, mcp__estado-proyectos__cb_saldos, mcp__estado-proyectos__cb_proyecciones, mcp__estado-proyectos__te_depositos, mcp__estado-proyectos__te_ultima_recomendacion]
references:
  - plugin/references/perfil-financiero.md
---

# Finanzas Report

Genera un informe financiero personal consolidado con datos en tiempo real de los 3 proyectos.

## Pasos a seguir

1. Verifica que los 3 backends estan corriendo con check_proyectos_status
2. Si algun backend esta OFFLINE, avisalo antes de continuar
3. Recoge datos de los 3 proyectos en paralelo:
   - IJ: resumen portfolio + posiciones
   - CB: saldos + proyecciones
   - TE: depositos + ultima recomendacion
4. Ejecuta el VALIDATION LOOP antes de generar el informe (ver seccion abajo)
5. Genera el informe con esta estructura (incluyendo los avisos de validacion si los hay):

## Estructura del informe

### Resumen ejecutivo
- Patrimonio total (portfolio + saldos bancarios)
- Variacion respecto al mes anterior si hay datos

### Portfolio de inversiones
- Valor total y P&L
- Top 3 posiciones ganadoras y perdedoras
- Alertas de precio activas

### Cuentas bancarias
- Saldo total por cuenta
- Proyeccion de gasto proximos 3 meses

### Cash optimizado
- Depositos propios activos con TAE y vencimiento
- Ultima recomendacion IA de Tesoreria

### Proximas acciones sugeridas
- Maximo 3 acciones concretas y accionables
- Incluye sugerencias de agente especializado si se cumplen las condiciones del Agent Routing (ver seccion abajo)

## Agent Routing

Evalua estas condiciones con los datos ya recogidos. Si se cumple alguna, añade la sugerencia correspondiente al final de la seccion "Proximas acciones sugeridas", usando la sintaxis exacta indicada.

### Condicion 1 — Exceso de liquidez

Criterio: `liquidez_disponible > (umbral_minimo + 20.000 EUR)`, es decir, liquidez disponible superior a **32.000 EUR** (12.000 EUR de umbral + 20.000 EUR de exceso).

- `liquidez_disponible` = suma de saldos en cuentas corrientes y cuentas remuneradas de acceso inmediato de CB (excluye depositos a plazo)
- Si se cumple → añadir en Proximas acciones:

> 💡 Tienes liquidez por encima del nivel optimo. Para analizar opciones de depositos y maximizar la rentabilidad del exceso, invoca:
> `@cash-optimizer analiza opciones de deposito para [importe_exceso] EUR`
> (donde `importe_exceso = liquidez_disponible - 32.000`)

### Condicion 2 — Posicion con perdida relevante en portfolio

Criterio: alguna posicion individual del portfolio tiene P&L porcentual < -10%.

- Usar los datos de posiciones de IJ (`pl_percentage` o campo equivalente por posicion)
- Si hay una o mas posiciones que cumplen → añadir en Proximas acciones (lista las posiciones afectadas):

> 💡 [N] posicion(es) con perdidas superiores al 10%: [lista de tickers y P&L%]. Para un analisis de rebalanceo, invoca:
> `@portfolio-analyst analiza rebalanceo, posiciones con perdida > 10%`

### Reglas de aplicacion

- Las sugerencias de agente se añaden al final de "Proximas acciones", despues de las acciones propias del informe.
- Si ambas condiciones se cumplen, incluye ambas sugerencias.
- Si ninguna se cumple, no menciones esta logica en el informe.
- El texto de la sugerencia debe aparecer exactamente como se indica arriba (incluyendo el bloque de codigo con la sintaxis de invocacion).

## Validation Loop

Antes de mostrar el informe, ejecuta estas 3 validaciones. Si alguna falla, añade un bloque de avisos visible al inicio del informe (antes del Resumen ejecutivo).

### V1 — Doble contabilizacion del portfolio

Logica: el patrimonio total debe ser `saldo_total_CB + valor_portfolio_IJ`. El valor del portfolio IJ NO debe estar sumado dentro de los saldos de CB (son sistemas separados).

Comprobacion:
- Toma `valor_total_portfolio` de IJ (campo `total_value` o equivalente del resumen)
- Toma `saldo_total` de CB (suma de todas las cuentas)
- Calcula `TOTAL = valor_total_portfolio + saldo_total`
- Si el `saldo_total` de CB ya incluye una linea o cuenta etiquetada como "portfolio", "IJ", "inversiones" o similar con valor cercano al portfolio IJ (diferencia < 5%), marca FALLO: doble contabilizacion detectada

Si falla → aviso: `⚠️ AVISO: Posible doble contabilizacion detectada. El total mostrado puede estar inflado. Revisa si CB incluye el portfolio de IJ.`

### V2 — Valores negativos sin justificacion

Comprobacion: revisa todos los valores numericos recogidos. Un valor es sospechoso si:
- Saldo de cuenta bancaria < 0 (una cuenta en numeros rojos es posible pero debe marcarse)
- Valor total del portfolio < 0 (imposible en condiciones normales)
- P&L porcentual < -50% en alguna posicion individual (perdida extrema, puede ser error de datos)
- TAE de deposito < 0 (imposible)

Para cada valor negativo sospechoso encontrado → aviso: `⚠️ AVISO: Valor negativo detectado en [campo]: [valor]. Verifica que el dato es correcto.`

No marcar como error: P&L en euros negativo en posiciones individuales (perdidas reales son validas).

### V3 — Antiguedad de los datos

Comprobacion: verifica el campo `last_updated`, `timestamp`, `fecha` o equivalente en la respuesta de cada API.

- Si el campo existe y la fecha es anterior a hace 24h respecto a la fecha actual → FALLO para ese proyecto
- Si el campo no existe en la respuesta → marca como "antiguedad desconocida" (no es fallo critico, pero avisa)

Si falla → aviso: `⚠️ AVISO: Datos de [proyecto] con mas de 24h de antiguedad (ultima actualizacion: [fecha]). El informe puede no reflejar el estado actual.`

### Formato del bloque de avisos

Si hay 1 o mas avisos, insertalos al inicio del informe asi:

```
---
🔴 AVISOS DE VALIDACION
[lista de avisos]
---
```

Si todas las validaciones pasan, no añadas ningun bloque (informe limpio sin mencionar las validaciones).