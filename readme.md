# Traducao de linguagem natural para Nile com modelo compacto

Este repositorio reune os notebooks, os pacotes operacionais e os registros utilizados em uma pesquisa sobre a traducao de intencoes escritas em ingles para a linguagem formal Nile com o modelo compacto `Qwen/Qwen2.5-1.5B-Instruct`.

O experimento avalia traducao direta, Representacao Intermediaria, justificativas explicitas, selecao de demonstracoes e autocorrecao orientada pelo retorno do validador.

## Organizacao do repositorio

A estrutura original do experimento foi preservada. Nenhum notebook, pacote operacional, prompt, resposta ou resultado foi movido ou renomeado para a publicacao no GitHub.

```text
nl-to-nile-compact-llm/
├── README.md
├── THIRD_PARTY_NOTICES.md
├── CITATION.cff
├── requirements.txt
├── .gitignore
├── LICENSES/
│   └── LUMI_GPL-3.0.txt
├── Dados iniciais/
│   └── extraction_campi.json
├── F0/
│   ├── f0-preparacao-base.ipynb
│   └── f0_operacional.zip
├── F1/
│   ├── f1-preparacao-ir.ipynb
│   └── f1_operacional.zip
├── F2/
│   ├── f2-preparacao-r1-r2-r3.ipynb
│   ├── f2_operacional.zip
│   ├── f2_solicitacoes_professor/
│   ├── f2_respostas_professor/
│   └── links_do_deepseek.txt
├── F3/
│   ├── f3-modelo.ipynb
│   ├── f3a-estrategia-a.ipynb
│   ├── f3b-estrategia-b.ipynb
│   ├── f3c-estrategia-c.ipynb
│   ├── f3d-estrategia-d.ipynb
│   ├── f3e-estrategia-e.ipynb
│   ├── f3-ac-final.ipynb
│   └── pacotes operacionais da F3
└── F4/
    ├── f4-resultados.ipynb
    └── f4_consolidacao_resultados.zip
```

Os arquivos internos dos pacotes ZIP preservam os codigos auxiliares, os manifests, os resultados individuais, as tabelas, os graficos e as auditorias produzidos em cada fase.

## Ordem de execucao

Os notebooks devem ser consultados ou executados na seguinte ordem:

1. `F0/f0-preparacao-base.ipynb`
2. `F1/f1-preparacao-ir.ipynb`
3. `F2/f2-preparacao-r1-r2-r3.ipynb`
4. `F3/f3-modelo.ipynb`
5. `F3/f3a-estrategia-a.ipynb`
6. `F3/f3b-estrategia-b.ipynb`
7. `F3/f3c-estrategia-c.ipynb`
8. `F3/f3d-estrategia-d.ipynb`
9. `F3/f3e-estrategia-e.ipynb`
10. `F3/f3-ac-final.ipynb`
11. `F4/f4-resultados.ipynb`

## Fases do experimento

| Fase | Finalidade |
|---|---|
| F0 | Preparar o CAMPI, a gramatica, o validador, as metricas, os folds e os templates |
| F1 | Construir e validar as Representacoes Intermediarias de referencia |
| F2 | Gerar, validar, revisar seletivamente e consolidar R1, R2 e R3 |
| F3 | Preparar demonstracoes, executar as estrategias F3-A a F3-E e aplicar as autocorrecoes |
| F4 | Consolidar os estados experimentais, realizar auditorias e produzir as comparacoes finais |

## Estrategias

| Estrategia | Fluxo |
|---|---|
| F3-A | Linguagem natural para Nile |
| F3-B | Primeira autocorrecao das saidas invalidas da F3-A |
| F3-C | Linguagem natural para IR e, em seguida, IR para Nile |
| F3-D | Linguagem natural para Nile com demonstracoes enriquecidas por R3 |
| F3-E | Linguagem natural com R1 para IR e, em seguida, IR com R2 para Nile |
| Autocorrecao final | Segunda tentativa de correcao das saidas ainda invalidas de F3-B, F3-C, F3-D e F3-E |

A autocorrecao final e uma etapa complementar da F3 e nao constitui uma sexta estrategia.

## Configuracao experimental

- Conjunto de dados: CAMPI, com 50 pares entre entradas em ingles e referencias Nile
- Modelo aluno: `Qwen/Qwen2.5-1.5B-Instruct`
- Modelo professor utilizado somente na F2: DeepSeek-V4
- Particoes: cinco folds, com 40 exemplos de treinamento e 10 de teste em cada fold
- Valores de `k`: 1, 3 e 5
- Metodos de selecao: aleatorio deterministico, lexical por TF-IDF e semantico por embeddings
- Zero-shot: somente F3-A e F3-B
- Semente global: 42
- Registros principais da F3: 3.700
- Registros recompostos pela autocorrecao final: 1.850
- Registros consolidados nos estados da F4: 5.550

## Metricas

O experimento utiliza:

- taxa de sucesso do parser, PSR;
- correspondencia exata, EM;
- distancia de edicao de Levenshtein, ED;
- distancia de edicao normalizada, NED;
- aderencia estrutural binaria, SLA-S;
- aderencia estrutural por campos, SLA-F.

Os embeddings sao utilizados somente na selecao semantica das demonstracoes. Eles nao sao empregados como metrica de avaliacao.

## Convencao de nomenclatura

Os notebooks e as pastas das fases utilizam nomes em portugues para acompanhar a organizacao metodologica apresentada na dissertacao.

Os artefatos internos gerados pelos notebooks preservam os nomes tecnicos definidos durante a implementacao, predominantemente em ingles e no formato `snake_case`. Essa convencao foi mantida porque os nomes fazem parte dos caminhos utilizados pelos notebooks, dos manifests, dos inventarios e dos registros de integridade.

A coexistencia de nomes em portugues e em ingles nao altera o conteudo, a funcao ou a validade dos artefatos. Arquivos equivalentes produzidos por estrategias diferentes seguem o mesmo padrao de nomenclatura. A manutencao dos nomes originais tambem preserva a correspondencia entre o material executado no Kaggle e o material disponibilizado neste repositorio.

## Ambiente e instalacao

As inferencias da F3 foram executadas no Kaggle com uma GPU NVIDIA Tesla T4. O modelo aluno foi carregado em precisao `float16`, com amostragem desativada, limite de 256 novos tokens para Nile e 512 novos tokens para IR.

Para criar um ambiente local:

```bash
python -m venv .venv
```

No Windows PowerShell:

```bash
.venv\Scripts\Activate.ps1
```

No Linux ou macOS:

```bash
source .venv/bin/activate
```

Em seguida:

```bash
pip install -r requirements.txt
```

Os pesos dos modelos nao estao incluidos no repositorio.

## Reproducao

Cada notebook carrega os pacotes produzidos pelas fases anteriores. Para preservar a configuracao documentada, mantenha os nomes das pastas e dos arquivos e execute os notebooks na ordem indicada.

Os pacotes operacionais armazenam os artefatos necessarios para auditoria e continuidade entre as fases. Os manifests internos registram os nomes, os tamanhos e os hashes dos arquivos correspondentes.

A reproducao integral das inferencias depende de acesso aos modelos empregados, ao ambiente computacional e as respectivas bibliotecas. A execucao de uma nova geracao pode produzir diferencas caso a revisao remota de um modelo ou de uma biblioteca tenha sido alterada.

## Registros do modelo professor na F2

A F2 contem as solicitacoes e as respostas efetivamente utilizadas para produzir R1, R2 e R3:

```text
F2/f2_solicitacoes_professor/
F2/f2_respostas_professor/
```

O arquivo:

```text
F2/links_do_deepseek.txt
```

registra os links publicos das conversas utilizadas nos seis lotes iniciais e na revisao seletiva de R2. Esses links sao disponibilizados apenas como documentacao complementar, pois podem deixar de funcionar ou sofrer alteracoes na plataforma de origem.

A reproducao e a auditoria da F2 nao dependem dos links externos. Os prompts e as respostas efetivamente utilizados permanecem preservados localmente nas pastas da F2 e no pacote operacional correspondente.

## Origem do CAMPI

O CAMPI e um conjunto de dados de terceiros e nao foi criado pela autora deste repositorio. O arquivo utilizado foi obtido do repositorio do projeto LUMI, no qual aparece em:

```text
res/dataset/extraction_campi.json
```

Neste repositorio, a fonte foi preservada em:

```text
Dados iniciais/extraction_campi.json
```

O repositorio LUMI fornecido para a preparacao deste material contem a GNU General Public License versao 3 e declara `GPL-3.0-or-later` em seu arquivo `pyproject.toml`. Nao foi encontrada, nesse material, uma licenca separada especifica para o arquivo do CAMPI.

A atribuicao completa e uma copia da licenca do repositorio de origem estao em:

```text
THIRD_PARTY_NOTICES.md
LICENSES/LUMI_GPL-3.0.txt
```

## Responsabilidade pelo desenvolvimento

A organizacao dos notebooks e a estrutura de seus blocos foram elaboradas pela pesquisadora com base no protocolo experimental e na literatura consultada. Ferramentas de inteligencia artificial generativa foram consultadas pontualmente para esclarecer duvidas tecnicas e apoiar a revisao da implementacao.

As decisoes metodologicas, a adaptacao dos procedimentos, a execucao, a validacao e a interpretacao dos resultados permaneceram sob responsabilidade da pesquisadora.

## Observacao sobre proveniencia

Os pacotes atuais de F0, F1 e F2 nao sao integralmente identicos, byte a byte, as copias registradas como entradas no pacote comum da F3. Os principais artefatos de F0 e F1 e as justificativas consolidadas de F2 permanecem equivalentes aos recursos utilizados na execucao.

A diferenca remanescente envolve o arquivo historico das geracoes do professor na F2. Essa ressalva nao altera os resultados preservados de F3 e F4, mas impede que o conjunto atual seja apresentado como uma cadeia binaria integralmente fechada de F0 a F4.

## Citacao

Os metadados para citacao deste repositorio estao em `CITATION.cff`.

O conjunto CAMPI e o trabalho LUMI devem ser citados separadamente, conforme indicado em `THIRD_PARTY_NOTICES.md`.

## Licenca

Nenhuma licenca geral foi atribuida ao codigo autoral deste repositorio nesta etapa.

A licenca armazenada em `LICENSES/LUMI_GPL-3.0.txt` documenta a licenca declarada pelo repositorio LUMI do qual o CAMPI foi obtido. Ela nao e apresentada como licenca automatica de todo o codigo desenvolvido para este experimento.

Os pesos dos modelos nao estao incluidos e permanecem sujeitos as licencas e aos termos de suas fontes originais.
