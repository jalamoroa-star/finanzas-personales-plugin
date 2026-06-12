---
description: Activa cuando el usuario pregunta sobre depositos, cuentas remuneradas, cash, liquidez, Raisin, HelpMyCash, TAE, intereses, Tesoreria o cuando quiere optimizar donde tiene el dinero aparcado.
references:
  - plugin/references/perfil-financiero.md
---

# Cash Optimizer Skill

## Contexto del proyecto
El usuario tiene una aplicacion de optimizacion de cash llamada Tesoreria.
- Backend FastAPI en http://localhost:8002 (ruta: backend/app/main.py)
- Frontend React en http://localhost:3002
- Base de datos: tesoreria.db (SQLite)
- Arranque: Tesoreria start.bat (usa uvicorn app.main:app --port 8002)

## Fuentes de datos
- HelpMyCash: depositos y cuentas remuneradas del mercado espanol
- Raisin: ~150 depositos europeos (ojo: TAE incluye ~1-1.3% comision plataforma)
- Scrapers se ejecutan automaticamente cada lunes 08:00-08:15
- Verificacion cruzada: badge verde=verificado en ambas fuentes, amarillo=solo HelpMyCash, azul=solo Raisin

## APIs disponibles via MCP (estado-proyectos)
- te_depositos: depositos propios registrados
- te_ultima_recomendacion: ultima recomendacion IA de cash
- te_ejecutar_scrapers: lanza scrapers manualmente (tarda ~60s)

## Integracion con Cuentas Bancarias
- Tesoreria consume http://localhost:8001/api/cuentas/remuneradas para mostrar cuentas reales
- CB debe estar corriendo para ver datos integrados

## Como responder
- Siempre compara la TAE del deposito propio con las mejores opciones del mercado
- Recuerda que la TAE de Raisin es ~1-1.3% inferior a la TAE real del banco
- Si los scrapers tienen datos de hace mas de 7 dias, sugerir ejecutarlos de nuevo
- Presenta opciones en tabla ordenada por TAE descendente