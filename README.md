# DP Calc

Editor de documentos de DP com variáveis nomeadas e cálculos integrados.

**Versão:** 0.9
**Arquivo:** `DPM_Documento_Calculos_v0_9.html`
**Tipo:** Single-File App (SFA) — HTML + CSS + JavaScript puro
**Autor:** João Miguel E. Ferreira · DPeople · contato.dpeople@gmail.com
**Licença:** MIT

---

## O que é

O DP Calc é um meio-termo entre Word, Excel e Notepad++: um editor de texto
(Markdown) com um motor de cálculo embutido, pensado para quem precisa
escrever um documento — relatório, memória de cálculo, rescisão, acordo —
onde texto e número precisam bater e ficar rastreáveis.

Não é uma calculadora de formulário nem uma planilha. O usuário escreve
naturalmente e declara os dados/cálculos no meio do próprio texto; o
documento final já nasce como registro auditável do raciocínio.

## Arquitetura

Um único arquivo `.html`, sem build step, sem dependência externa, sem
CDN, 100% offline. Abre direto via `file://` em qualquer navegador
moderno.

```
<style>   → CSS com tokens de cor DPeople, organizado por seção
<script>  → Parser/Engine (motor de cálculo) + renderização Markdown
```

### Motor de cálculo (`Parser` + `Engine`)

- **`Parser`**: tokeniza uma expressão (`+ - * / ( ) %`, número, variável)
  e monta uma AST respeitando precedência de operadores. Não usa `eval()`
  nem `Function()` — todo cálculo passa por essa árvore.
- **`Engine`**: lê o documento linha a linha, identifica declarações de
  variável, resolve cada uma sob demanda (recursivamente, com memoização
  em `this.vals`) e acumula erros por linha (`this.issues`).
- **`CalcError`**: erro tipado usado internamente para nome inválido,
  variável duplicada, variável inexistente, divisão por zero, resultado
  numérico inválido e dependência circular.

### Duas sintaxes de declaração (compatíveis entre si)

| Sintaxe | Visibilidade no documento | Uso |
|---|---|---|
| `@ nome = expr` | Visível — vira uma linha `Nome: valor` no local em que foi escrita | Padrão atual |
| `nome = expr` | Invisível — só aparece via `{{ }}` | Compatibilidade com documentos antigos |

Ambas passam pelo mesmo `Parser`. A diferença é só se a linha permanece
no fluxo de renderização (`keep`) e ganha uma marcação `visible:true`.

### Duas sintaxes de referência (compatíveis entre si)

| Sintaxe | Comportamento |
|---|---|
| `@nome` | Substitui pelo valor **somente se `nome` já foi declarado**; caso contrário mantém o texto original (não quebra e-mail, menção etc.) |
| `{{ expr }}` | Sintaxe antiga — aceita variável ou expressão completa |

### Escape com aspas

Texto entre `'...'` ou `"..."` é protegido de qualquer substituição
`@nome` — usado para exibir literalmente algo que comece com `@` sem
disparar cálculo. Implementado extraindo os trechos entre aspas para um
placeholder antes de rodar a regex de substituição, e restaurando depois.

### Pipeline de renderização

```
texto do editor
  → engine.load(texto)      // extrai variáveis, calcula, gera issues
  → markdown(linhas)         // converte cada linha em HTML
      → inline(texto)        // resolve {{ }}, aspas, @nome, negrito/itálico/código
  → preview.innerHTML
```

`markdown()` reconhece, nessa ordem de checagem por linha: declaração
`@` visível, bloco de código (```` ``` ````), cabeçalho (`#`), tabela,
lista, citação (`>`), parágrafo (fallback).

Um cuidado importante: a linha de declaração `@` só é renderizada como
campo (`dp-field`) se `v.line === i+1` — ou seja, se aquela linha
específica é de fato onde a variável foi registrada. Isso evita que uma
declaração duplicada ou inválida seja exibida como se tivesse dado
certo; ela cai no fallback de parágrafo, mostrando o texto bruto para o
usuário identificar o problema.

## Identidade visual

Segue o `DPEOPLE_MODULOS_PADRAO.md` (fonte de verdade): tokens de cor em
`:root` (`--dp-primary`, `--dp-bg` etc. — aqui nomeados `--blue`,
`--bg`, `--ink` por historicidade do arquivo), Segoe UI/Libre Franklin,
ícones SVG inline, rodapé com autoria, marca DPeople na impressão,
impressão A4 em Arial Narrow.

## Compatibilidade com versões anteriores

- v0.1–v0.6: sintaxe `{{ expr }}` e `nome = expr` (invisível). Ainda
  funcionam sem alteração.
- v0.9: adiciona `@` sem quebrar nada do que já existia. Um documento
  `.md` salvo em qualquer versão anterior abre normalmente.

## Testes realizados nesta versão

Validação automatizada via Node (`node --check` + execução isolada do
núcleo lógico, sem DOM):

- Sintaxe JS válida após cada alteração.
- Documento de exemplo completo (`@`, referência inline, tabela,
  citação com aspas) renderiza sem erros.
- Compatibilidade retroativa (`nome = expr` + `{{ }}`) intacta.
- Casos de erro: nome inválido, variável duplicada, divisão por zero,
  dependência circular — todos detectados e reportados com a linha
  correta, sem quebrar a renderização do restante do documento.

Não há suíte automatizada permanente incluída no arquivo (SFA sem
dependência de build/teste). Testes futuros devem seguir o mesmo
padrão: extrair o núcleo lógico do `<script>` e rodar via Node antes de
liberar.

## Limitações conhecidas / não implementado nesta fase

- Sem autosave (localStorage) — perda de conteúdo se a aba fechar sem
  `Ctrl+S`.
- Sem integração com o Hub DPeople (`dpeople:init` / `dpeople:salvar`).
- Sem exportação em Excel (`.xlsx`) — só `.md`.
- Painel de variáveis (F2) e autocomplete (Ctrl+Espaço) ainda não
  usam o nome humanizado nem mostram a memória de cálculo expansível.
- Nome de variável não pode conter espaço, acento ou hífen com espaço
  ao redor (colide com o operador de subtração).

## Roadmap (não implementado)

Ver histórico de conversas com o autor para o prompt completo de
evolução (autocomplete com nome humano, memória de cálculo clicável,
autosave, modelos/Novo/Duplicar, abas no mobile).
