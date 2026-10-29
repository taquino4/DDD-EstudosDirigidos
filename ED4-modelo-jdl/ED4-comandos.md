ED4 – Comandos e Conteúdo de Apoio
Do Modelo DDD ao Modelo Implementável com JDL
Este arquivo reúne os comandos e trechos de código utilizados no ED4.
> \\\*\\\*Importante:\\\*\\\* neste estudo dirigido \\\*\\\*não será instalado nem executado o JHipster\\\*\\\*.  
> O objetivo é preparar os pré-requisitos, criar a primeira especificação `fmr.jdl` e comparar o que foi preservado ou não em relação ao modelo DDD em CML.
---
1. Abrir o projeto FMR-DDD
Abra o Visual Studio Code e selecione a pasta:
```text
FMR-DDD
```
A estrutura inicial deverá conter, pelo menos:
```text
FMR-DDD/
│
├── fmr.cml
└── src-gen/
```
O arquivo `fmr.cml` deverá permanecer preservado.
---
2. Validar o Java
Abra um novo terminal no VS Code ou um Prompt de Comando e execute:
```bash
java --version
```
O ambiente deverá possuir Java 21 ou superior.
Exemplo:
```text
java 25...
```
Caso o comando não seja reconhecido, corrija a instalação ou o PATH antes de prosseguir.
---
3. Validar Node.js e npm
Após a instalação do Node.js LTS 64-bit, feche os terminais que estavam abertos e abra um novo terminal.
Execute:
```bash
node --version
```
Em seguida:
```bash
npm --version
```
Os dois comandos deverão retornar suas respectivas versões.
Exemplo:
```text
v24.22.1
11.19.1
```
Se aparecer:
```text
'node' não é reconhecido...
```
ou:
```text
'npm' não é reconhecido...
```
verifique a instalação e o PATH antes de continuar.
> \\\*\\\*Não instale o JHipster neste ED.\\\*\\\*  
> A instalação e a geração da aplicação serão realizadas no ED5.
---
4. Criar o arquivo fmr.jdl
No painel Explorer do VS Code:
Clique com o botão direito sobre a pasta `FMR-DDD`;
Escolha New File;
Digite:
```text
fmr.jdl
```
A estrutura deverá ficar semelhante a:
```text
FMR-DDD/
│
├── fmr.cml
├── fmr.jdl
└── src-gen/
```
Considere:
```text
fmr.cml → modelo de domínio
fmr.jdl → especificação para futura geração
```
---
5. Inserir a Entity Projeto
No arquivo `fmr.jdl`, insira:
```jdl
entity Projeto {
    idProjeto String required unique,
    nome String required,
    dataInicio LocalDate,
    dataTermino LocalDate,
    gerenteResponsavel String,
    situacao String
}
```
---
6. Inserir a Entity SolicitacaoAquisicao
Logo abaixo, insira:
```jdl
entity SolicitacaoAquisicao {
    idSolicitacao String required unique,
    dataCriacao LocalDate,
    justificativa String,
    idAtividade String required,
    situacao String
}
```
Observe especialmente:
```text
idAtividade
```
A solicitação conhece a identidade da atividade relacionada, sem incorporar diretamente a `Entity` interna `AtividadeProjeto` pertencente ao Aggregate `Projeto`.
---
7. Inserir a Entity ItemSolicitacao
Acrescente:
```jdl
entity ItemSolicitacao {
    descricao String required,
    quantidade Integer required,
    valorUnitarioEstimado BigDecimal required
}
```
---
8. Inserir o relacionamento
Acrescente o relacionamento entre a solicitação e seus itens:
```jdl
relationship OneToMany {
    SolicitacaoAquisicao{itens} to ItemSolicitacao{solicitacao required}
}
```
Ao final, o arquivo `fmr.jdl` deverá conter:
```jdl
entity Projeto {
    idProjeto String required unique,
    nome String required,
    dataInicio LocalDate,
    dataTermino LocalDate,
    gerenteResponsavel String,
    situacao String
}

entity SolicitacaoAquisicao {
    idSolicitacao String required unique,
    dataCriacao LocalDate,
    justificativa String,
    idAtividade String required,
    situacao String
}

entity ItemSolicitacao {
    descricao String required,
    quantidade Integer required,
    valorUnitarioEstimado BigDecimal required
}

relationship OneToMany {
    SolicitacaoAquisicao{itens} to ItemSolicitacao{solicitacao required}
}
```
---
9. Revisar o arquivo JDL
Neste ED, a revisão é estrutural e conceitual.
Verifique:
```text
\\\[ ] Entity Projeto presente
\\\[ ] Entity SolicitacaoAquisicao presente
\\\[ ] Entity ItemSolicitacao presente
\\\[ ] atributos e tipos conferidos
\\\[ ] idAtividade presente
\\\[ ] relacionamento OneToMany presente
\\\[ ] fmr.cml preservado
\\\[ ] nenhuma regra de domínio foi removida do modelo CML para adaptar o modelo ao JDL
```
A validação/importação efetiva do JDL pelo JHipster será realizada no ED5.
---
10. Comparar CML e JDL
Utilize a seguinte referência para preencher a tabela solicitada no ED4:
Elemento do modelo	CML	JDL	Observação
Entity	Sim	Sim	representação estrutural
Atributo	Sim	Sim	representação direta
Relacionamento	Sim	Sim	pode exigir adaptação
Aggregate Root	Sim	Não diretamente	decisão DDD permanece no modelo
Invariante	Sim	Não diretamente	deverá ser implementada
Operação de domínio	Sim	Não diretamente	deverá ser implementada
Domain Event	Sim	Não diretamente	deverá ser implementado
---
11. Questões para análise
Compare `SolicitacaoAquisicao` nos dois modelos e responda:
```text
Onde ficou aprovarSolicitacao()?
Onde ficou a transição ENVIADA → APROVADA?
Onde ficou SolicitacaoAprovada?
Onde estão as invariantes da solicitação?
O arquivo JDL representa todas as regras existentes no modelo de domínio?
```
Considere também:
```text
Se uma aplicação fosse gerada agora, poderíamos afirmar que o Domain Model estaria completamente implementado?
```
E reflita sobre o risco de uma estrutura formada apenas por:
```text
atributos
getters/setters
relacionamentos
persistência
```
resultar em um Anemic Domain Model.
---
12. Estrutura esperada ao final
```text
FMR-DDD/
│
├── fmr.cml
├── fmr.jdl
└── src-gen/
```
O `fmr.cml` continua sendo a referência do modelo de domínio.
O `fmr.jdl` representa uma especificação destinada à futura geração e não substitui as decisões de modelagem DDD.
A instalação do JHipster, a importação do JDL e a análise do código gerado serão realizadas no ED5.

