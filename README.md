# Empleo en Argentina: análisis de la Encuesta Permanente de Hogares (3.er trimestre 2025)

Análisis de la **Encuesta Permanente de Hogares (EPH)** del INDEC con técnicas de limpieza de datos, clustering, clasificación y regresión. El objetivo es entender la situación laboral de la población: agrupar perfiles, predecir si un empleo es formal, informal o doméstico, y estimar los ingresos de las personas ocupadas.

## Qué hice

### 1. Limpieza de datos
- Me quedé con las personas que respondieron la encuesta individual (`H15 = 1`) y con un subconjunto de variables relevantes (región, sexo, edad, educación, estado laboral, búsqueda de empleo, características del trabajo, ingreso total `P47T`, etc.).
- Completé con 0 los faltantes estructurales (por ejemplo, datos de empleo en personas desocupadas), eliminé ingresos inválidos y respuestas "No sabe / No responde".
- Pasé las variables categóricas a *dummies* y simplifiqué algunas (estado civil, alfabetización) a binarias.

### 2. Clustering
- Estandarización + **PCA** para visualizar los datos en dos dimensiones.
- **K-Means** eligiendo K con el método del codo.
- Los clusters distinguen personas **ocupadas**, **jubiladas** y **en búsqueda de empleo**. Dentro de los ocupados, aparece una diferencia entre quienes trabajan por cuenta propia o desde su casa y el resto.

### 3. Clasificación: ¿empleo formal, informal o doméstico?
- **kNN** con 80/20 de entrenamiento y prueba, buscando el mejor *k* (k = 10).
- Un modelo reducido con solo tres variables (`EMPLEO`, `P47T`, `NIVEL_ED`) clasifica bien a cerca de 3 de cada 4 personas.

### 4. Regresión: predicción del ingreso (log)
- Comparé **regresión lineal**, **Ridge** (α elegido con GridSearchCV) y una **red neuronal MLP** en PyTorch, usando el RMSE en el conjunto de prueba.
- La regresión lineal explica alrededor del **47 %** de la variación de los ingresos (R² ≈ 0,47). Ridge da prácticamente lo mismo y el MLP, aun siendo más complejo, no la supera.
- Analicé el efecto de **sexo**, **nivel educativo** y **región** sobre el ingreso manteniendo el resto constante.

## Datos
`usu_individual_T325.txt`: base de microdatos individuales de la EPH, 3.er trimestre de 2025, publicada por el INDEC en
https://www.indec.gob.ar/indec/web/Institucional-Indec-BasesDeDatos

## Cómo correrlo
```bash
pip install -r requirements.txt
jupyter notebook "Proyecto Empleo Argentina.ipynb"
```

## Tecnologías
Python · pandas · NumPy · Matplotlib · Seaborn · scikit-learn · PyTorch

## Autor
Lázaro Reynoso
