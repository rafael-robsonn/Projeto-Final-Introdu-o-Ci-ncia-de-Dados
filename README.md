# Infrações de Trânsito no Distrito Federal (2018–2025)

Projeto Final da disciplina **Introdução à Ciência de Dados** — CEUB.
Análise exploratória (EDA) do dado bruto à apresentação de resultados, usando exclusivamente dados
do **Portal de Dados Abertos do Distrito Federal**.

## Integrantes

| Nome | Email |
|------|-------|
| Paulo Henrique Pereira Couto Cabral Filho | paulo.hf@sempreceub.com |
| Arthur Novais Vieira | arthur.novais@sempre.ceub.com |
| Rafael Robson Nunes de Araújo | rafael.robson@sempreceub.com |

## Pergunta orientadora

> **Como o número de infrações de trânsito no DF evoluiu ao longo dos anos (2018–2025)?**

Perguntas secundárias:
1. Quais são os tipos de infração mais frequentes no DF?
2. Quais meses concentram mais infrações registradas?
3. Como as infrações se distribuem por gravidade, tipo de veículo e hora do dia?

## Conjunto de dados

- **Nome:** Infrações de Trânsito (Autos de Infração — DER-DF)
- **Link:** https://www.dados.df.gov.br/dataset/infracoes-transito
- **Formato:** 83 arquivos CSV (um por mês), de julho/2018 a abril/2026
- **Volume:** ~6,5 milhões de registros brutos

Cada linha é um auto de infração, com tipo da infração, descrição, tipo de veículo, data, hora,
gravidade e dados de localização (estes últimos majoritariamente vazios e não usados na análise).

## Principais conclusões

1. **As infrações cresceram até 2022 e vêm caindo desde então** — pico de ~1,14 milhão em 2022 e
   retração consistente até 2025. Os anos de 2018–2019 têm cobertura parcial do registro, então o
   "crescimento inicial" é, em parte, efeito de cobertura, não só de comportamento.
2. **O dataset é dominado por excesso de velocidade captado por radar** — "velocidade até 20% acima
   do limite" sozinha supera todas as outras infrações somadas, e ~61% dos autos são de gravidade
   Média. O dado reflete sobretudo fiscalização eletrônica.
3. **Há padrões temporais e de perfil bem definidos** — concentração no 2º semestre (pico em
   novembro), no período diurno (pico 16–17h) e em automóveis. São padrões de *registro*, ligados ao
   fluxo de veículos e à presença de radares.

## Estrutura do repositório

```
.
├── README.md                          # este arquivo
├── notebook/
│   ├── analise.ipynb                  # análise completa (limpa, pronta para rodar)
│   └── analise_executada.ipynb        # mesma análise com as saídas já executadas
└── apresentacao/
    ├── slides.pdf                     # apresentação exportada em PDF (entregável oficial)
    └── slides.pptx                    # fonte editável dos slides
```

## Como rodar o notebook

1. Baixe o dataset do portal e **descompacte os 83 arquivos `.csv`** numa pasta chamada `dados/`,
   na raiz do projeto (ou ajuste a variável `PASTA_DADOS` na primeira célula de código).
2. Instale as dependências:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
3. Abra e execute do início ao fim:
   ```bash
   jupyter notebook notebook/analise.ipynb
   ```
   Ou rode no **Google Colab** (basta subir os CSVs e apontar `PASTA_DADOS`).

### Nota de engenharia

O dataset bruto tem ~6,5 milhões de linhas em 83 arquivos com **5 formatos diferentes** (separador
`;` e `,`, aspas no header, encoding UTF-8 e ISO-8859-1, de 6 a 13 colunas). O notebook trata tudo
isso com um leitor robusto e usa **agregação incremental** — processa um arquivo por vez e acumula
apenas as contagens que alimentam os gráficos. O pico de memória fica em torno de **300 MB**, então
roda em máquinas modestas e no Colab gratuito, sem estourar a RAM.

## Decisões de limpeza

| Problema encontrado | Decisão |
|---|---|
| 5 formatos de arquivo (separador, aspas, encoding, nº de colunas) | Leitor que detecta encoding e separador arquivo a arquivo e padroniza os nomes das colunas |
| Datas em texto `DD/MM/AAAA`; ~53 mil inválidas | Conversão para `datetime`; linhas com data inválida removidas |
| Mojibake de encoding (`MÃ©dia` → `Média`) | Re-encode `latin1 → utf-8` nas colunas categóricas |
| Categorias inconsistentes em `tipo_veiculo` | Normalização para rótulos canônicos |
| Ano de 2026 incompleto (só até abril) | Recorte da análise em 2018–2025 |

## Ferramentas

Python · pandas · numpy · matplotlib · seaborn · Jupyter
