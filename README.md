# simplex
Método simplex
import numpy as np 
def simplex(A, b, c, tipo="max"):
    """
    Resuelve un problema un problema de programacion
    lineal utilizando el metodo Simplex.
    Args:
        A(numpy.ndarray) : Matriz de 
        coeficientes de las restricciones.
        b(numpy.ndarray): Vector de 
        terminos independientes de las
        restricciones.
        c(numpy.ndarray): Vector de 
        coeficientes de la funcion objetivo.
        tipo(str):"max" para 
        maximimacion,"min" para minimizacion.
    Returns:
        tuple:(optimo, solucion)donde:
            - optimo es el valor optimo 
              de la funcion objetivo.
            - solucion es el vector de 
              valores de las variables en la solucion 
              optima.
    """
    # 1. Preparacion inicial 
    m, n = A.shape  # m = numero de restricciones, n = numero de variables 
    num_holgura = m # numero de variables de holgura necesarias
    matriz_simplex = np.zeros((m + 1, n + m + 1))  # crea la tabla simplex 
    # Llenar la tabla simplex 
    matriz_simplex[:m, :n] = A
    matriz_simplex[:m, n:n + m] = np.eye(m)
    # matriz identidad para las variables de holgadura 
    matriz_simplex[:m, -1] = b
    matriz_simplex[-1, :n] = -c if tipo == "max" else c 
    # funcion objetivo (negativa para maximizar)
    # 2. Interaccion del simplex 
    while True:
        # a. Encontrar la columna pivote 
        columna_pivote = np.argmin(matriz_simplex[-1, :-1]) 
        # la mas negativa en la fila objetivo 
        if matriz_simplex[-1, columna_pivote] >= 0:
            break # no hay mejoras posibles 
        # b. Encontrar la fila pivote 
        razones = matriz_simplex[:-1, -1] / matriz_simplex[:-1, columna_pivote]
        razones[razones <= 0] = np.inf
        #ignorar razones negativas o cero
        fila_pivote = np.argmin(razones)
        # c.Convertir el elemento pivote en 1
        pivote = matriz_simplex[fila_pivote, columna_pivote]
        matriz_simplex[fila_pivote, :] /= pivote
        # d.Hacer ceros en la columna pivote 
        for i in range(m + 1):
            if i != fila_pivote: 
                factor = matriz_simplex[i, columna_pivote]
                matriz_simplex[i, :] -= factor * matriz_simplex[fila_pivote, :] 
                # 3.Extraer la solucion
                optimo = matriz_simplex[-1, -1] if tipo == "max" else -matriz_simplex[-1, -1]
                solucion = np.zeros (n)
        for i in range(n):
                         columna = matriz_simplex[: , i]
                         unos = np.sum (columna == 1)
                         ceros = np.sum (columna == 0)
                         if unos == 1 and ceros == m:
                               indice =np.where(columna == 1)[0][0]
                               solucion[i] = matriz_simplex[indice, -1]
                               return optimo, solucion
                         

              
      
       
       
