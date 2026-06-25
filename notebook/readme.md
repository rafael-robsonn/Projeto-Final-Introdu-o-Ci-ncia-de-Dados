# Notebook

Análise exploratória completa — do dado bruto à apresentação de resultados.

## Arquivos

| Arquivo | Descrição |
|---|---|
| `analise.ipynb` | Notebook limpo, pronto para rodar do zero |
| `analise_executada.ipynb` | Mesma análise com todas as saídas já executadas |

## Pré-requisitos

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

Python 3.9+ recomendado. Sem dependências além da stdlib e das quatro bibliotecas acima.

## Como rodar

1. Baixe os 83 arquivos `.csv` do portal e descompacte numa pasta chamada `dados/`
   na raiz do projeto (mesmo nível que `README.md`):

   ```
   .
   ├── dados/
   │   ├── Consolidação de dados de Autos de Infração do mês de Janeiro_2025.csv
   │   └── ... (83 arquivos no total)
   ├── notebook/
   │   └── analise.ipynb
   └── ...
   ```

2. Abra o notebook:

   ```bash
   jupyter notebook notebook/analise.ipynb
   ```

3. Execute *Kernel → Restart & Run All*.

### Rodando no Google Colab

1. Faça upload dos CSVs para o Drive ou diretamente na sessão Colab.
2. Ajuste a variável `PASTA_DADOS` na primeira célula de código para o caminho correto.
3. Execute tudo normalmente — o notebook roda em ~300 MB de RAM, dentro do limite gratuito.

## Seções do notebook

| Seção | Conteúdo |
|---|---|
| 0. Configuração | Imports, paleta de cores, constantes |
| 1. Entendimento dos dados | Dicionário de colunas, formatos encontrados, head() |
| 2. Completude e limpeza | % de nulos, decisões justificadas, estatísticas básicas |
| 3. Análise e visualização | 6 gráficos com interpretação abaixo de cada um |
| 4. Conclusões | 3 conclusões sustentadas por evidência, com ressalvas honestas |

## Nota de engenharia

O dataset tem **~6,5 milhões de linhas** em 83 arquivos com **5 formatos diferentes**
(separador `;` e `,`, aspas no header, encoding UTF-8 e ISO-8859-1, de 6 a 13 colunas).

O notebook resolve isso com um leitor robusto que detecta encoding e separador arquivo a
arquivo. Para manter o uso de memória baixo, usa **agregação incremental**: processa um
arquivo por vez, acumula as contagens e descarta as linhas. O pico de memória fica em
torno de **300 MB** — cabe em qualquer máquina e no Colab gratuito.

## Gráficos gerados

| # | Tipo | Pergunta respondida |
|---|---|---|
| 1 | Barras verticais | Evolução anual 2018–2025 (pergunta principal) |
| 2 | Barras horizontais | Top 8 tipos de infração mais frequentes |
| 3 | Linha com marcador | Sazonalidade: infrações por mês |
| 4 | Barras verticais | Distribuição por gravidade |
| 5 | Barras horizontais | Infrações por tipo de veículo |
| 6 | Histograma | Distribuição por hora do dia |
