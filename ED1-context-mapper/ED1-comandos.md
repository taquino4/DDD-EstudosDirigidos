# ED1 – Comandos e Referências

## 1. Validar o Java

No terminal do VS Code, execute:

```bash
java --version
javac --version
```

Confirme que o Java e o compilador do JDK estão disponíveis no ambiente.

---

## 2. Validar o Graphviz

No terminal do VS Code, execute:

```bash
dot -V
```

Se o comando não for reconhecido, instale/configure o Graphviz antes de prosseguir.

---

## 3. Extensões do VS Code

Instale as seguintes extensões:

- **Context Mapper**
- **PlantUML**, de **jebbs**

Atalho para abrir as extensões:

```text
Ctrl + Shift + X
```

---

## 4. Estrutura inicial do projeto

Crie a pasta:

```text
FMR-DDD
```

Abra a pasta no VS Code:

```text
File → Open Folder
```

Dentro da pasta, crie o arquivo:

```text
fmr.cml
```

---

## 5. Bounded Contexts iniciais

No arquivo `fmr.cml`, declare:

```cml
BoundedContext Projetos

BoundedContext Compras

BoundedContext Financeiro
```

---

## 6. Context Map inicial

Acrescente:

```cml
ContextMap FMR {
    contains Projetos
    contains Compras
    contains Financeiro
}
```

---

## 7. Relacionamentos entre os Bounded Contexts

Evolua o `ContextMap` para:

```cml
ContextMap FMR {
    contains Projetos
    contains Compras
    contains Financeiro

    Projetos -> Compras
    Compras -> Financeiro
}
```

Nesta primeira formalização, a sintaxe simplificada:

```text
A -> B
```

representa a relação de Upstream / Downstream.

Para a FMR:

```text
Projetos -> Compras
Compras -> Financeiro
```

---

## 8. Salvar o arquivo

```text
Ctrl + S
```

---

## 9. Gerar o Context Map

Com o arquivo `fmr.cml` aberto:

```text
Ctrl + Shift + P
```

Procure um comando do Context Mapper relacionado a:

```text
Generate Context Map
```

ou:

```text
Generate Context Map Diagram
```

Quando houver opção de formato, escolha **PNG**.

Os arquivos gerados serão colocados na pasta:

```text
src-gen
```

---

## 10. Gerar diagramas PlantUML

Com o arquivo `fmr.cml` aberto:

```text
Ctrl + Shift + P
```

Procure o comando:

```text
Generate PlantUML Diagrams
```

Os arquivos `.puml` serão gerados na pasta:

```text
src-gen
```

---

## 11. Visualizar PlantUML no VS Code

Abra um arquivo `.puml` gerado e pressione:

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

Opcionalmente, o conteúdo de um arquivo `.puml` pode ser visualizado em uma ferramenta externa compatível com PlantUML.

---

## 12. Validar o projeto

Antes de encerrar o ED1, confirme:

- o arquivo `fmr.cml` está salvo;
- os três Bounded Contexts estão presentes;
- o `ContextMap FMR` contém os três contextos;
- os relacionamentos `Projetos -> Compras` e `Compras -> Financeiro` estão presentes;
- a pasta `src-gen` foi criada;
- o Context Map foi gerado;
- os arquivos `.puml` podem ser visualizados;
- a aba **Problems** do VS Code não apresenta erros de sintaxe.

---

## Fluxo das ferramentas

### Context Map

```text
fmr.cml
   ↓
Context Mapper
   ↓
Graphviz
   ↓
src-gen
   ↓
PNG
```

### PlantUML

```text
fmr.cml
   ↓
Context Mapper
   ↓
src-gen
   ↓
.puml
   ↓
PlantUML
   ↓
Diagrama
```

---

## Resultado esperado do ED1

Estrutura básica:

```text
FMR-DDD/
│
├── fmr.cml
└── src-gen/
```

O modelo deverá representar o fluxo:

```text
Projetos -> Compras -> Financeiro
```

---

## Importante

Não exclua a pasta `FMR-DDD` nem o arquivo `fmr.cml`.

O mesmo projeto será ampliado nos próximos Estudos Dirigidos.
