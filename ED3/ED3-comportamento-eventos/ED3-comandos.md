# ED3 – Comandos e Referências

## 1. Abrir e validar o projeto existente

Abra no VS Code a pasta:

```text
FMR-DDD
```

Abra o arquivo:

```text
fmr.cml
```

Confirme que continuam presentes:

- `Projetos`;
- `Compras`;
- `Financeiro`;
- os Aggregates construídos no ED2;
- o `ContextMap FMR`.

Verifique também a aba **Problems** e confirme que o arquivo `fmr.cml` não possui erros.

---

## 2. Representar o ciclo de vida da Solicitação

Dentro do `Aggregate SolicitacaoAquisicao`, acrescente a enumeração:

```cml
enum SituacaoSolicitacao {
    aggregateLifecycle
    EM_ELABORACAO,
    ENVIADA,
    APROVADA,
    REJEITADA,
    CANCELADA
}
```

A palavra-chave:

```text
aggregateLifecycle
```

indica que os valores da enumeração representam estados do Aggregate.

### Operação associada à mudança de estado

No mesmo `Aggregate SolicitacaoAquisicao`, acrescente:

```cml
Service SolicitacaoAquisicaoService {
    void aprovarSolicitacao() : write [ ENVIADA -> APROVADA ];
}
```

A operação representa a transição:

```text
ENVIADA -> APROVADA
```

---

## 3. Criar o primeiro Domain Event

Ainda no `Aggregate SolicitacaoAquisicao`, acrescente:

```cml
DomainEvent SolicitacaoAprovada {
    String idSolicitacao
    String idProjeto
    BigDecimal valorAutorizado
}
```

A relação conceitual é:

```text
aprovarSolicitacao()
        ↓
ENVIADA -> APROVADA
        ↓
SolicitacaoAprovada
```

---

## 4. Representar eventos do Contexto de Compras

Dentro do `Aggregate ProcessoCompra`, acrescente:

```cml
DomainEvent NovaAutorizacaoSolicitada {
    String idProcessoCompra
    String idSolicitacaoOrigem
    BigDecimal valorAutorizado
    BigDecimal novoValor
}
```

Acrescente também:

```cml
DomainEvent AquisicaoConcluida {
    String idProcessoCompra
    String idSolicitacaoOrigem
    BigDecimal valorFinal
}
```

Considere as relações:

```text
NovaAutorizacaoSolicitada
Compras -> Projetos
```

```text
AquisicaoConcluida
Compras -> 
```

Não acrescente referências diretas às Entities internas dos outros Bounded Contexts.

---

## 5. Representar eventos do Contexto 

Dentro do `Aggregate ObrigacaoPagamento`, acrescente:

```cml
DomainEvent ParcelaPaga {
    String idObrigacao
    String idParcela
}
```

Acrescente também:

```cml
DomainEvent ObrigacaoQuitada {
    String idObrigacao
    String idProcessoCompraOrigem
}
```

Considere:

```text
ParcelaPaga
Evento interno ao contexto Financeiro
```

`ParcelaPaga` poderá permanecer como evento interno ao contexto Financeiro.

```text
ObrigacaoQuitada
Financeiro -> Projetos
```

`ObrigacaoQuitada` poderá interessar ao contexto Projetos para acompanhamento financeiro.

---

## 6. Identificar produtor, interessado e contrato

Utilize como referência:

| Evento | Produzido por | Interessado | Informação principal |
|---|---|---|---|
| `SolicitacaoAprovada` | Projetos | Compras | solicitação, projeto, valor |
| `NovaAutorizacaoSolicitada` | Compras | Projetos | aquisição e novo valor |
| `AquisicaoConcluida` | Compras | Financeiro | aquisição e valor final |
| `ParcelaPaga` | Financeiro | Interno ao Financeiro | obrigação e parcela |
| `ObrigacaoQuitada` | Financeiro | Projetos | obrigação e aquisição |

Regra:

> Transportar as informações necessárias, e não o modelo interno do Bounded Context.

---

## 7. Validar o arquivo CML

Salve o arquivo:

```text
Ctrl + S
```

Verifique a aba:

```text
Problems
```

Confirme que o arquivo `fmr.cml` não possui:

- erros de sintaxe;
- referências não resolvidas;
- erros de escopo;
- dependências indevidas entre Bounded Contexts.

---

## 8. Gerar novamente os diagramas PlantUML

Com o arquivo `fmr.cml` aberto, pressione:

```text
Ctrl + Shift + P
```

Procure por:

```text
Generate PlantUML Diagrams
```

Execute o comando.

Os arquivos `.puml` serão gerados novamente na pasta:

```text
src-gen
```

### Observação

A extensão PlantUML poderá apresentar o warning:

```text
Diagram unnamed. Try "@startuml name"
```

Esse warning não impediu a geração nem a visualização dos diagramas durante a validação do ED3.

Não altere manualmente os arquivos `.puml` gerados apenas para eliminar esse warning, pois eles poderão ser sobrescritos em uma nova geração.

---

## 9. Visualizar os diagramas

Abra o arquivo `.puml` desejado e pressione:

```text
Alt + D
```

Alternativamente:

```text
Ctrl + Shift + P
```

e execute:

```text
PlantUML: Preview Current Diagram
```

Também é possível visualizar o conteúdo em um visualizador PlantUML externo.

---

## 10. Verificações sugeridas

Confirme nos diagramas se aparecem os elementos acrescentados no ED3, como:

```text
SituacaoSolicitacao
SolicitacaoAprovada
SolicitacaoAquisicaoService
NovaAutorizacaoSolicitada
AquisicaoConcluida
ParcelaPaga
ObrigacaoQuitada
```

Os elementos exibidos dependerão do diagrama tático aberto.

---

## 11. Análise do modelo

Antes de encerrar o ED3, discuta:

- as mudanças de estado respeitam as regras do domínio?
- os eventos representam fatos que já aconteceram?
- existe algum evento que deveria permanecer apenas dentro do próprio contexto?
- algum evento está transportando informação demais?
- algum contexto passou a depender de uma Entity interna de outro contexto?
- os relacionamentos `Projetos -> Compras -> Financeiro` continuam preservando a autonomia dos modelos?

---

## Resultado esperado do ED3

Ao final do ED3, o projeto deverá manter:

```text
FMR-DDD/
│
├── fmr.cml
└── src-gen/
    └── arquivos .puml
```

O modelo deverá preservar os três Bounded Contexts:

```text
Projetos
Compras
Financeiro
```

e acrescentar:

- estados relevantes dos Aggregates;
- operações associadas a mudanças de estado;
- Domain Events;
- identificação de eventos internos e eventos relevantes para integração;
- diagramas PlantUML atualizados;
- Context Map preservado.

---

## Evidência

Apresente uma captura de tela contendo:

- Explorer do VS Code com a pasta `FMR-DDD`;
- arquivo `fmr.cml`;
- pelo menos um Aggregate com estados e Domain Events;
- pasta `src-gen`;
- diagrama PlantUML atualizado;
- tabela resumida com `Evento / Produtor / Interessado`.

---

## Importante

Não exclua nem substitua o trabalho produzido no ED2.

O mesmo projeto e o mesmo arquivo `fmr.cml` continuarão sendo ampliados nos próximos Estudos Dirigidos.
