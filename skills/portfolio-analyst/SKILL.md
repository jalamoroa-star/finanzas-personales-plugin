---
name: portfolio-analyst
description: Activa cuando el usuario pregunta sobre su portfolio, inversiones, posiciones, P&L, rentabilidad, activos, ETFs, acciones o mercados financieros. Tambien activa con menciones a Inversiones Juan, eToro, Trade Republic, Bitvavo, Revolut o Robinhood.
version: 1.0.0
---

# Portfolio Analyst Skill

## Contexto del proyecto
El usuario tiene un portfolio personal gestionado en la aplicacion Inversiones Juan.
- Backend FastAPI en http://localhost:8000
- Frontend React en http://localhost:3000
- Base de datos: inversiones.db (SQLite)
- Inversores: Juan (93.8%), David (3.1%), Gael (3.1%)
- Plataformas: eToro, Trade Republic, Bitvavo, Revolut, Robinhood

## APIs disponibles via MCP (estado-proyectos)
- ij_portfolio_resumen: valor total y P&L consolidado
- ij_portfolio_posiciones: posiciones actuales con precio en tiempo real
- ij_ultimo_informe: ultimo informe de mercado generado por IA
- ij_alertas: alertas de precio activas

## Como responder
- Siempre consulta los datos en tiempo real via MCP antes de responder
- Presenta los datos en tablas cuando hay multiples posiciones
- Calcula siempre el porcentaje de variacion ademas del valor absoluto
- Si el backend esta OFFLINE, indicalo claramente y sugiere arrancarlo con start.bat
- Ten en cuenta la distribucion multi-inversor al hablar de rentabilidad total

## Alertas importantes
- Si una posicion tiene P&L negativo superior al 10%, destacarlo
- Si hay alertas de precio disparadas, mencionarlas al inicio de la respuesta