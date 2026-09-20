# Práctica: Lenguaje M en el Editor Avanzado

### 1. ¿Qué hace exactamente el bloque let...in en lenguaje M? ¿Por qué cada paso puede referenciar al anterior?
El bloque `let...in` define el flujo de la consulta. 
* En la sección `let`, se declaran variables (los "pasos" de transformación). 
* En la sección `in`, se especifica cuál de esas variables es el resultado final que se cargará en el modelo de Power BI.
Cada paso puede referenciar al anterior porque M evalúa las expresiones de forma funcional y secuencial. Cada paso almacena el estado de la tabla transformada; al citar el nombre del paso anterior como argumento de una nueva función, le estamos pasando la tabla con todas las transformaciones acumuladas hasta ese momento.

### 2. ¿Por qué M es Case Sensitive y qué consecuencia práctica tiene? Dá un ejemplo de un error que esto puede causar.
M es *Case Sensitive* (sensible a mayúsculas y minúsculas) lo que significa que distingue estrictamente cómo está escrita una palabra, tanto en la sintaxis de las funciones como en los datos mismos. La consecuencia práctica es que el código debe escribirse con total precisión, ya que todas las funciones nativas de M comienzan con mayúscula.
* **Ejemplo de error:** Si escribimos `table.selectrows` en lugar de `Table.SelectRows`, Power BI devolverá el error `Expression.Error: El nombre 'table.selectrows' no se reconoce`, impidiendo que el script se ejecute.

### 3. ¿Cuál es la diferencia entre usar Text.Trim y Text.Clean en M?
* **`Text.Trim`**: Elimina únicamente los espacios en blanco tradicionales que se encuentran al principio y al final de una cadena de texto (como en `" Laptop "`).
* **`Text.Clean`**: Elimina los caracteres de control no imprimibles (como saltos de línea, retornos de carro o tabulaciones) que suelen aparecer cuando se importan datos desde sistemas *legacy* o PDFs.

### 4. ¿Por qué filtraste los registros "PRUEBA" después de estandarizar la categoría y no antes?
Se filtró después para aprovechar que el paso de estandarización (`Text.Proper`) convierte cualquier variación del texto (como "PRUEBA", "prueba", "pRuEbA") a un formato único y predecible: "Prueba". 
Dado que M es *Case Sensitive*, si intentábamos filtrar antes, habríamos tenido que escribir una condición compleja que contemplara todas las combinaciones posibles de mayúsculas y minúsculas. Al estandarizar primero, logramos limpiar el dato con una única y simple regla booleana: `each [categoria] <> "Prueba"`.
