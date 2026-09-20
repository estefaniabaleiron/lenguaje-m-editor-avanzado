let
    // Paso 1: Fuente de datos original (Tu código exacto)
    Source = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("XZBBasMwEEWvMmgdB0m2WndpJ4GWNBAaly5MFoqihcCWjGxBr5Mz9Ai+WEcOhai7mQeP/2faljCyIvAuh8kNcPQOmAAkG9cPYZLKzD8WV8YpXVOKE6e8yCjLqCDnVUs4ooMLo4Y3K7v51l+8UQ6hVEqPzhs3Rqn8J5eLnMfoRqtOXh0ctJpv9i4fPz53dYXDi0hFxhexWFKtmZyHYh/7qrRvIZK6PKP5IoqYWAXsGDrp9Qh1g6QKV+PuV6YWo4v1hOh02sLue9Le4ouaGh5bsjzx8r/nPCP60hcle3jdxpzHn5QideJp518=", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [id_venta = _t, nombre_producto = _t, categoria = _t, precio = _t, fecha_venta = _t]),

    // Paso 2: Eliminar espacios en blanco al inicio y al final
    // Usamos 'Source' aquí porque así se llamó el paso anterior
    LimpiarEspacios = Table.TransformColumns(Source, {{"nombre_producto", Text.Trim, type text}}),

    // Paso 3: Estandarizar la columna categoria a Title Case
    EstandarizarCategoria = Table.TransformColumns(LimpiarEspacios, {{"categoria", Text.Proper, type text}}),

    // Paso 4: Filtrar y eliminar registros de prueba
    EliminarPruebas = Table.SelectRows(EstandarizarCategoria, each [categoria] <> "Prueba"),

    // Paso 5: Definir tipos de datos correctos
    // Notá que acá le decimos que 'precio' sea 'type number' y no Int64
    TiparColumnas = Table.TransformColumnTypes(EliminarPruebas,{
        {"id_venta", Int64.Type}, 
        {"nombre_producto", type text}, 
        {"categoria", type text}, 
        {"precio", type number}, 
        {"fecha_venta", type date}
    })
in
    TiparColumnas
