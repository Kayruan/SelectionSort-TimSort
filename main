import time
import random
import matplotlib.pyplot as plt
import pandas as pd # Para organizar a tabela final

# --- ALGORITMOS ---

def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

def insertion_sort(arr, left, right):
    for i in range(left + 1, right + 1):
        key = arr[i]
        j = i - 1
        while j >= left and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

def merge(arr, l, m, r):
    len1, len2 = m - l + 1, r - m
    left, right = arr[l:m+1], arr[m+1:r+1]
    i, j, k = 0, 0, l
    while i < len1 and j < len2:
        if left[i] <= right[j]:
            arr[k] = left[i]; i += 1
        else:
            arr[k] = right[j]; j += 1
        k += 1
    while i < len1: arr[k] = left[i]; i += 1; k += 1
    while j < len2: arr[k] = right[j]; j += 1; k += 1

def tim_sort(arr):
    n = len(arr)
    min_run = 32
    for i in range(0, n, min_run):
        insertion_sort(arr, i, min((i + min_run - 1), n - 1))
    size = min_run
    while size < n:
        for left in range(0, n, 2 * size):
            mid = min(n - 1, left + size - 1)
            right = min((left + 2 * size - 1), (n - 1))
            if mid < right:
                merge(arr, left, mid, right)
        size *= 2

# --- CONFIGURAÇÃO DO EXPERIMENTO ---

# Sugestão: Não aumente muito o N_MAX pois o Selection Sort é O(n²) e pode travar.
TAMANHOS = [1000, 3000, 5000, 7000, 10000]
REPETICOES_ALEATORIO = 10 
resultados = []

def medir_tempo(algoritmo, vetor):
    inicio = time.time()
    algoritmo(vetor)
    return time.time() - inicio

print("Iniciando testes... Isso pode levar alguns minutos.")

for n in TAMANHOS:
    # 1. CASO ALEATÓRIO (Média)
    tempos_sel_ale = []
    tempos_tim_ale = []
    for _ in range(REPETICOES_ALEATORIO):
        v_base = [random.randint(0, n) for _ in range(n)]
        tempos_sel_ale.append(medir_tempo(selection_sort, v_base.copy()))
        tempos_tim_ale.append(medir_tempo(tim_sort, v_base.copy()))
    
    m_sel_ale = sum(tempos_sel_ale) / REPETICOES_ALEATORIO
    m_tim_ale = sum(tempos_tim_ale) / REPETICOES_ALEATORIO

    # 2. CASO ORDENADO
    v_ord = list(range(n))
    t_sel_ord = medir_tempo(selection_sort, v_ord.copy())
    t_tim_ord = medir_tempo(tim_sort, v_ord.copy())

    # 3. CASO INVERSO
    v_inv = list(range(n, 0, -1))
    t_sel_inv = medir_tempo(selection_sort, v_inv.copy())
    t_tim_inv = medir_tempo(tim_sort, v_inv.copy())

    # Guardar dados para a tabela
    resultados.append({
        'N': n,
        'Sel_Aleat': m_sel_ale, 'Tim_Aleat': m_tim_ale,
        'Sel_Ord': t_sel_ord, 'Tim_Ord': t_tim_ord,
        'Sel_Inv': t_sel_inv, 'Tim_Inv': t_tim_inv
    })
    print(f"N={n} finalizado.")

# --- GERAÇÃO DE SAÍDAS ---

df = pd.DataFrame(resultados)
print("\n--- TABELA DE RESULTADOS (Segundos) ---")
pd.options.display.float_format = '{:.4f}'.format
print(df.to_string(index=False))

# --- GRÁFICOS ---
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

# Gráfico Aleatório
axes[0].plot(df['N'], df['Sel_Aleat'], 'r-o', label='Selection')
axes[0].plot(df['N'], df['Tim_Aleat'], 'g-s', label='Timsort')
axes[0].set_title('Vetores Aleatórios (Média)')
axes[0].legend(); axes[0].grid(True)

# Gráfico Ordenado
axes[1].plot(df['N'], df['Sel_Ord'], 'r-o', label='Selection')
axes[1].plot(df['N'], df['Tim_Ord'], 'g-s', label='Timsort')
axes[1].set_title('Vetores Já Ordenados')
axes[1].legend(); axes[1].grid(True)

# Gráfico Inverso
axes[2].plot(df['N'], df['Sel_Inv'], 'r-o', label='Selection')
axes[2].plot(df['N'], df['Tim_Inv'], 'g-s', label='Timsort')
axes[2].set_title('Vetores Inversamente Ordenados')
axes[2].legend(); axes[2].grid(True)

plt.tight_layout()
plt.show()
