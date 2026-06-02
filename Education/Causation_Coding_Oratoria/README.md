# Causation Coding - Formação em Oratória

Replicação didática do exemplo de **Causation Coding** apresentado por Saldaña
(2013). O estudo original analisa respostas abertas de adultos que participaram
de aulas, clubes e torneios de oratória durante o ensino médio. O objetivo é
identificar atribuições causais percebidas pelos próprios participantes.

Este case study mantém o desenho metodológico, mas utiliza respostas fictícias,
adaptadas e redigidas em português. Nenhuma resposta deve ser tratada como dado
empírico real.

## Estrutura

- `project.synp`: ponto de entrada do projeto.
- `template.synt`: contrato para fontes, itens, cadeias e ontologia.
- `annotations.syn`: oito participantes fictícios e dezesseis atribuições causais.
- `ontology.syno`: antecedentes, mediadores e resultados usados nas cadeias.
- `references.bib`: referência metodológica e registros fictícios das respostas.
- `exports/causation_coding_oratoria.json`: exportação JSON compilada.
- `analise_causal.ipynb`: notebook VSCode com resumo e rede causal agregada.

## Perguntas de pesquisa

Uma aplicação real pode usar perguntas abertas equivalentes a:

1. Olhando para trás, qual foi o maior desafio que você enfrentou ou superou nas
   atividades de oratória durante o ensino médio?
2. De que maneira sua participação em atividades de oratória no ensino médio
   afetou a pessoa adulta que você se tornou?

## Modelo analítico

Cada item registra uma citação, um memo e uma cadeia causal percebida no campo
reservado `chain`:

```text
antecedente -> relação -> mediador -> relação -> resultado
```

As categorias de resultado reproduzem a lógica agregadora do estudo:

- preparação profissional;
- habilidades de apresentação;
- habilidades de liderança;
- confiança;
- ganhos de competição;
- pertencimento social;
- efeitos pessoais.

## Compilar

Execute a partir do diretório do compilador:

```powershell
python -m synesis.cli check ..\case-studies\Education\Causation_Coding_Oratoria\project.synp
python -m synesis.cli compile ..\case-studies\Education\Causation_Coding_Oratoria\project.synp --json ..\case-studies\Education\Causation_Coding_Oratoria\exports\causation_coding_oratoria.json
```
