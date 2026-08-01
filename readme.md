# Tradução de linguagem natural para Nile com modelo compacto

Este repositório reúne os notebooks, os pacotes operacionais e os registros utilizados em uma pesquisa sobre a tradução de intenções escritas em inglês para a linguagem formal Nile com o modelo compacto `Qwen/Qwen2.5-1.5B-Instruct`.

O experimento avalia tradução direta, Representação Intermediária, justificativas explícitas, seleção de demonstrações e autocorreção orientada pelo retorno do validador.

## Organização do repositório

A estrutura original do experimento foi preservada. Nenhum notebook, pacote operacional, prompt, resposta ou resultado foi movido ou renomeado para a publicação no GitHub.

```text
traducao-nl-nile-modelo-compacto/
├── readme.md
├── third_party_notices.md
├── citation.cff
├── requirements.txt
├── repository_inventory.json
├── .gitignore
├── licenses/
│   └── LUMI_GPL-3.0.txt
├── dados_iniciais/
│   └── extraction_campi.json
├── f0/
│   ├── f0-preparacao-base.ipynb
│   └── f0_operacional.zip
├── f1/
│   ├── f1-preparacao-ir.ipynb
│   └── f1_operacional.zip
├── f2/
│   ├── f2-preparacao-r1-r2-r3.ipynb
│   ├── f2_operacional.zip
│   ├── f2_solicitacoes_professor/
│   ├── f2_respostas_professor/
│   └── links_do_deepseek.txt
├── f3/
│   ├── f3-modelo.ipynb
│   ├── f3a-estrategia-a.ipynb
│   ├── f3b-estrategia-b.ipynb
│   ├── f3c-estrategia-c.ipynb
│   ├── f3d-estrategia-d.ipynb
│   ├── f3e-estrategia-e.ipynb
│   ├── f3-ac-final.ipynb
│   └── pacotes operacionais da F3
└── f4/
    ├── f4-resultados.ipynb
    └── f4_consolidacao_resultados.zip
```

Os arquivos internos dos pacotes ZIP preservam os códigos auxiliares, os manifestos, os resultados individuais, as tabelas, os gráficos e as auditorias produzidos em cada fase.

## Ordem de execução

Os notebooks devem ser consultados ou executados na seguinte ordem:

1. `f0/f0-preparacao-base.ipynb`
2. `f1/f1-preparacao-ir.ipynb`
3. `f2/f2-preparacao-r1-r2-r3.ipynb`
4. `f3/f3-modelo.ipynb`
5. `f3/f3a-estrategia-a.ipynb`
6. `f3/f3b-estrategia-b.ipynb`
7. `f3/f3c-estrategia-c.ipynb`
8. `f3/f3d-estrategia-d.ipynb`
9. `f3/f3e-estrategia-e.ipynb`
10. `f3/f3-ac-final.ipynb`
11. `f4/f4-resultados.ipynb`

## Fases do experimento

| Fase | Finalidade |
|---|---|
| F0 | Preparar o CAMPI, a gramática, o validador, as métricas, os folds e os templates |
| F1 | Construir e validar as Representações Intermediárias de referência |
| F2 | Gerar, validar, revisar seletivamente e consolidar R1, R2 e R3 |
| F3 | Preparar demonstrações, executar as estratégias F3-A a F3-E e aplicar as autocorreções |
| F4 | Consolidar os estados experimentais, realizar auditorias e produzir as comparações finais |

## Estratégias

| Estratégia | Fluxo |
|---|---|
| F3-A | Linguagem natural para Nile |
| F3-B | Primeira autocorreção das saídas inválidas da F3-A |
| F3-C | Linguagem natural para IR e, em seguida, IR para Nile |
| F3-D | Linguagem natural para Nile com demonstrações enriquecidas por R3 |
| F3-E | Linguagem natural com R1 para IR e, em seguida, IR com R2 para Nile |
| Autocorreção final | Segunda tentativa de correção das saídas ainda inválidas de F3-B, F3-C, F3-D e F3-E |

A autocorreção final é uma etapa complementar da F3 e não constitui uma sexta estratégia.

## Configuração experimental

- Conjunto de dados: CAMPI, com 50 pares entre entradas em inglês e referências Nile
- Modelo aluno: `Qwen/Qwen2.5-1.5B-Instruct`
- Modelo professor utilizado somente na F2: DeepSeek-V4
- Partições: cinco folds, com 40 exemplos de treinamento e 10 de teste em cada fold
- Valores de `k`: 1, 3 e 5
- Métodos de seleção: aleatório determinístico, lexical por TF-IDF e semântico por embeddings
- Zero-shot: somente F3-A e F3-B
- Semente global: 42
- Registros principais da F3: 3.700
- Registros recompostos pela autocorreção final: 1.850
- Registros consolidados nos estados da F4: 5.550

## Métricas

O experimento utiliza:

- taxa de sucesso do parser, PSR;
- correspondência exata, EM;
- distância de edição de Levenshtein, ED;
- distância de edição normalizada, NED;
- aderência estrutural binária, SLA-S;
- aderência estrutural por campos, SLA-F.

Os embeddings são utilizados somente na seleção semântica das demonstrações. Eles não são empregados como métrica de avaliação.

## Convenção de nomenclatura

Os notebooks e as pastas das fases utilizam nomes em português para acompanhar a organização metodológica apresentada na dissertação.

Os artefatos internos gerados pelos notebooks preservam os nomes técnicos definidos durante a implementação, predominantemente em inglês e no formato `snake_case`. Essa convenção foi mantida porque os nomes fazem parte dos caminhos utilizados pelos notebooks, dos manifestos, dos inventários e dos registros de integridade.

A coexistência de nomes em português e em inglês não altera o conteúdo, a função ou a validade dos artefatos. Arquivos equivalentes produzidos por estratégias diferentes seguem o mesmo padrão de nomenclatura. A manutenção dos nomes originais também preserva a correspondência entre o material executado no Kaggle e o material disponibilizado neste repositório.

## Ambiente e instalação

As inferências da F3 foram executadas no Kaggle com uma GPU NVIDIA Tesla T4. O modelo aluno foi carregado em precisão `float16`, com amostragem desativada, limite de 256 novos tokens para Nile e 512 novos tokens para IR.

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

Os pesos dos modelos não estão incluídos no repositório.

## Reprodução

Cada notebook carrega os pacotes produzidos pelas fases anteriores. Para preservar a configuração documentada, mantenha os nomes das pastas e dos arquivos e execute os notebooks na ordem indicada.

Os pacotes operacionais armazenam os artefatos necessários para auditoria e continuidade entre as fases. Os manifestos internos registram os nomes, os tamanhos e os hashes dos arquivos correspondentes.

A reprodução integral das inferências depende do acesso aos modelos empregados, ao ambiente computacional e às respectivas bibliotecas. Uma nova execução pode produzir diferenças caso a revisão remota de um modelo ou de uma biblioteca tenha sido alterada.

## Registros do modelo professor na F2

A F2 contém as solicitações e as respostas efetivamente utilizadas para produzir R1, R2 e R3:

```text
f2/f2_solicitacoes_professor/
f2/f2_respostas_professor/
```

O arquivo:

```text
f2/links_do_deepseek.txt
```

registra os links públicos das conversas utilizadas nos seis lotes iniciais e na revisão seletiva de R2. Esses links são disponibilizados apenas como documentação complementar, pois podem deixar de funcionar ou sofrer alterações na plataforma de origem.

A reprodução e a auditoria da F2 não dependem dos links externos. Os prompts e as respostas efetivamente utilizados permanecem preservados localmente nas pastas da F2 e no pacote operacional correspondente.

## Origem do CAMPI

O CAMPI é um conjunto de dados de terceiros e não foi criado pela autora deste repositório. O arquivo utilizado foi obtido do repositório do projeto LUMI, no qual aparece em:

```text
res/dataset/extraction_campi.json
```

Neste repositório, a fonte foi preservada em:

```text
dados_iniciais/extraction_campi.json
```

O repositório LUMI fornecido para a preparação deste material contém a GNU General Public License versão 3 e declara `GPL-3.0-or-later` em seu arquivo `pyproject.toml`. Não foi encontrada, nesse material, uma licença separada específica para o arquivo do CAMPI.

A atribuição completa e uma cópia da licença do repositório de origem estão em:

```text
third_party_notices.md
licenses/LUMI_GPL-3.0.txt
```

## Responsabilidade pelo desenvolvimento

A organização dos notebooks e a estrutura de seus blocos foram elaboradas pela pesquisadora com base no protocolo experimental e na literatura consultada. Ferramentas de inteligência artificial generativa foram consultadas pontualmente para esclarecer dúvidas técnicas e apoiar a revisão da implementação.

As decisões metodológicas, a adaptação dos procedimentos, a execução, a validação e a interpretação dos resultados permaneceram sob responsabilidade da pesquisadora.

## Proveniência e integridade da cadeia experimental

Os pacotes operacionais de F0, F1 e F2 disponibilizados no repositório preservam os mesmos artefatos internos registrados como entradas da F3. A verificação foi realizada por meio dos manifestos e dos hashes SHA-256 dos arquivos declarados, confirmando a correspondência de nomes, tamanhos e conteúdos.

A continuidade entre os estados da F3 e a consolidação realizada na F4 também permanece preservada pelos manifestos e pelos arquivos de auditoria dos pacotes operacionais.

O hash do arquivo ZIP externo pode ser alterado quando o pacote é recomposto, pois o formato ZIP armazena metadados próprios, como datas, ordem dos arquivos e parâmetros de compactação. Essa alteração não representa diferença nos artefatos experimentais. A verificação de integridade utiliza os manifestos e os hashes dos arquivos internos, que permanecem estáveis.

Dessa forma, a cadeia experimental de F0 a F4 encontra-se preservada e auditável por meio dos artefatos, manifestos e registros de integridade disponibilizados no repositório.

## Citação

Os metadados para citação deste repositório estão em `citation.cff`.

O conjunto CAMPI e o trabalho LUMI devem ser citados separadamente, conforme indicado em `third_party_notices.md`.

## Licença

Nenhuma licença geral foi atribuída ao código autoral deste repositório nesta etapa.

A licença armazenada em `licenses/LUMI_GPL-3.0.txt` documenta a licença declarada pelo repositório LUMI do qual o CAMPI foi obtido. Ela não é apresentada como licença automática de todo o código desenvolvido para este experimento.

Os pesos dos modelos não estão incluídos e permanecem sujeitos às licenças e aos termos de suas fontes originais.
