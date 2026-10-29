# ED5 – Da Especificação JDL à Análise da Implementação
1. Validar os pré-requisitos
Execute:
```bash
java --version
```
```bash
node --version
```
```bash
npm --version
```
O Java deverá estar na versão 21 ou superior.
---
2. Instalar o JHipster
Execute:
```bash
npm install -g generator-jhipster@9.4.0
```
Após a instalação, valide:
```bash
jhipster --version
```
Se o PowerShell bloquear o script `jhipster.ps1`, utilize:
```bash
jhipster.cmd --version
```
Resultado esperado:
```text
9.4.0
```
---
3. Criar a pasta da aplicação
Dentro de:
```text
FMR-DDD
```
crie a pasta:
```text
app-fmr
```
A estrutura deverá ficar semelhante a:
```text
FMR-DDD/
│
├── fmr.cml
├── fmr.jdl
├── src-gen/
└── app-fmr/
```
---
4. Acessar a pasta da aplicação
No terminal:
```powershell
cd app-fmr
```
Confirme que o terminal está apontando para algo semelhante a:
```text
...\FMR-DDD\app-fmr
```
---
5. Gerar a aplicação base
Execute:
```bash
jhipster
```
Se o PowerShell bloquear o script, utilize:
```bash
jhipster.cmd
```
Responda às perguntas da seguinte forma:
```text
What is the base name of your application?
fmrProjetos

Which type of application would you like to create?
Monolithic application

What is your default Java package name?
br.com.fmr.projetos

Would you like to use Maven or Gradle for building the backend?
Maven

Do you want to make it reactive with Spring WebFlux?
No

Which type of authentication would you like to use?
JWT authentication (stateless, with a token)

Besides JUnit, which testing frameworks would you like to use?
Cucumber

Which type of database would you like to use?
SQL (H2, PostgreSQL, MySQL, MariaDB, Oracle, MSSQL)

Which production database would you like to use?
PostgreSQL

Which development database would you like to use?
H2 with in-memory persistence

Which cache do you want to use?
Caffeine (local cache, for a single node)

Do you want to use Hibernate 2nd level cache?
Yes

Which other technologies would you like to use?
WebSockets using Spring Websocket

Which framework would you like to use for the client?
No client

Would you like to enable internationalization support?
No

Please choose the native language of the application
English
```
Aguarde a conclusão da geração.
Resultado esperado:
```text
Spring Boot application generated successfully.
Congratulations, JHipster execution is complete!
```
Os avisos sobre Docker Compose e Git podem ser ignorados para esta atividade.
---
6. Copiar o arquivo fmr.jdl
Com o terminal ainda em:
```text
...\FMR-DDD\app-fmr
```
execute:
```powershell
Copy-Item ..\fmr.jdl .
```
Confirme a cópia:
```powershell
dir
```
O arquivo:
```text
fmr.jdl
```
deverá aparecer dentro de `app-fmr`.
---
7. Importar o modelo JDL
Execute:
```bash
jhipster.cmd import-jdl fmr.jdl
```
O comando importa a especificação JDL e gera as estruturas correspondentes às Entities, atributos e relacionamentos definidos em `fmr.jdl`.
Durante a importação, o JHipster poderá solicitar sobrescrita de arquivos.
Quando aparecer:
```text
Overwrite src\main\resources\config\liquibase\master.xml?
```
responda:
```text
y
```
Se aparecer:
```text
Overwrite src\main\java\br\com\fmr\projetos\config\CacheConfiguration.java?
```
responda:
```text
y
```
Aguarde a conclusão da importação.
Resultado esperado:
```text
Spring Boot application generated successfully.
Congratulations, JHipster execution is complete!
```
---
8. Localizar as Entities geradas
No Explorer do VS Code, acesse:
```text
src/main/java/br/com/fmr/projetos/domain
```
Localize:
```text
Projeto.java
SolicitacaoAquisicao.java
ItemSolicitacao.java
```
---
9. Localizar os Repositories
Acesse:
```text
src/main/java/br/com/fmr/projetos/repository
```
Localize:
```text
ProjetoRepository.java
SolicitacaoAquisicaoRepository.java
ItemSolicitacaoRepository.java
```
---
10. Elementos para comparação
Em `Projeto.java`, procure:
```text
idProjeto
nome
dataInicio
dataTermino
gerenteResponsavel
situacao
```
Em `SolicitacaoAquisicao.java`, procure:
```text
idSolicitacao
dataCriacao
justificativa
idAtividade
situacao
itens
```
Em `ItemSolicitacao.java`, procure:
```text
descricao
quantidade
valorUnitarioEstimado
solicitacao
```
---
11. Procurar comportamento de domínio
Utilize a pesquisa do VS Code para procurar:
```text
AprovarSolicitacao
RejeitarSolicitacao
CancelarSolicitacao
SolicitacaoAprovada
ENVIADA
APROVADA
```
Compare os resultados com o modelo definido em:
```text
fmr.cml
```
---
12. Importante
O arquivo:
```text
fmr.cml
```
continua sendo a referência do modelo de domínio.
O arquivo:
```text
fmr.jdl
```
representa a especificação utilizada para geração.
A pasta:
```text
app-fmr/
```
contém a estrutura técnica gerada pelo JHipster.
A geração automática produz uma base técnica inicial e não substitui a implementação das regras, invariantes, operações de domínio, mudanças de estado e Domain Events.
