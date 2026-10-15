# Business Case ML

Aplicación guiada para armar el business case de un modelo predictivo: CAPEX (horas de construcción e infraestructura de desarrollo), OPEX (servicios, nube mensual y otros costos) y beneficio incremental. Calcula ROI, payback, VAN y la conversión de equilibrio.

Es un sitio estático de un solo archivo (`index.html`), sin build ni dependencias que instalar.

## Publicar en Netlify

**Opción rápida (arrastrar y soltar)**
1. Entra a https://app.netlify.com/drop
2. Arrastra la carpeta `business-case-ml` completa.
3. Netlify te da una URL pública. Puedes renombrar el sitio en *Site configuration → Change site name*.

**Opción con repositorio (actualizaciones automáticas)**
1. Sube esta carpeta a un repositorio de GitHub.
2. En Netlify: *Add new site → Import an existing project* y elige el repositorio.
3. No hace falta comando de build; `netlify.toml` ya indica publicar la raíz.

**Opción CLI**
```bash
npm install -g netlify-cli
netlify deploy --dir . --prod
```

## Cómo se guardan los datos
- Lo que ingresas se guarda solo en el navegador de cada usuario (localStorage). No hay servidor ni base de datos.
- **Guardar** descarga el escenario como `.json`; **Abrir** lo vuelve a cargar. Así se comparten o comparan escenarios.
- En Resultados puedes descargar un Excel (la librería SheetJS se carga desde cdnjs solo al exportar), copiar los datos o imprimir a PDF.

## Lógica de cálculo
- CAPEX = Σ horas × tarifa de construcción + infra de desarrollo (US$ × tipo de cambio). Se registra en el mes 0.
- OPEX mensual = servicios (horas × tarifa) + infra mensual (mín, máx o máx + margen, × tipo de cambio) + otros costos.
- Beneficio mensual = contactos × (conversión con modelo − sin modelo) × margen + ahorros. Durante el ramp-up se toma al 50%.
- Flujo: mes 0 = −CAPEX; meses de construcción = 0; meses de operación = beneficio − OPEX.
- ROI = (beneficio total − TCO) ÷ TCO, con TCO = CAPEX + OPEX × horizonte.
- VAN con tasa mensual equivalente a la tasa anual ingresada. Payback contado desde el go-live.
