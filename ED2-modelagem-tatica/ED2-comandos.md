# ED2 – Comandos e Referências

## 1. Abrir o projeto existente

Abra no VS Code a pasta:

```text
FMR-DDD
```

Abra o arquivo:

```text
fmr.cml
```

Confirme que o Context Map criado no ED1 continua presente.

---

## 2. Bounded Context Projetos

### Aggregate SolicitacaoAquisicao

```cml
BoundedContext Projetos {

    Aggregate SolicitacaoAquisicao {

        Entity SolicitacaoAquisicao {
            aggregateRoot

            String idSolicitacao
            Date dataCriacao
            String justificativa
            String situacao
            - List<ItemSolicitacao> itens

            Repository SolicitacaoAquisicaoRepository {
                @SolicitacaoAquisicao find(String idSolicitacao);
            }
        }

        ValueObject ItemSolicitacao {
            String descricao
            int quantidade
            BigDecimal valorUnitarioEstimado
        }
    }
}
```

### Aggregate Projeto

Acrescente, dentro do mesmo `BoundedContext Projetos`:

```cml
Aggregate Projeto {

    Entity Projeto {
        aggregateRoot

        String idProjeto
        String nome
        Date dataInicio
        Date dataTermino
        String gerenteResponsavel
        String situacao
        - List<AtividadeProjeto> atividades

        Repository ProjetoRepository {
            @Projeto find(String idProjeto);
        }
    }

    Entity AtividadeProjeto {
        String idAtividade
        String descricao
        - DinheiroProjeto valorAutorizado
        - DinheiroProjeto saldoDisponivel
    }

    ValueObject DinheiroProjeto {
        BigDecimal valor
        String moeda
    }
}
```

---

## 3. Bounded Context Compras

### Aggregate ProcessoCompra

```cml
BoundedContext Compras {

    Aggregate ProcessoCompra {

        Entity ProcessoCompra {
            aggregateRoot

            String idProcessoCompra
            String idSolicitacaoOrigem
            Date dataCriacao
            String situacao
            - List<ItemProcessoCompra> itens

            Repository ProcessoCompraRepository {
                @ProcessoCompra find(String idProcessoCompra);
            }
        }

        ValueObject ItemProcessoCompra {
            String descricao
            int quantidade
            BigDecimal valorUnitarioEstimado
        }
    }
}
```

### Aggregate Fornecedor

Acrescente, dentro do mesmo `BoundedContext Compras`:

```cml
Aggregate Fornecedor {

    Entity Fornecedor {
        aggregateRoot

        String idFornecedor
        String nome
        - DocumentoFiscal documentoFiscal

        Repository FornecedorRepository {
            @Fornecedor find(String idFornecedor);
        }
    }

    ValueObject DocumentoFiscal {
        String numero
    }
}
```

---

## 4. Bounded Context Financeiro

```cml
BoundedContext Financeiro {

    Aggregate ObrigacaoPagamento {

        Entity ObrigacaoPagamento {
            aggregateRoot

            String idObrigacao
            String idProcessoCompraOrigem
            Date dataCriacao
            String situacao
            - List<Parcela> parcelas

            Repository ObrigacaoPagamentoRepository {
                @ObrigacaoPagamento find(String idObrigacao);
            }
        }

        Entity Parcela {
            String idParcela
            Date dataVencimento
            String situacao
            - DinheiroFinanceiro valor
        }

        ValueObject DinheiroFinanceiro {
            BigDecimal valor
            String moeda
        }
    }
}
```

---

## 5. Context Map

Preserve o Context Map criado no ED1:

```cml
ContextMap FMR {
    contains Projetos
    contains Compras
    contains Financeiro

    Projetos -> Compras
    Compras -> Financeiro
}
```

---

## 6. Validar o modelo

Salve o arquivo:

```text
Ctrl + S
```

Verifique no VS Code:

- sublinhados vermelhos;
- mensagens da aba **Problems**;
- referências não resolvidas;
- ambiguidades de nomes;
- erros de sintaxe.

Antes de gerar os diagramas, confirme que não existem erros no arquivo `fmr.cml`.

---

## 7. Gerar diagramas PlantUML

Com o arquivo `fmr.cml` aberto:

```text
Ctrl + Shift + P
```

Procure:

```text
Generate PlantUML Diagrams
```

Os arquivos `.puml` serão criados na pasta:

```text
src-gen
```

---

## 8. Visualizar os diagramas

Abra um arquivo `.puml` e pressione:

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

Também é possível copiar o conteúdo do arquivo `.puml` para um visualizador PlantUML externo.

---

## 9. Arquivos esperados

Após a geração, a pasta `src-gen` poderá conter arquivos semelhantes a:

```text
fmr_BC_Projetos.puml
fmr_BC_Projetos_Projeto.puml
fmr_BC_Projetos_SolicitacaoAquisicao.puml

fmr_BC_Compras.puml
fmr_BC_Compras_ProcessoCompra.puml
fmr_BC_Compras_Fornecedor.puml

fmr_BC_Financeiro.puml
fmr_BC_Financeiro_ObrigacaoPagamento.puml

fmr_ContextMap.puml
```

Os nomes exatos podem variar conforme a versão da ferramenta e o modelo.

---

## 10. Pontos de atenção em CML

### Repository

O `Repository` deve ficar dentro da `Entity` marcada como:

```text
aggregateRoot
```

Exemplo:

```cml
Entity Projeto {
    aggregateRoot

    Repository ProjetoRepository {
        @Projeto find(String idProjeto);
    }
}
```

As operações do Repository devem terminar com:

```text
;
```

### Referência para outro tipo do modelo

Exemplo:

```cml
- List<AtividadeProjeto> atividades
```

ou:

```cml
- DinheiroProjeto saldoDisponivel
```

### Evitar nomes ambíguos

Evite declarar objetos táticos diferentes com exatamente o mesmo nome no modelo.

Por isso, neste projeto usamos:

```text
DinheiroProjeto
DinheiroFinanceiro
```

em vez de dois `ValueObject Dinheiro`.

---

## 11. Análise do modelo

Antes de encerrar o ED2, verifique:

- os limites dos Aggregates estão coerentes?
- algum Aggregate ficou excessivamente grande?
- alguma Entity deveria ser Value Object?
- algum Value Object deveria possuir identidade?
- os Repositories foram criados apenas para Aggregate Roots?
- algum objeto interno está sendo tratado indevidamente como Aggregate Root?
- a separação entre Projetos, Compras e Financeiro continua preservada?
- alguma referência atravessa indevidamente a fronteira de um Aggregate ou Bounded Context?

---

## Resultado esperado do ED2

Ao final do ED2, o projeto deverá possuir:

```text
FMR-DDD/
│
├── fmr.cml
└── src-gen/
    └── arquivos .puml
```

O modelo deverá conter:

```text
Projetos
├── SolicitacaoAquisicao
└── Projeto

Compras
├── ProcessoCompra
└── Fornecedor

Financeiro
└── ObrigacaoPagamento
```

Além de:

- Aggregate Roots;
- Entities;
- Value Objects;
- Repositories;
- referências internas;
- Context Map preservado.

---

## Importante

Não exclua nem substitua o trabalho produzido no ED1.

O mesmo arquivo `fmr.cml` continuará sendo ampliado nos próximos Estudos Dirigidos.
