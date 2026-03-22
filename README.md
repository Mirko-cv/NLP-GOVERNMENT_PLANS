# NLP-GOVERNMENT_PLANS
##   ANÁLISIS NLP DE PLANES DE GOBIERNO — MINERÍA EN PERÚ

OBJETIVO
--------
Identificar y comparar la orientación temática de los planes
de gobierno de cuatro partidos políticos peruanos respecto
al problema público de la minería ilegal, usando técnicas
de Procesamiento de Lenguaje Natural (NLP).

DATOS
-----
Se analizaron 97 oraciones extraídas de 4 planes de gobierno:
  • Plan Fuerza Popular        (30 oraciones)
  • Plan Alianza Para el Progreso (43 oraciones)
  • Plan País Para Todos       (19 oraciones)
  • Plan Renovación Popular    ( 5 oraciones)

METODOLOGÍA
-----------
1. PREPROCESAMIENTO
   Se limpiaron y normalizaron los textos: eliminación de
   stopwords, puntuación y tokens cortos. Se aplicó
   lematización con spaCy (es_core_news_sm) para reducir
   cada palabra a su forma base.

2. LDA (Latent Dirichlet Allocation)
   Se entrenó un modelo LDA con 4 topics sobre el corpus
   lematizado. El modelo identificó los siguientes temas
   latentes (coherencia c_v = 0.44):

     Topic 0 → Empleo y formalización minera
     Topic 1 → Pequeña minería e innovación productiva
     Topic 2 → Minería ilegal y zonas críticas
     Topic 3 → Infraestructura y conflictos locales

   Cada oración fue asignada al topic de mayor probabilidad.

3. ZERO-SHOT CLASSIFICATION
   Usando el modelo facebook/bart-large-mnli, cada oración
   fue clasificada en 12 etiquetas temáticas predefinidas
   (derivadas de los topics LDA y del contexto actual de
   minería ilegal en Perú) sin necesidad de datos etiquetados.

4. TF-IDF
   Se calculó la relevancia léxica de palabras y bigramas
   para cada plan de gobierno, permitiendo identificar el
   vocabulario distintivo de cada propuesta.

PRINCIPALES HALLAZGOS
---------------------
  • El topic dominante en el corpus es "Minería ilegal y
    zonas críticas" (Topic 2), presente en todos los planes.
  • Alianza Para el Progreso concentra el mayor volumen de
    propuestas, especialmente en formalización e infraestructura.
  • Renovación Popular, con solo 5 oraciones, muestra una
    propuesta poco desarrollada en materia minera.
  • El vocabulario TF-IDF revela que cada plan tiene un énfasis
    léxico distinto: FP enfatiza proyectos,
    APP enfatiza formalización y empleo, PPT enfatiza formalización y RP enfatiza medio ambiente.

PRINCIPALES LIMITANTES
-----------------------
Este tipo de análisis es poderoso, pero tiene restricciones importantes que impiden tomarlo como verdad absoluta:
1. El corpus es pequeño
97 oraciones distribuidas en 4 planes es un volumen muy reducido para NLP. Los modelos estadísticos como LDA necesitan grandes cantidades de texto para encontrar patrones robustos. Una coherencia de 0.44 (por debajo del umbral aceptable de 0.50) nos indica precisamente eso: los topics encontrados son orientativos, no definitivos.
2. Los planes de gobierno no son el discurso real
Un plan de gobierno es un documento político-técnico redactado para cumplir requisitos electorales. No necesariamente refleja lo que un partido hará en el poder, ni captura el discurso real de sus candidatos en campaña. Analizar el texto no equivale a analizar la intención política.
3. LDA no entiende el significado, solo la co-ocurrencia
LDA agrupa palabras que aparecen juntas, no palabras que significan lo mismo. Dos oraciones con el mismo significado pero vocabulario distinto pueden caer en topics diferentes. El modelo no comprende contexto, ironía, negaciones ni matices.
4. Zero-Shot tiene sesgo de idioma
El modelo facebook/bart-large-mnli fue entrenado predominantemente en inglés. Al aplicarlo sobre texto en español, su capacidad de inferencia semántica se reduce. Las etiquetas en español pueden no ser procesadas con la misma precisión que en inglés, lo que introduce ruido en la clasificación.
5. Las etiquetas temáticas las define el investigador
En Zero-Shot, tú decides qué categorías existen. Si una etiqueta importante no está en la lista, el modelo la ignorará por completo y asignará la oración a la categoría más cercana disponible, aunque no encaje bien. El análisis está condicionado por el conocimiento previo de quien lo diseña.
6. TF-IDF no captura semántica
Un TF-IDF alto indica que una palabra es distintiva de un plan, pero no dice nada sobre si esa propuesta es viable, coherente o prioritaria dentro del documento. Dos planes pueden usar la palabra "formalización" con significados operativos completamente distintos y el modelo no lo detectará.
7. El tamaño desigual de los planes distorsiona la comparación
Alianza Para el Progreso tiene 43 oraciones frente a las 5 de Renovación Popular. Comparar proporciones entre documentos tan distintos en volumen requiere cautela: un plan corto puede parecer poco comprometido simplemente porque tiene menos texto, no porque tenga menos propuestas.

¿Entonces para qué sirve?
Este análisis es válido como exploración inicial y sistemática del discurso político. Permite identificar patrones, comparar énfasis y generar hipótesis, pero debe complementarse con:
→ Análisis cualitativo del contenido
→ Revisión experta en política minera
→ Contraste con el discurso real de los candidatos
→ Corpus más amplio si se quieren conclusiones robustas
Los datos orientan. No reemplazan el juicio crítico.

HERRAMIENTAS UTILIZADAS
-----------------------
  Python · pandas · gensim · spaCy · transformers (HuggingFace)
  scikit-learn · matplotlib · seaborn · nltk


*NOTA. LOS TEXTOS DE LOS PLANES DE GOBIERNO FUERON EXTRAIDOS MANUALMENTE DE https://github.com/sorenriosdev/planes-gobierno-pe-2026/tree/master/data*
=============================================================
