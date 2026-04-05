# complejidad-algortmica
import time
import tracemalloc

# =========================================
# PARTE 1.2 — BÚSQUEDA BINARIA O(log n)
# =========================================

def busqueda_binaria(lista, objetivo):
    izquierda = 0
    derecha = len(lista) - 1
    comparaciones = 0

    while izquierda <= derecha:
        medio = (izquierda + derecha) // 2
        comparaciones += 1

        if lista[medio] == objetivo:
            return medio, comparaciones
        elif lista[medio] < objetivo:
            izquierda = medio + 1
        else:
            derecha = medio - 1

    return -1, comparaciones


def probar_binaria():
    print("\n=== Búsqueda Binaria ===")

    tamaños = [100, 1000, 10000]

    for n in tamaños:
        lista = list(range(n))

        # Mejor caso (centro)
        _, comp_mejor = busqueda_binaria(lista, lista[n // 2])

        # Peor caso (no existe)
        _, comp_peor = busqueda_binaria(lista, -1)

        print(f"\nTamaño n = {n}")
        print(f"Comparaciones (mejor caso): {comp_mejor}")
        print(f"Comparaciones (peor caso): {comp_peor}")

# =========================================
# PARTE 2.1 — BÚSQUEDA LINEAL O(n)
# =========================================

def busqueda_lineal(lista, objetivo):
    comparaciones = 0

    for i in range(len(lista)):
        comparaciones += 1
        if lista[i] == objetivo:
            return i, comparaciones

    return -1, comparaciones


def probar_lineal():
    print("\n=== Búsqueda Lineal ===")

    n = 10
    lista = list(range(n))

    # Mejor caso (inicio)
    _, comp_mejor = busqueda_lineal(lista, lista[0])

    # Caso promedio (mitad)
    _, comp_promedio = busqueda_lineal(lista, lista[n // 2])

    # Peor caso (no existe)
    _, comp_peor = busqueda_lineal(lista, -1)

    print(f"Mejor caso (O(1)): {comp_mejor} comparaciones")
    print(f"Caso promedio (O(n)): {comp_promedio} comparaciones")
    print(f"Peor caso (O(n)): {comp_peor} comparaciones")

# =========================================
# PARTE 3.1 — COMPARAR TIEMPOS
# =========================================

def medir_tiempos():
    print("\n=== Comparación Lineal vs Binaria ===")

    tamaños = [1000, 10000, 100000]

    print("\n n | Lineal (ms) | Binaria (ms)")
    print("--------------------------------")

    for n in tamaños:
        lista = list(range(n))
        objetivo = -1  # peor caso

        # Medir lineal
        inicio = time.perf_counter()
        for _ in range(100):
            busqueda_lineal(lista, objetivo)
        fin = time.perf_counter()
        tiempo_lineal = (fin - inicio) / 100 * 1000

        # Medir binaria
        inicio = time.perf_counter()
        for _ in range(100):
            busqueda_binaria(lista, objetivo)
        fin = time.perf_counter()
        tiempo_binaria = (fin - inicio) / 100 * 1000

        print(f"{n} | {tiempo_lineal:.5f} | {tiempo_binaria:.5f}")

# =========================================
# PARTE 3.2 — USO DE MEMORIA
# =========================================

def cuadrados_lista(n):
    return [i**2 for i in range(1, n+1)]


def cuadrados_generador(n):
    for i in range(1, n+1):
        yield i**2


def medir_memoria():
    print("\n=== Uso de Memoria ===")

    n = 100000

    # Lista
    tracemalloc.start()
    lista = cuadrados_lista(n)
    memoria_lista = tracemalloc.get_traced_memory()[1]
    tracemalloc.stop()

    # Generador
    tracemalloc.start()
    gen = cuadrados_generador(n)
    lista_gen = list(gen)
    memoria_gen = tracemalloc.get_traced_memory()[1]
    tracemalloc.stop()

    print(f"Memoria lista: {memoria_lista / 1024 / 1024:.2f} MB")
    print(f"Memoria generador: {memoria_gen / 1024 / 1024:.2f} MB")

# =========================================
# MENÚ PRINCIPAL
# =========================================

def main():
    while True:
        print("\n=== MENÚ ===")
        print("1. Probar búsqueda binaria")
        print("2. Probar búsqueda lineal")
        print("3. Comparar tiempos")
        print("4. Comparar memoria")
        print("5. Salir")

        op = input("Seleccione una opción: ")

        if op == "1":
            probar_binaria()
        elif op == "2":
            probar_lineal()
        elif op == "3":
            medir_tiempos()
        elif op == "4":
            medir_memoria()
        elif op == "5":
            break
        else:
            print("Opción inválida")

if __name__ == "__main__":
    main()




    Ejercicio 1.2 — Búsqueda binaria
Mejor caso: O(1) → el elemento está en el centro.
Peor caso: O(log n) → se divide la lista repetidamente.
Conclusión:
El número de comparaciones crece de forma logarítmica.
Por ejemplo:
n = 100 → ~7 comparaciones
n = 1000 → ~10 comparaciones
n = 10000 → ~14 comparaciones




Ejercicio 2.1 — Búsqueda lineal
Mejor caso: O(1) → elemento al inicio.
Promedio: O(n)
Peor caso: O(n) → elemento al final o no existe.



Ejercicio 3.1 — Comparación
Búsqueda lineal: O(n)
Búsqueda binaria: O(log n)


Ejercicio 3.2 — Memoria
Lista: O(n)
Generador: O(1)
