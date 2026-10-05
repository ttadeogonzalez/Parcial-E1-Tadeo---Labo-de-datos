# Diccionario — E1 Ingresos laborales

Fuente: INDEC EPH 4T2025. Una fila es una persona ocupada de 16–70 años, con ingreso principal positivo y horas semanales entre 1 y 168. Objetivo: logaritmo natural de P21, sin imputación. Recorte definitivo: 14.977 personas; 10.450 entrenamiento, 2.255 validación, 2.272 prueba. Viviendas: 6.899 / 1.478 / 1.479. No se comparten viviendas.

## 1.2 Diccionario y curación entregada

| Campo | Significado | Uso autorizado |
|---|---|---|
| `id_persona` | Identificador creado para el paquete | Auditoría/errores; no predictor |
| `grupo_vivienda` | Recodificación de CODUSU | Separación; no predictor |
| `edad_anios` | Edad observada en años | Auditoría y EDA |
| `sexo_registrado` | Varón/mujer según el registro de la encuesta | EDA y límites; no predictor |
| `nivel_educativo` | Nivel máximo agrupado | EDA; su aproximación numérica entra al modelo |
| `anios_educacion` | Años aproximados por la convención anterior | Predictor base |
| `experiencia_potencial` | Aproximación de experiencia en años | Predictor base |
| `experiencia_cuadrado` | Cuadrado de la experiencia potencial | Predictor base |
| `horas_semana` | Horas de la ocupación principal en semana de referencia | Menú laboral, numérica |
| `sector` | Estatal, privado, otro o no informado | Menú laboral, categórica |
| `rama_agrupada` | Agrupación pedagógica de actividad CAES | Menú laboral, categórica |
| `categoria_ocupacional` | Patrón, cuenta propia, asalariado u otra categoría | EDA; no predictor en este contrato |
| `region` | Región de los aglomerados | EDA y errores; no predictor |
| `ingreso_principal_ars` | P21 original positivo, pesos nominales | EDA; nunca predictor |
| `log_ingreso` | Logaritmo natural del ingreso | Objetivo |



## Recodificaciones y convenciones

- `id_persona`: ID secuencial del paquete. `grupo_vivienda`: correspondencia uno a uno con CODUSU; nunca predictor. La trazabilidad original queda sólo en material docente.
- `edad_anios`: CH06; `sexo_registrado`: CH04 (1 varón, 2 mujer). Son códigos del registro, no una medición exhaustiva de identidad de género.
- `nivel_educativo`: NIVEL_ED. 1 primaria incompleta, 2 primaria completa, 3 secundaria incompleta, 4 secundaria completa, 5 superior incompleta, 6 superior completa, 7 sin instrucción.
- `anios_educacion`: convención 1→3, 2→7, 3→10, 4→12, 5→14, 6→17, 7→0. No son años cursados observados. Experiencia potencial: máximo entre edad menos educación menos seis y cero. Cuadrado: potencia dos de esa aproximación.
- `horas_semana`: PP3E_TOT. Horas trabajadas en la ocupación principal durante la semana de referencia. Se excluyeron cuatro personas con ingreso positivo y 999 (no respuesta), además de los otros casos sin horas válidas. No se interpreta 999 como número de horas.
- `sector`: PP04A, 1 estatal, 2 privado, 3 otro; restante no informado.
- `rama_agrupada`: divisiones CAES obtenidas de la parte entera de PP04B_COD/100. Agrupación pedagógica: 01–03 agro/silvicultura/pesca; 05–09 minería; 10–33 industria; 35–39 electricidad/agua/residuos; 40 construcción; 45 y 48 comercio/reparación; 49–53 transporte/almacenamiento; 55–56 alojamiento/comidas; 58–63 información/comunicación; 64–66 finanzas; 68–82 servicios profesionales/empresariales (incluye inmobiliarios); 84 administración pública; 85 enseñanza; 86–88 salud/servicios sociales; 90–96 otros servicios; 97–98 hogares empleadores; 99 organismos extraterritoriales, salvo código 9999 que se conserva como no informado. Códigos fuera de estos rangos se etiquetan código no interpretado. No se inventa la actividad de esos casos. Esta agrupación no es una clasificación oficial adicional.
- `categoria_ocupacional`: CAT_OCUP 1 patrón, 2 cuenta propia, 3 asalariado, 4 familiar sin remuneración, 9 no informado.
- `region`: REGION 1 Gran Buenos Aires, 40 Noroeste, 41 Noreste, 42 Cuyo, 43 Pampeana, 44 Patagonia. No se distribuye aglomerado; en la fuente 31 es Ushuaia–Río Grande y 91 Rawson–Trelew.
- `ingreso_principal_ars`: P21 en pesos nominales, ingreso habitual de ocupación principal; -9 es no respuesta y 0 ingreso cero en la tabla previa al filtro. `log_ingreso`: logaritmo natural de ese monto positivo. Ninguno puede incorporarse a X.

## Selección y ponderación

La tabla auxiliar `no_respuesta_ingreso.csv` agrega por región a los 19.368 ocupados de 16–70 años antes de filtrar: ingreso positivo, ingreso cero y no respuesta (-9). No se utiliza para modelar. Las particiones se crean a partir de viviendas y se guardan en CSV separados. Los porcentajes son aproximados por filas. No se usan ponderadores: no estimar resultados poblacionales ni imputar no respuestas a partir de la media.
