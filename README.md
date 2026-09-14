# PC.2 — Acesso a dados textuais no spaCy e ao corpora SUBTLEX-pt-BR

Disciplina de Processamento de Linguagem Natural — lista de exercícios da introdução.

## Conteúdo desta pasta

| Arquivo | Descrição |
|---|---|
| `PC2_spacy_subtlex.ipynb` | Notebook com a resolução completa do PC.2 (código-fonte) |
| `requirements.txt` | Dependências do projeto |
| `apresentacao_TC2_PC2.pptx` | Apresentação cobrindo o TC.2 (discussão) e o PC.2 (demonstração) |
| `README.md` | Este arquivo — procedimento de execução |

O corpus **não** está incluído: o notebook o baixa automaticamente do OSF na primeira execução
(≈ 2,7 MB) e mantém cache local em `SUBTLEX_PT-BR_CDAbove2_Alpha_Spellcheck.tsv`.

## Procedimento de execução

### 1. Criar e ativar um ambiente virtual

```bash
python3 -m venv .venv

# Linux / macOS
source .venv/bin/activate

# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

### 2. Instalar as dependências

```bash
python -m pip install -r requirements.txt
python -m spacy download pt_core_news_sm
```

Os dois comandos são necessários: o modelo de língua não está no `requirements.txt`
de propósito, porque instalá-lo por URL faz o pip abortar o arquivo inteiro caso o
download falhe.

Use `python -m pip` em vez de só `pip` — garante que é o pip do ambiente ativo.
Para conferir se ficou tudo certo:

```bash
python -m pip check
python -c "import spacy; print(spacy.__version__)"
```

### 3. Abrir o notebook

```bash
jupyter notebook PC2_spacy_subtlex.ipynb
```

Execute as células na ordem (`Cell → Run All`). O notebook é entregue sem saídas gravadas —
elas são geradas na sua máquina, com o corpus completo.

### 4. Se o download automático do corpus falhar

O notebook tenta duas URLs do OSF. Se ambas falharem (bloqueio de rede), baixe manualmente:

1. Acesse <https://osf.io/vb5yp/>
2. Baixe `SUBTLEX_PT-BR_CDAbove2_Alpha_Spellcheck.tsv`
3. Salve o arquivo nesta mesma pasta, com esse nome
4. Reexecute a célula — ela detecta o cache local e segue adiante

## O que o notebook faz

**Parte 1 — dados prontamente disponíveis no spaCy**
Frases de exemplo embutidas, lista de *stop words* do português, exceções de tokenização
(abreviaturas), metadados do modelo `pt_core_news_sm` (corpora de treino, licenças, rótulos,
acurácia), `spacy.explain`, vocabulário/lexemas e o pipeline completo aplicado a uma frase.

**Parte 2 — acesso ao corpora SUBTLEX-pt-BR**
Download com cache, carregamento em pandas, inspeção das colunas (`Word`, `FREQcount`, `CDcount`,
`Spellcheck`), cálculo das medidas derivadas (SUBTLWF, Lg10WF, CDpct, Zipf) e demonstração do uso
dos metadados.

**Parte 3 — aplicação a um problema simples de PLN**
Triagem lexical de chamados de *helpdesk*: o spaCy tokeniza, lematiza e classifica gramaticalmente;
o SUBTLEX-pt-BR informa quão comum cada forma é na língua falada. O sistema sinaliza termos fora do
vocabulário corrente (jargão ou erro de digitação) e calcula um índice de dificuldade lexical por
chamado. Como segunda aplicação, deriva uma lista de *stop words* a partir dos dados e a compara
com a lista embutida do spaCy.

## Ambiente testado

Python 3.11 · spaCy 3.8 · pandas 2.3 · modelo `pt_core_news_sm` 3.8.0

## Referências

- TANG, K. *A 61 Million Word Corpus of Brazilian Portuguese Film Subtitles as a Resource for
  Linguistic Research.* UCL Working Papers in Linguistics, v. 24, p. 208–214, 2012.
- SUBTLEX-PT-BR no OSF: <https://osf.io/vb5yp/> — licença CC BY-NC-ND 4.0
- Documentação do spaCy: <https://spacy.io/api>
- VAN HEUVEN, W. J. B. et al. *SUBTLEX-UK: A new and improved word frequency database for British
  English.* Quarterly Journal of Experimental Psychology, v. 67, n. 6, p. 1176–1190, 2014.
  (definição da escala Zipf)
