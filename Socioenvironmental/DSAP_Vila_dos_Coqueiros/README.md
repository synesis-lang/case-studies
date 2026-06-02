# DSAP - Vila dos Coqueiros

Case study fictício de Diagnóstico Socioambiental Participativo (DSAP), baseado no
exemplo da documentação Synesis. O projeto combina fontes técnicas e comunitárias,
codificação qualitativa, classificação SWOT e dimensões da sustentabilidade.

## Arquivos

- `project.synp`: ponto de entrada do projeto.
- `template.synt`: campos e regras estruturais do diagnóstico.
- `annotations.syn`: três fontes e seis itens codificados.
- `ontology.syno`: seis fatores e suas dimensões de sustentabilidade.
- `references.bib`: referências fictícias usadas pelas fontes.
- `exports/dsap_vila_dos_coqueiros.json`: exportação JSON para análise.
- `escala_sustentabilidade.ipynb`: notebook para uso no VSCode, com compilação
  em memória via `synesis.load`.

## Compilar

Execute a partir deste diretório:

```powershell
synesis check project.synp
synesis compile project.synp --json exports/dsap_vila_dos_coqueiros.json
```

## Escala de sustentabilidade

O notebook lê os arquivos do projeto com `synesis.load` e calcula a escala local
de 0 a 10 sem depender da exportação JSON intermediária:

```text
Escala = ((F + O) - (Fr + A)) / ((F + O) + (Fr + A)) * 5 + 5
```

Com os dados do exemplo, o resultado esperado é `5.0` (`Estagnação`).
