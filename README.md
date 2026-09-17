# Selection Sort × Tim Sort — análise empírica

Comparação experimental entre um algoritmo de ordenação **O(n²)** (Selection Sort) e o
**Tim Sort** (Insertion Sort + Merge Sort, a estratégia usada pelo `sorted()` do Python),
ambos implementados do zero, medidos em três cenários e plotados lado a lado.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat-square&logo=python&logoColor=white)

---

## O experimento

Para cada tamanho de entrada `N ∈ {1.000, 3.000, 5.000, 7.000, 10.000}`, os dois
algoritmos ordenam o **mesmo vetor** em três cenários:

| Cenário | Entrada | Por que importa |
| --- | --- | --- |
| **Aleatório** | valores sorteados, média de 10 repetições | caso médio |
| **Já ordenado** | `0, 1, 2, …, N` | melhor caso do Tim Sort — expõe se o algoritmo aproveita ordem pré-existente |
| **Inversamente ordenado** | `N, …, 2, 1` | pior caso — número máximo de trocas |

O caso aleatório é repetido **10 vezes** e a média é usada, para que uma variação
pontual da máquina não contamine o resultado. Os dois algoritmos recebem cópias do
mesmo vetor base (`v_base.copy()`), então a comparação é justa.

## Implementações

- **`selection_sort`** — a cada passo, varre o resto do vetor procurando o menor
  elemento e o troca de posição. Faz sempre `n(n-1)/2` comparações, **independente
  da entrada**: é O(n²) no melhor, no médio e no pior caso.
- **`tim_sort`** — divide o vetor em *runs* de 32 elementos ordenados por
  **Insertion Sort** (rápido em trechos pequenos e quase ordenados) e depois funde os
  runs aos pares com **Merge Sort**, dobrando o tamanho a cada rodada. Complexidade
  O(n log n).

## O que os resultados mostram

O Selection Sort gasta praticamente o mesmo tempo nos três cenários — a curva é a
mesma parábola, porque ele não tem como perceber que o vetor já está ordenado. O Tim
Sort cresce de forma quase linear e a distância entre os dois se abre rapidamente
conforme `N` aumenta: é a diferença entre O(n²) e O(n log n) ficando visível em
segundos de relógio, não apenas em notação assintótica.

## Como executar

```bash
pip install matplotlib pandas
python main.py
```

O script imprime a tabela de tempos em segundos e abre uma figura com três gráficos
— um por cenário.

> O Selection Sort é O(n²): aumentar muito `TAMANHOS` faz o tempo de execução
> explodir. Com `N = 10.000` o experimento completo já leva alguns minutos.

## Estrutura

```
SelectionSort-TimSort/
├── main.py     algoritmos, experimento, tabela e gráficos
└── README.md
```

---

Trabalho desenvolvido para a disciplina de análise de algoritmos, originalmente
executado no Google Colab.
