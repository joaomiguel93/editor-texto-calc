# DocNum

Editor de documentos para Departamento Pessoal com **variáveis nomeadas**, cálculos automáticos e visualização em tempo real.

O DocNum funciona diretamente no navegador, sem instalação, dependências externas ou conexão com a internet.

## Como usar

1. Abra o arquivo `DocNum.html` no navegador.

1. Escreva ou cole o conteúdo no painel **Editor**.

1. Acompanhe o resultado no painel **Visualização**.

1. Use **Salvar .md** para exportar o documento em Markdown.

1. Use **Imprimir / PDF** para imprimir ou gerar um PDF pelo navegador.

## Declarando variáveis

As variáveis devem ser declaradas no início da linha. Depois, podem ser usadas em qualquer parte do documento.

### Números

```
@ salarioBase = 3200
@ valorHora = salarioBase / 220
@ adicionalNoturno = salarioBase * 20%
```

### Horas

Use o formato `HH:MM`.

```
: entrada = 08:00
: saida = 17:30
: horasTrabalhadas = saida - entrada
```

Horas podem ser somadas ou subtraídas. Também podem ser multiplicadas por um número:

```
@ valorHora = 25
: horasExtras = 02:30
@ valorExtra = valorHora * horasExtras
```

### Datas

Use o formato `DD/MM/AAAA`.

```
# dataAdmissao = 15/03/2026
@ diasExperiencia = 90
# fimExperiencia = dataAdmissao + diasExperiencia
```

A diferença entre duas datas retorna a quantidade de dias:

```
@ diasTrabalhados = dataDemissao - dataAdmissao
```

## Usando variáveis no texto

Para mostrar o valor de uma variável, use `@nomeDaVariavel`:

```
O salário-base é de @salarioBase.
O valor da hora é @valorHora.
```

O DocNum substitui automaticamente a referência pelo valor calculado.

## Cálculos diretos

Também é possível calcular diretamente no texto usando colchetes:

```
O desconto aplicado foi de [500-120] reais.
```

A sintaxe antiga com chaves duplas continua disponível:

```
O resultado é {{ salarioBase / 220 }}.
```

## Formatação do documento

O editor aceita recursos básicos de Markdown:

```
# Título principal
## Título de seção

**Texto em negrito**
*Texto em itálico*

- Item de lista
- Outro item

> Observação importante
```

Também é possível criar tabelas:

```
| Verba | Valor |
|---|---:|
| Salário-base | @salarioBase |
| Valor da hora | @valorHora |
```

## Atalhos

| Ação | Atalho |
| --- | --- |
| Inserir variável | `Ctrl + Espaço` |
| Abrir painel de variáveis | `F2` |
| Salvar Markdown | `Ctrl + S` |
| Imprimir ou gerar PDF | `Ctrl + P` |
| Inserir indentação | `Tab` |
| Fechar sugestões | `Esc` |

## Indicadores e erros

O cabeçalho mostra a quantidade de variáveis e erros encontrados. Quando houver um erro de cálculo, a aplicação informa a linha e o motivo na área de visualização.

O painel **Variáveis** também permite consultar o nome, a expressão e o resultado de cada variável declarada.

## Limitações conhecidas

- Horas não tratam automaticamente virada de meia-noite.

- Os nomes das variáveis devem começar com uma letra e usar apenas letras e números, preferencialmente em `camelCase`.

- Datas devem usar o formato `DD/MM/AAAA`.

- Horas devem usar o formato `HH:MM`.

## Privacidade

O DocNum funciona localmente no navegador. O conteúdo digitado não é enviado para servidores externos.

## Licença

Este projeto é distribuído sob a licença MIT.

Desenvolvido por **João Miguel E. Ferreira — DPeople**.
