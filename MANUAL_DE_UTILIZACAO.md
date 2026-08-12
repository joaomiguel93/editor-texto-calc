# DP Calc — Manual de Utilização

Versão 0.9 · DPeople

---

## O que é o DP Calc

Um editor de texto simples onde você escreve o documento e os cálculos
ao mesmo tempo — sem precisar abrir Excel ao lado. Pense nele como um
meio-termo entre Word, Excel e um bloco de notas: você escreve
naturalmente, declara os valores que precisa, e o resultado aparece
calculado no próprio documento.

Não precisa instalar nada. Abra o arquivo `.html` em qualquer
navegador (Chrome, Edge, Firefox) e comece a digitar.

---

## Como abrir

Dê duplo clique no arquivo `DPM_Documento_Calculos_v0_9.html`. Ele abre
no navegador, funciona offline, e nenhum dado é enviado para a
internet — tudo fica só na sua tela até você salvar.

---

## A tela

- **Editor** (esquerda): onde você escreve.
- **Visualização** (direita): mostra o documento já formatado, com os
  cálculos resolvidos, atualizado a cada tecla.

---

## Declarando um dado ou um cálculo

Use `@` no início da linha:

```
@ valorAcordo = 8500
@ percentualPatronal = 20%
@ contribuicaoPatronal = valorAcordo * percentualPatronal
```

Cada linha dessas aparece no seu documento como uma linha de dado,
nesta ordem:

```
Valor Acordo         8.500,00
Percentual Patronal   20,00%
Contribuicao Patronal 1.700,00
```

Não precisa esconder nada nem lembrar de mostrar depois — o que você
declara já aparece ali, no lugar onde escreveu.

**Regra do nome:** sem espaço, sem acento, sem símbolo. Junte as
palavras e comece cada uma (a partir da segunda) com letra maiúscula:
`valorAcordo`, `percentualPatronal`, `contribuicaoIndividual`.

---

## Usando o valor dentro do texto

Depois de declarar, é só escrever `@` colado no nome, em qualquer
parte do texto:

```
O acordo foi firmado no valor de @valorAcordo.
A contribuição patronal corresponde a @contribuicaoPatronal.
```

O sistema troca automaticamente pelo valor calculado. Se o nome depois
do `@` não corresponder a nada que você declarou, o texto simplesmente
fica como está — não dá erro, não quebra a frase. Por isso é seguro
escrever coisas como `contato@empresa.com` ou mencionar alguém com
`@joão` sem se preocupar.

---

## Mostrando "@algumaCoisa" sem calcular

Se você quiser que o `@` apareça como texto puro, sem tentar
substituir, coloque entre aspas:

```
Escreva "@naoCalcular" para mostrar assim mesmo.
```

Funciona com aspas simples `'...'` ou duplas `"..."`.

---

## Porcentagem

Digite do jeito natural:

```
@ percentualPatronal = 20%
```

Internamente isso vale `0,20` — você não precisa se preocupar com
essa conversão.

---

## Operações

Dentro de uma declaração `@ nome = ...`, você pode usar:

| Operação | Símbolo |
|---|---|
| Soma | `+` |
| Subtração | `-` |
| Multiplicação | `*` |
| Divisão | `/` |
| Agrupar | `( )` |

Exemplo:

```
@ valorHora = salarioBase / horasMes
@ adicionalNoturno = valorHora * 20%
@ valorHoraNoturna = valorHora + adicionalNoturno
```

Uma variável pode usar o resultado de outra — o sistema calcula na
ordem certa sozinho.

---

## Tabelas

Use o formato padrão de tabela Markdown, e pode usar `@nome` dentro de
qualquer célula:

```
| Descrição | Valor |
|---|---:|
| Acordo | @valorAcordo |
| Patronal | @contribuicaoPatronal |
```

---

## Formatação de texto

```
**negrito**
*itálico*
`código`
# Título grande
## Título médio
> Citação
- Item de lista
```

---

## Documentos antigos

Se você já tinha um documento salvo em uma versão anterior, com
`salarioBase = 3500` (sem `@`) e `{{ salarioBase }}` no texto, ele
continua funcionando normalmente. A única diferença é que essas
declarações antigas ficam escondidas — só aparecem onde você usar
`{{ }}`. Não precisa reescrever nada.

---

## Erros comuns

| Mensagem | O que significa | Como corrigir |
|---|---|---|
| nome inválido | Nome com espaço, acento ou símbolo | Reescreva em camelCase: `valorAcordo` |
| variável duplicada | O mesmo nome foi declarado duas vezes | Renomeie uma das duas |
| variável não definida | Você usou um nome que não declarou | Confira a grafia ou declare o valor antes |
| divisão por zero | Uma conta está dividindo por zero | Confira a fórmula |
| dependência circular | Duas variáveis dependem uma da outra | Refaça a ordem do cálculo |

O contador de erros fica visível no topo da tela. Enquanto houver erro,
a linha problemática aparece como texto simples no documento, para
você localizar e corrigir.

---

## Atalhos de teclado

| Atalho | Ação |
|---|---|
| `Ctrl + Espaço` | Abre o seletor de variáveis no cursor |
| `F2` | Abre/fecha o painel de variáveis calculadas |
| `Ctrl + S` | Salva o documento como arquivo `.md` |
| `Ctrl + P` | Abre a janela de impressão (para gerar PDF) |
| `Tab` | Insere indentação no editor |

---

## Salvando e imprimindo

- **Salvar .md**: guarda o documento-fonte, editável depois. Use
  sempre que quiser continuar de onde parou.
- **Imprimir / PDF**: gera o documento final, formatado em A4, sem os
  elementos de tela (editor, barras, botões) — só o conteúdo, pronto
  para arquivar junto com a folha ou enviar ao cliente.

---

## O que a ferramenta não faz

- Não salva automaticamente — feche a aba sem salvar e o conteúdo se
  perde. Use `Ctrl + S` com frequência.
- Não se conecta com outros sistemas nem com a internet.
- Não substitui o cálculo oficial da folha — é uma ferramenta de
  documentação e memória de cálculo, não um sistema de folha de
  pagamento.

---

Dúvidas ou sugestões: contato.dpeople@gmail.com
