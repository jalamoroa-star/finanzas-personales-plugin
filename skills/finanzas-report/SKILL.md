---
description: Genera un informe consolidado del estado financiero personal completo
argument-hint: [semanal|mensual|completo]
allowed-tools: [mcp__estado-proyectos__check_proyectos_status, mcp__estado-proyectos__ij_portfolio_resumen, mcp__estado-proyectos__ij_portfolio_posiciones, mcp__estado-proyectos__cb_saldos, mcp__estado-proyectos__cb_proyecciones, mcp__estado-proyectos__te_depositos, mcp__estado-proyectos__te_ultima_recomendacion]
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
4. Genera el informe con esta estructura:

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