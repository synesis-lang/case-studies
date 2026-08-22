# Roteiro de teste manual — correções F1–F6 e G1–G3

Projeto montado para exercitar os defeitos corrigidos. Abra a pasta
`d:\GitHub\case-studies\Tests\f7_manual` no VS Code (extensão já instalada,
v0.11.0 com as correções).

> Se o LSP não iniciar, confira `synesisExplorer.lsp.pythonPath`. O
> `synesis-lsp` está instalado em modo **editável** apontando para
> `D:\GitHub\synesis-lsp`, então já roda com as correções.

---

## 1. Sidebar navega para a linha certa (F1)

1. Abra o painel **Synesis → References**. Devem aparecer `silva2020` e `souza2019`.
2. Expanda uma referência e clique na ocorrência (`Ln N`) → o cursor deve cair na
   linha `SOURCE @...`.
3. **O teste real do defeito:** insira 5 linhas de comentário no **topo** de
   `teste.syn` (`# a`, `# b`, …) e **salve**. Clique de novo na mesma ocorrência.
   - ✅ Esperado: o cursor cai em `SOURCE @...`, agora deslocado.
   - ❌ Antes: caía no meio do bloco anterior, porque a árvore mantinha a linha
     antiga (as contagens não mudaram, e o refresh era abortado).
4. Desfaça as 5 linhas.

## 2. Comentário-cabeçalho abre o abstract certo (F2 + F3)

**Cenário A — comentário no topo do arquivo**

1. Ponha o cursor na **linha 1, 2 ou 3** (o bloco `# CENARIO A ...`).
2. Rode **Synesis: Show Abstract**.
   - ✅ Esperado: abre o abstract de **@souza2019** (o próximo bloco).
   - ❌ Antes: `No reference found. Position cursor inside a SOURCE or ITEM block.`

**Cenário B — comentário que rotula a fonte seguinte**

1. Ponha o cursor na **linha 16 ou 17** (`# CENARIO B ...`), logo acima de
   `SOURCE @silva2020`.
2. Rode **Show Abstract**.
   - ✅ Esperado: abre **@silva2020** — a fonte que o comentário rotula.
   - ❌ Antes: abria **@souza2019**, a fonte anterior. Este é o relato
     "renderiza informação de outro source".

**Contraprova (não sobre-corrigir):** com o cursor numa linha *dentro* de um
bloco, o abstract deve ser o do próprio bloco — sem mudança de comportamento.

## 3. Campos do bloco SOURCE aparecem (F4)

Com o abstract de **@silva2020** aberto:

- ✅ Esperado: seção **Source** entre o cabeçalho bibliográfico e o abstract,
  exibindo `description` ("Estudo sobre aceitacao social da transicao") e
  `method` ("Survey com 400 respondentes").
- ❌ Antes: nada — o cabeçalho vinha só do `.bib` e esses campos não tinham como
  chegar à tela.

## 4. Trechos localizados apesar de acento e hifenização (F5)

Ainda no abstract de **@silva2020**. O abstract do `.bib` contém, de propósito:

- `parti-cipação` (hifenizado, como sai de PDF)
- `socio-economicos` (hifenizado)

E os ITEMs citam `participação local` e `socioeconomicos` — sem o hífen.

- ✅ Esperado: os dois trechos aparecem **destacados** no abstract, e o destaque
  cobre a palavra hifenizada inteira.
- ❌ Antes: nenhum dos dois era localizado.

## 5. Sobreposição sinalizada e contador honesto (F6)

Ainda no mesmo abstract. Os ITEMs 1 e 2 citam trechos que **se cruzam**
("participação local" aparece nos dois).

- ✅ Esperado:
  - **2** trechos destacados no abstract;
  - o rodapé diz **"2 excerpts in abstract"** (não 3);
  - o card do ITEM 2 exibe o selo **`overlaps another excerpt`**.
- ❌ Antes: o rodapé dizia **3** havendo 2 destaques, e o ITEM 2 sumia do
  abstract em silêncio — mantendo a cor na legenda sem marcação correspondente.

## 6. synesis-coder insere no lugar certo (F2, efeito colateral)

> Requer `synesis-coder` configurado. Pule se não estiver.

1. Selecione um trecho de texto qualquer com o cursor **dentro do primeiro ITEM
   de @souza2019**.
2. Botão direito → **Synesis: Code Selection**.
   - ✅ Esperado: o ITEM gerado é inserido logo após o `END ITEM` desse bloco.
   - ❌ Antes: era inserido **depois** do comentário `# CENARIO B ...`, ou seja,
     no território visual da fonte seguinte.

---

## 7. Hover conta conceito usado só em CHAIN (G1/G2)

O conceito `Confianca_Institucional` aparece **apenas** dentro de uma chain
(último ITEM de `@silva2020`), nunca num campo `code:`.

1. Pare o mouse sobre `Confianca_Institucional` na linha do `chain:`.
   - ✅ Esperado: a descrição da ontologia **e** "Usado em **1** itens".
   - ❌ Antes: a descrição correta seguida de **"Usado em 0 itens"**.
2. Pare o mouse sobre `Aceitacao_Social` (usado em `code:` **e** numa chain).
   - ✅ Esperado: "Usado em **2** itens".
   - ❌ Antes: 1 — a ocorrência na chain não era contada.

**Find All References** (Shift+F12) sobre `Confianca_Institucional`:
   - ✅ Esperado: 1 ocorrência.
   - ❌ Antes: nada — sugeria que o conceito não era usado em lugar algum.

**Autocomplete:** numa linha `code: `, dispare a sugestão (Ctrl+Espaço).
   - ✅ Esperado: `confianca_institucional` com "(1 usos)".

**Contraprova:** o painel **Codes** no sidebar sempre esteve correto. Os números
do hover agora devem bater com os dele.

## 8. Erro de referência inexistente aparece no editor (G3)

No fim de `teste.syn` há um bloco comentado:

```
# ITEM @barbieri20101
#     citation: "chave inexistente - deve acusar erro"
# END ITEM
```

1. **Descomente as três linhas** e salve.
2. Observe o painel **Problems** (Ctrl+Shift+M) e a marca no editor.
   - ✅ Esperado: erro na linha do `ITEM`, dizendo que não há bloco SOURCE com
     essa referência.
   - ❌ Antes: **nenhum diagnóstico** — embora `synesis compile` apontasse o erro.
3. Comente as linhas de volta e salve → o erro desaparece.

> **Limitação conhecida:** corrigir o erro sem salvar mantém a marca na tela até
> o próximo save, quando o projeto é recompilado. É esperado.

**Também vale testar:** um `SOURCE` com bibref válido e **sem nenhum ITEM** deve
gerar um **aviso** (não erro) — a severidade do compilador é preservada.

---

## Se algo falhar

Recarregue a janela (`Developer: Reload Window`) — a extensão foi reinstalada por
cima da mesma versão (0.11.0) e o VS Code pode manter o bundle antigo em memória.
Persistindo, veja o Output → **Synesis Language Server**.
