# BA DMO — MANUAL FUNCIONAL

Referência funcional atual do BA DMO para Sol.

## Índice

**Parte I — Modelo Global**

1. [Modelo Global — Módulos e Perfis](#p1-1)
2. [Estrutura Operacional dos Módulos](#p1-2)
3. [Utilizadores e Acesso](#p1-3)
4. [Identidade e Contexto — tool_id / jobon_id / cm_id / mf_id / bq_id](#p1-4)

**Parte II — Módulos Funcionais**

5. [Job On](#p2-jobon)
6. [Controlo](#p2-controlo)
   - [Peso (incl. Comparação)](#p2-peso)
   - [Pegamentos](#p2-pegamentos)
   - [Resumo / Folha de Controlo](#p2-resumo)
   - [Histórico do Controlo](#p2-historico-controlo)
7. [Ferramentas](#p2-ferramentas)
8. [Armazém](#p2-armazem)
9. [Boquilhas](#p2-boquilhas)
10. [Reparação Interna](#p2-ri)
11. [Reparação Externa](#p2-re)
12. [Tampões](#p2-tampoes)
13. [Admin](#p2-admin)

**Parte III — Aplicação Transversal**

14. [Shell, Navegação e Design System](#p3-shell)
15. [Documentos e Impressão](#p3-documentos)

**Parte IV — Questões em Aberto**

16. [Questões Funcionais em Aberto](#p4-abertas)

---

<a id="p1-1"></a>
# Parte I — Modelo Global

## 1. Modelo Global — Módulos e Perfis

### 1.1 Perfis funcionais

Existem exatamente **três** perfis funcionais:

1. **Admin**
2. **Operador / Controlador**
3. **Responsável**

Não existe um quarto perfil. Não existe perfil read-only autónomo. Não existe perfil de gestão, metrologia ou consulta como perfil separado.

- O **Admin** é o perfil administrativo (utilizadores, templates, aplicações, auditoria). O Admin não é implicitamente um utilizador operacional; o privilégio administrativo não concede módulos operacionais.
- O **Operador / Controlador** é o perfil operacional de execução, medição, registo e consulta, onde esse comportamento estiver definido.
- O **Responsável** é o perfil de revisão, aprovação, decisão, configuração ou validação, onde esse comportamento estiver definido.

Um perfil não concede automaticamente módulos. O perfil determina **como** o utilizador experiencia um módulo que já lhe está atribuído.

Títulos/funções de texto livre (por exemplo "Chefe", "Engenheiro", "Metrologia") são **apenas rótulos visuais**; nunca concedem permissões nem criam novos perfis.

### 1.2 O que é um módulo

**MÓDULO** = unidade lógica de acesso que pode ser atribuída a um utilizador.

Não classificar algo como módulo apenas porque tem página própria, tab própria, namespace técnico, serviço próprio ou workflow próprio. Uma página, tab, namespace, serviço ou workflow pode existir **dentro** de um módulo sem ser um módulo.

Conceitos distintos:

| Termo | Significado |
|---|---|
| **MÓDULO** | Unidade lógica de acesso atribuível a um utilizador. |
| **ÁREA INTERNA / VERTENTE** | Área funcional contida dentro de um módulo. Não é módulo de topo. |
| **WORKFLOW / TIPO DE REGISTO** | Processo ou tipo de operação dentro de uma área. Não é módulo. |
| **PERFIL FUNCIONAL** | O tipo de responsabilidade do utilizador (Admin / Operador / Controlador / Responsável). |
| **VARIANTE DE PERFIL / EXPERIÊNCIA** | Interface/workflow diferente do mesmo módulo atribuído, conforme o perfil. |
| **CONTEXTO DE PRODUÇÃO ATIVO** | O Job On / revisão selecionado usado por módulos que dependem do contexto de produção. |

### 1.3 Módulos funcionais atuais de topo

| Módulo | Classificação funcional | Atribuição |
|---|---|---|
| Job On | Módulo de topo / hub operacional | Atribuível |
| Controlo | Módulo de topo | Atribuível |
| Ferramentas | Módulo de topo | Atribuível |
| Armazém | Módulo de topo | Atribuível |
| Boquilhas | Módulo de topo | Atribuível |
| Reparação Interna | Módulo de topo | Atribuível |
| Reparação Externa | Módulo de topo | Atribuível |
| Tampões | Módulo de topo | Atribuível |
| Admin | Módulo de topo / área transversal de sistema | Atribuível |

Não existem módulos funcionais de topo separados para:

- Peso;
- Pegamentos;
- Resumo / Folha de Controlo;
- Histórico do Controlo;
- Comparação;
- História;
- Users / Access;
- Login / Auth;
- Definições;
- Design Laboratório.

### 1.4 Controlo como módulo único

**CONTROLO** é um único módulo funcional de topo. A sua estrutura interna é:

- **Peso** — área interna;
- **Pegamentos** — área interna;
- **Resumo / Folha de Controlo** — área interna;
- **Histórico do Controlo** — área interna.

Dentro de Peso:

- **Controlo inicial** — workflow;
- **Comparação** — workflow / tipo de registo.

Peso, Pegamentos, Resumo / Folha de Controlo, Histórico do Controlo e Comparação **não** são módulos de topo separados.

### 1.5 Classificação de itens não modulares

| Candidato | Classificação |
|---|---|
| Peso | Área interna do Controlo |
| Pegamentos | Área interna do Controlo |
| Resumo / Folha de Controlo | Área interna do Controlo |
| Histórico do Controlo | Área interna do Controlo |
| Comparação | Workflow / tipo de registo dentro de Peso |
| Users / Access | Área transversal de sistema, não módulo operacional atribuível |
| Login / Auth | Área transversal de sistema, não módulo |
| História | Superfície transversal de leitura de eventos de auditoria; **não** é módulo atribuível |
| Definições | Tab/área interna dentro de módulos, não módulo de topo |
| Design Laboratório | Superfície técnica de design/demonstração; não é módulo funcional |
| Verificações (por lote) | Subárea interna / tipo de registo em Ferramentas; Job On materializa as ocorrências |

### 1.6 Relação entre perfil e módulo

Dois conceitos independentes:

1. **Módulos atribuídos** — determinam **quais** módulos o utilizador pode aceder.
2. **Perfil funcional** — determina **como** o utilizador experiencia um módulo atribuído, quando existir variante dependente de perfil.

Um utilizador pode ter um módulo atribuído e a experiência dentro desse módulo variar conforme seja Operador / Controlador ou Responsável.

Um utilizador **não** perde acesso a um módulo apenas porque não tem um contexto de produção carregado. "Nenhum Job On carregado" é uma condição de contexto, não falta de permissão de módulo.

### 1.7 Navegação, visibilidade e start page

- Módulo atribuído aparece na navegação e é acessível conforme o perfil.
- Módulo não atribuído não aparece na navegação normal e não é funcionalmente acessível (barreira de acesso, não apenas ocultação de UI). Não deve existir rota/atalho que contorne a restrição.
- Dentro de um módulo atribuído, as tabs/áreas internas podem variar conforme o perfil.
- A área de Administração aparece apenas quando a permissão administrativa adequada está concedida.

**Start page confirmada:**

- utilizadores operacionais (Operador / Controlador e Responsável) iniciam no **Job On**;
- Admin puro inicia no **Admin**.

O Job On é o landing universal para utilizadores funcionais operacionais. Admin puro não recebe acesso ao Job On e cai para Admin.

Não existe escolha manual de perfil no login. O encaminhamento inicial é determinado pelo servidor.

### 1.8 Ownership e relações cross-module

| Domínio | Owner funcional |
|---|---|
| Produção / planeamento / revisão | Job On |
| Registos/resultados de controlo | Controlo |
| Master record / identidade das ferramentas (incl. BQ master) | Ferramentas |
| Movimentos de reparação externa de BQ | Boquilhas |
| Localização física / movimentos físicos | Armazém |
| Registos de reparação interna | Reparação Interna |
| Registos de reparação externa (batches) | Reparação Externa |
| Saldos/movimentos/configuração de Tampões | Tampões |
| Leitura transversal de eventos de auditoria | História (não é owner dos eventos) |
| Administração / utilizadores / templates / auditoria | Admin |

Relações principais:

- **Job On → módulos operacionais:** Job On é o hub central; os módulos downstream consomem o contexto exato do Job On e não reconstroem produção ou tooling.
- **Controlo → Job On:** Controlo consome o contexto exato de produção/revisão. A ausência de Job On carregado é condição de contexto.
- **Controlo → Ferramentas / Armazém / Boquilhas:** Controlo consome identidade de ferramentas e contexto BQ; Ferramentas mantém o master, Armazém mantém localização/movimentos, Boquilhas mantém os movimentos de reparação externa de BQ.
- **Controlo ↔ Reparação Interna:** não existe relação direta; ambos são independentes e downstream do Job On.
- **Admin ↔ módulos operacionais:** Admin não concede implicitamente módulos operacionais.
- **História ↔ módulos concedidos:** História mostra apenas eventos dos módulos concedidos ao utilizador; eventos administrativos exigem permissão específica de auditoria.

---

<a id="p1-2"></a>
## 2. Estrutura Operacional dos Módulos

### 2.1 Entrada na aplicação

Todos os utilizadores operacionais começam no **Job On**. O Job On apresenta:

- o calendário de produção / planeamento;
- as produções em curso / previstas;
- os Job On selecionáveis;
- o detalhe da produção/dia selecionado.

O Job On é o contexto central de produção/planeamento e o ponto de partida comum — não é apenas uma dependência do Controlo.

### 2.2 Contexto de Job On ativo

Alguns módulos/áreas usam o Job On selecionado na página comum como o seu **contexto de produção ativo**. O Controlo é um exemplo confirmado:

```
Job On page
  -> utilizador seleciona/carrega um Job On
  -> Controlo recebe esse contexto de produção ativo
  -> Peso, Pegamentos, Resumo / Folha de Controlo e Histórico usam o mesmo contexto
```

O estado **"Nenhum Job On carregado"** pertence a módulos/áreas que exigem contexto de produção ativo. Indica que o módulo não tem, no momento, produção contra a qual trabalhar. Não deve ser aplicado automaticamente a todos os módulos.

### 2.3 Acesso vs perfil vs contexto

| Conceito | Significa |
|---|---|
| **ACESSO AO MÓDULO** | se o utilizador pode entrar nesse módulo. |
| **PERFIL** | que experiência/ações o utilizador tem dentro do módulo. |
| **CONTEXTO DE JOB ON ATIVO** | com que produção um módulo dependente de produção está a trabalhar. |

Falta de contexto não é falta de permissão.

### 2.4 Autorização

- A navegação reflete o acesso, mas **ocultar um botão ou tab não é autorização**.
- Toda a ação sensível ou acesso a dados exige validação server-side (fail-closed).
- Deep-links para módulos não atribuídos resultam em "Acesso Negado", não num 404 genérico.
- Atributos HTML de apresentação servem apenas para orquestrar a UI; a autoridade real reside no servidor.

---

<a id="p1-3"></a>
## 3. Utilizadores e Acesso

### 3.1 Princípio

Os módulos acessíveis por cada utilizador são definidos **individualmente** no painel de Administração. Ao criar ou editar um utilizador, o Admin seleciona os módulos a que esse utilizador terá acesso.

### 3.2 Perfil vs módulos atribuídos

| Conceito | Responde a | Determina |
|---|---|---|
| **Perfil** | como o utilizador atua | a variante/experiência dentro de um módulo atribuído |
| **Módulos atribuídos** | que áreas estão disponíveis | quais módulos/áreas o utilizador pode entrar e usar |

### 3.3 O perfil não implica módulos

Um Operador / Controlador **não** recebe automaticamente todos os módulos operacionais. Um Responsável também **não**. Os módulos são escolhidos individualmente pelo Admin.

**Exemplo confirmado:** dois utilizadores Operador / Controlador podem ter módulos diferentes — Operador A: Job On, Controlo, Armazém; Operador B: Job On, Boquilhas, Tampões. Cada um vê e usa apenas os módulos que lhe foram atribuídos.

### 3.4 Módulo não atribuído

Se um módulo não estiver atribuído ao utilizador:

- não aparece na navegação normal;
- não pode ser utilizado funcionalmente;
- o utilizador não deve conseguir aceder-lhe diretamente por rota/atalho.

A não-atribuição é uma barreira de **acesso funcional**, não apenas ocultação de UI.

### 3.5 Variante dentro do módulo

Dentro de um módulo atribuído, o perfil determina a variante apresentada. Exemplo no Controlo:

- Operador / Controlador → medição, registo, OK/NOK técnico e submissão;
- Responsável → revisão, aprovação, rejeição e decisão.

---

<a id="p1-4"></a>
## 4. Identidade e Contexto — tool_id / jobon_id / cm_id / mf_id / bq_id

Este é o modelo confirmado de identidade e contexto. É a base de todas as relações funcionais entre módulos.

### 4.1 Modelo confirmado

- **tool_id = identidade canónica da Ferramenta.**
  É a identidade interna, estável e invisível da ferramenta/ficha. Não é gerada a partir de uma concatenação/hash mutável de tipo + referência + lote + máquina.

- **jobon_id = ocorrência de produção.**
  É a identidade da ocorrência de produção no Job On.

- **cm_id / mf_id / bq_id = contexto de Ferramenta congelado dentro desse Job On.**
  São a referência da ferramenta tal como ficou fixada no contexto daquela ocorrência de produção (o contexto exato da revisão).

- **A funcionalidade posterior continua relações existentes em vez de criar identidades paralelas.**
  Um módulo downstream que continua um contexto já existente liga-se à relação já estabelecida; não cria uma segunda identidade para a mesma ferramenta/produção.

### 4.2 Atributos visíveis vs identidade

Os atributos visíveis (tipo/família, referência, lote, máquina(s)/linha(s), classificação/contexto) servem para pesquisar, filtrar e confirmar a ferramenta correta. **Não** são a chave de relação cross-module.

```
Tool
- tool_id            <- estável, interno, invisível
- tipo
- referência
- lote
- máquinas/linhas [0..N]
- outros atributos reais da ferramenta
```

Os registos operacionais relevantes referenciam o mesmo **tool_id** (direta ou indiretamente através do contexto congelado do Job On — cm_id/mf_id/bq_id).

### 4.3 Regra de relação

As ferramentas não devem ser relacionadas entre módulos voltando a emparelhar repetidamente vários campos visíveis. O utilizador identifica a ferramenta através dos atributos industriais familiares; o sistema resolve e guarda a identidade estável.

Se não existir uma ficha registada correspondente, a aplicação **não** deve inventar uma identidade de ferramenta nem criar silenciosamente uma associação adivinhada. Deve usar o comportamento de alerta/tratamento de registo em falta já existente.

### 4.4 Granularidade por família

- **CM / MF:** a identidade estável da ferramenta **não** substitui o número individual da peça. Os registos que operam por peça preservam **ambos**: a identidade da ferramenta/contexto e o número individual.
- **BQ:** usa o mesmo princípio de identidade estável para a sua ficha de referência/lote. O rasto de reparação de BQ é **por quantidade** — não se cria uma identidade persistente por peça física de BQ. Os movimentos de BQ registam quantidades (e o reparador), e a referência, lote e máquina/linha permanecem atributos/contexto visíveis.

### 4.5 Atribuição de identidade no contexto de produção

- O Job On seleciona uma ferramenta/ficha existente e fixa o resolvedor do contexto dessa ferramenta na associação de produção/revisão (cm_id/mf_id/bq_id congelados nessa ocorrência de produção).
- Módulos operacionais (Controlo/Peso, Pegamentos, Reparação Interna, Armazém, movimentos de reparação externa) herdam esse contexto exato em vez de reconstruírem a produção/tooling.
- O contexto congelado preserva o que foi decidido naquela ocorrência de produção; edições posteriores noutros domínios não o reescrevem silenciosamente.

---

<a id="p2-jobon"></a>
# Parte II — Módulos Funcionais

## 5. Job On

### 5.1 Visão geral

O Job On é o **contexto central de produção/planeamento** do BA DMO. Operacionalmente é um **hub**, não apenas uma folha técnica. A "folha" é uma representação do Job On.

O Job On:

- cria e mantém o planeamento de produção;
- expõe as produções previstas através do calendário;
- identifica o contexto exato de produção (Referência, Produção, Máquina/Linha);
- mantém a revisão/snapshot exata da produção;
- identifica as ferramentas/lotes previstos para essa produção;
- fornece esse contexto de produção aos módulos operacionais;
- recebe/liga a informação que esses módulos registam sobre essa produção;
- contém as verificações de produção;
- preserva o histórico do contexto de produção;
- produz os documentos/impressão de produção.

**Regra de rastreio central:** não devem existir sistemas gerais de rastreio de produção/tooling recriados em cada módulo operacional. O contexto central é o Job On. Os módulos operacionais consomem este contexto exato, registam os seus próprios factos e não reconstroem a produção.

**Topologia conceptual** (fluxo de informação, não ownership de dados):

```
                    JOB ON
              hub central de produção
            planeamento / calendário / contexto
                       |
       +---------------+---------------+
       |               |               |
       v               v               v
   CONTROLO     REPARAÇÃO INTERNA   outros consumidores
       |
       +-- PESO
       |
       +-- PEGAMENTOS
```

Ferramentas / Boquilhas / Armazém alimentam o Job On:

```
FERRAMENTAS / BOQUILHAS / ARMAZÉM
                  |
                  v
                JOB ON
```

### 5.2 Calendário e planeamento

O calendário do Job On é o planeador/localizador central das produções. A partir do calendário o utilizador localiza uma produção, abre o seu Job On, vê o contexto de produção, vê as ferramentas previstas, vê o estado de verificação relevante e navega para informação associada a essa produção.

Comportamento estrutural preservado:

- um clique num dia seleciona-o; duplo clique abre a folha do Job On;
- dias passados mostram Referências com movimentos registados; o dia presente mostra os registos desse dia; um dia futuro oferece "Criar Job On para este dia" (o dia selecionado passa a ser a data do novo Job On);
- mudar de mês não auto-seleciona um dia;
- o calendário consulta factos registados; nunca infere entradas/saídas pela ausência de um Job On;
- depois de criado/persistido, o Job On aparece automaticamente; uma alteração de data atualiza o evento na mesma identidade estável do Job On (nunca uma cópia duplicada);
- a cor da máquina/linha identifica a máquina/linha, nunca um estado semântico.

O calendário não é um sistema de rastreio separado — é a face de agendamento do hub Job On.

### 5.3 Conceitos fundamentais

**Produção.** Uma produção é um evento de fabrico planeado identificado no Job On. Transporta o contexto necessário para executar e compreender essa produção: o produto a fabricar, o identificador de produção e a máquina/linha operacional.

**Referência de Produto.** Identifica o produto internamente:

- a parte numérica da Referência de Produto é a identificação interna do produto e é **baseada no MF** (o MF molda a forma final do produto);
- o **tipo de Marisa** é uma parte separada que identifica o tipo de gargalo/acabamento feito pela Boquilha;
- exemplo: `5447T173` = `5447` (identificação numérica baseada no MF) + `T173` (tipo de Marisa).

Não confundir: número interno do CM · número interno do MF · Referência de Produto · tipo de Marisa.

**Contexto Produção + Referência.** Dados específicos de uma exata Produção + Referência de Produto pertencem ao contexto do Job On. O Job On mantém o snapshot editável de decisão de produção para esse contexto: contexto de cabeçalho, tooling/componentes previstos, campos tipados, linhas repetíveis, verificações e notas.

**Revisão / contexto histórico.** Uma revisão do Job On é uma versão/snapshot histórico do Job On para a mesma produção num ponto no tempo. Não cria nem substitui a Produção; preserva como o contexto do Job On dessa Produção foi registado naquele momento.

- a **Produção** permanece a entidade/contexto principal;
- uma revisão não cria nem substitui uma Produção;
- múltiplas revisões podem pertencer à mesma Produção.

O Job On deve preservar o contexto histórico de produção/revisão: dados de produção passados não devem ser reescritos silenciosamente e correções não devem reinterpretar contexto histórico.

**Contexto de tooling.** O planeamento do Job On identifica o lote/ferramenta planeado exato para o tooling principal. O contexto de tooling pode incluir: tipo; referência/identificação interna; lote; máquina/linha; estado técnico; % utilização; contexto de localização/disponibilidade quando útil; último reparador/contexto de reparação quando útil. A máquina/linha é contexto registado útil, não uma regra automática de decisão.

### 5.4 O que o Job On possui

O Job On possui configuração e dados específicos de produção/referência. Pode incluir, onde aplicável:

- CM selecionado + Lote;
- MF selecionado + Lote;
- BQ selecionado + Lote;
- FF / Fundo Final;
- Calibres;
- Pinças;
- configuração específica de produção de PU;
- configuração específica de produção de CS;
- configuração específica de produção de TP / Tampão;
- outros campos específicos de produção e componentes previstos;
- detalhes/snapshots necessários para essa produção/revisão.

**Regra crítica:** dados específicos da exata Produção + Referência de Produto pertencem ao contexto do Job On. Não mover automaticamente campos da folha de produção para o master de Ferramentas.

**O Job On possui:**

- a identidade estável do Job On e as suas revisões;
- o snapshot editável completo de decisão de produção (contexto, campos, linhas, quantidades, notas);
- as seleções de ferramenta/lote escolhidas e os snapshots legíveis do que foi decidido para a produção;
- as ocorrências de verificação da produção (estado materializado, utilizador que confirmou, data/hora);
- a sua própria auditoria/histórico do contexto de produção;
- a superfície de documento/impressão de produção (produzida a partir do snapshot).

**O Job On NÃO possui:**

- dados master de tooling (domínio Ferramentas, incluindo o master BQ; Boquilhas registra apenas movimentos de reparação de BQ);
- estado físico, presença, localização e movimentos de armazém (Armazém);
- resultados de controlo (Controlo);
- resultados de Peso / Pegamentos (áreas internas do Controlo);
- registos de reparação interna (Reparação Interna);
- a imagem do artigo em si (pertence à referência master); o Job On consome a imagem de referência, não é dono dela por revisão;
- qualquer modificação de master — editar um snapshot do Job On nunca edita a ficha, estado, vida, posição ou histórico de uma ferramenta.

### 5.5 Seleção de tooling

> **O Responsável escolhe o tooling.** O Job On apresenta contexto registado; a aplicação não necessita de inferir a ferramenta correta.

O Job On usa o seu contexto de produção para **filtrar** as opções de tooling apresentadas ao Responsável. O contexto de seleção relevante inclui:

- Produção;
- Referência de Produto;
- Máquina/Linha.

Por exemplo, para um Job On na linha `B3`, o seletor de CM deve apresentar apenas as opções de CM/tool-Lote relevantes para essa Referência e registadas para `B3`. O mesmo princípio aplica-se aos outros seletores de tooling (MF, BQ).

Distinções importantes:

- **Máquina/Linha é informação registada do tool/Lote.**
- Os dados de tooling registados devem suportar filtragem pela Referência de Produto e Máquina/Linha do Job On.
- Isto é uma **regra de seleção/filtragem**, não uma nova identidade de domínio.
- `CM + Lote + Máquina` **não** é uma identidade composta de negócio.
- O Job On não necessita de inferir relações de tooling — **não existe inferência automática CM↔MF**.
- A aplicação não determina automaticamente compatibilidade nem escolhe o tooling.
- O **Responsável** faz a seleção final a partir das opções filtradas.

Para CM/MF/BQ, o tool/lote selecionado é registado contra os registos autoritativos de tooling. O Job On guarda o tool/lote selecionado mais um snapshot legível do valor decidido para essa produção. Os módulos operacionais herdam este contexto exato de tooling previsto.

### 5.6 Responsável vs Operador

- Apenas o **Responsável** edita a configuração de produção/tooling do Job On.
- O **Operador** lê/consulta o Job On.
- O **Operador** realiza a confirmação manual de verificação/check onde aplicável.

Não inventar permissões de edição adicionais. Se uma permissão não estiver confirmada, permanece em aberto.

### 5.7 Dados de tooling específicos de produção

A fronteira entre os dois domínios:

- **FERRAMENTAS** = informação registada/master estável da ferramenta.
- **JOB ON** = como o tooling/componentes selecionados são usados numa exata produção/referência.

> **Edições posteriores em Ferramentas NÃO devem reescrever silenciosamente os dados históricos de produção/revisão do Job On.** O Job On pode preservar o snapshot/detalhe relevante de produção/revisão.

FF pertence ao lado MF do contexto de produção/tooling. FF / Calibres / Pinças são dados de produção/referência do Job On, salvo evidência explícita em contrário.

#### 5.7.1 Configuração específica de produção — PU / CS / TP

Regra confirmada: **PU / CS / TP são campos de configuração específicos de produção do Job On.**

- **PU e CS** são peças mostradas/configuradas no contexto de produção.
- **TP/Tampão** aqui é o **item de tooling/configuração específico de produção** do Job On (ver o aviso de terminologia abaixo — não fundir com o valor Tampão/calote do Peso).
- **Pinças, Calibres e outros campos equivalentes específicos de produção** também são mantidos no Job On.

**Origem atual:** PU, CS e TP não têm registo/integração própria no Armazém. Por isso são configurados **manualmente no Job On** pelo **Responsável**, como parte da configuração específica de produção. O mesmo se aplica a Pinças, Calibres e aos outros campos equivalentes mantidos no Job On.

**Novo Job On:** o Responsável preenche/revê os valores específicos de produção conforme necessário (PU, CS, TP e a configuração manual equivalente como Pinças e Calibres).

**Job On duplicado:** estes valores são herdados/copiados do Job On duplicado; o **Responsável revê-os** e **altera apenas o que difere** para a nova produção.

**Padrão de utilização normal:** esta configuração manual é geralmente introduzida **principalmente no primeiro Job On/referência relevante**; os Job Ons subsequentes são **na sua maioria criados por duplicação**.

**Importante — valores copiados NÃO são defaults imutáveis:** duplicar significa copiar a configuração específica de produção para o novo Job On como ponto de partida. Não significa defaults permanentes, valores imutáveis ou correção automática para a nova produção. O **Responsável pode alterar** os valores copiados sempre que a nova produção o exigir.

> **Aviso de terminologia — TP/Tampão vs Peso Tampão/calote:** o **TP/Tampão no Job On** é o item de tooling/configuração específico de produção; o **Tampão/calote no Peso** é um **valor técnico informativo calculado** (fórmula `π × s² × (3r − s) / 3`, fora do cálculo principal do Peso). São conceitos distintos e não devem ser fundidos.

### 5.8 Verificações

A divisão de ownership estabelecida:

- a **regra/configuração** de verificação pertence ao **Lote em Ferramentas**;
- o **Job On** materializa/apresenta as ocorrências relevantes para a produção;
- o **Operador** confirma-as manualmente;
- duplicar um Lote copia a configuração, não ocorrências/histórico anteriores.

Dentro de uma produção prevista, o Job On contém ocorrências operacionais de verificação/check. Os utilizadores podem confirmá-las. Quando confirmadas, o sistema regista a ocorrência, o estado confirmado, o utilizador autenticado da aplicação e a data/hora da confirmação, no contexto da produção do Job On relevante.

A identidade do utilizador vem da sessão autenticada / contexto de utilizador atual. Não deve ser escolhida manualmente pelo browser/utilizador. A UI mantém visível quem confirmou e quando.

Exemplo:

```
MF · Confirmar folga
Confirmada
João Silva · 17/08/2026 14:32
```

Estas confirmações fazem parte do histórico da produção.

**Distinção clara:**

- **NOTAS** = informação de produção em texto livre.
- **VERIFICAÇÕES** = confirmações operacionais atribuídas, com estado + utilizador + timestamp.

Mecânica de verificação preservada:

- a confirmação é exclusivamente manual; nunca é inferida de movimentos de armazém, reparação, estado técnico, % utilização ou tempo decorrido;
- as ocorrências provêm de regras configuradas na ficha do próprio tool/lote, não dentro do Job On; o Job On materializa-as e confirma-as;
- duplicar um Job On não copia checagens antigas; gera as ocorrências da nova produção.

As ocorrências confirmadas e as confirmações/resets anteriores não são reescritas silenciosamente. O comportamento de reset/reabertura e regras de ator não são promovidos a verdade confirmada.

### 5.9 Relações com Ferramentas

- **Ferramentas fornece contexto de tooling registado.**
- **O Job On não possui os dados master de Ferramentas.**
- O Job On usa o tooling selecionado no contexto de produção.
- Alterações posteriores em Ferramentas não reescrevem os dados históricos de produção do Job On.

A informação de tool/Lote registada em Ferramentas — incluindo as associações Máquina/Linha registadas — é o que permite ao Job On filtrar as opções de tooling por Referência de Produto e Máquina/Linha. A filtragem restringe as opções; não escolhe a ferramenta.

O Job On pode apresentar informação de tooling em read-only onde útil: referência, lote, nome técnico, estado técnico, % utilização, linhas/máquina permitidas. Isto não duplica o modelo operacional completo de Ferramentas.

O `% utilização` pertence ao contexto tool/Lote em Ferramentas (lido manualmente de SAP e introduzido manualmente). O Job On consome-o em read-only onde útil.

### 5.10 Relações com Armazém

- **Armazém possui localização física e movimentos.**
- O Job On pode apresentar contexto de localização/disponibilidade para planeamento.
- **Selecionar tooling no Job On não é, por si, um movimento de armazém.** Não inferir movimentos físicos da seleção no Job On.

Durante o planeamento, o Job On precisa da situação real do tooling para julgar se as ferramentas previstas são adequadas/disponíveis. A filtragem das opções baseia-se na informação registada de tool/Lote (Referência + Máquina/Linha); o Armazém fornece contexto complementar de localização/disponibilidade.

### 5.11 Relações com Controlo

O Controlo é um **módulo funcional independente**; não é propriedade do Job On. No entanto, o seu contexto de produção vem do Job On.

Fronteira preservada:

- **Job On pergunta:** que ferramentas/lotes estão previstos para a produção?
- **Controlo pergunta:** que ferramenta/lote está a ser controlado?

**Caso A:** o Controlo controla tooling já previsto no Job On e pode receber esse contexto automaticamente. Nesse caso, usa o Job On exato, a revisão/contexto relevante para essa produção e os lotes exatos de CM / MF / BQ do Job On. No Caso A estes lotes não são re-selecionados independentemente no Controlo — o Controlo herda o tooling já filtrado e selecionado no Job On.

**Caso B:** o Controlo pode controlar outro lote válido/novo mesmo que não esteja selecionado no Job On.

> **Crítico:** selecionar outro sujeito de controlo no Caso B NÃO altera o tooling de produção do Job On.

O Controlo regista os resultados de controlo para essa produção. Não é criado um segundo sistema geral de rastreio de lotes dentro do Controlo.

**Fronteira de consumo de PU/CS:** o Resumo / Folha de Controlo pode avaliar **PU e CS** (parte das cinco peças do Resumo: CM/BQ/MF/PU/CS), mas **PU/CS vêm do contexto exato de produção/revisão do Job On** — não do Armazém. O Controlo não cria/seleciona/mantém independentemente esses valores de produção; consome o snapshot/contexto do Job On.

```
JOB ON     = contexto autoritativo de produção/tooling
CONTROLO   = registos/resultados de controlo para esse contexto
```

### 5.12 Relações com Peso

- O Peso opera dentro do contexto de produção herdado através do Job On/Controlo.
- Está associado ao contexto exato de produção do Job On.
- Usa o contexto de produção/tooling herdado onde aplicável.
- **Para o Peso, o contexto funcional de tooling é CM + Lote herdado do Job On.** O contexto de produção mais amplo pode incluir CM, MF, BQ, mas o Peso não deve depender de tipos de tooling não relacionados.
- O Peso não reconstrói independentemente a identidade de produção/tooling.

Os **Pegamentos** operam no mesmo workspace de produção, contra o mesmo contexto exato do Job On, e herdam o contexto exato de tooling de produção CM / MF / BQ. CM / MF / BQ não devem ser re-selecionados independentemente onde o modelo o proíbe.

### 5.13 Relações com Reparação

Relações confirmadas:

- **O histórico de reparação pertence à Reparação.**
- O Job On pode apresentar o último reparador / informação de reparação em read-only onde útil.
- **O Job On não possui/edita histórico de reparação.**
- **A Reparação Interna diz respeito apenas a CM/MF.**
- **BQ nunca é ferramenta de Reparação Interna.** BQ pode aparecer apenas como contexto de produção/referência.

Topologia: **não** existe relação Controlo → Reparação Interna. Ambos são módulos independentes downstream do Job On, consumindo o mesmo contexto de produção upstream.

```
               JOB ON
               /    \
              v      v
       CONTROLO    REPARAÇÃO INTERNA
```

A Reparação Interna regista o que os operadores repararam durante essa produção. Os registos de reparação permanecem associados ao Job On, produção, referência, revisão/contexto e linha/máquina corretos.

### 5.14 Relações com Boquilhas

- **BQ é uma ferramenta cujo master pertence a Ferramentas.** Boquilhas apenas registra os movimentos relacionados com a reparação externa de BQ.
- O Job On pode selecionar/usar **BQ + Lote** como contexto de tooling de produção (seleciona BQ + Lote mas não possui o master BQ).
- Onde aplicável, o seletor de BQ segue o mesmo princípio de filtragem que CM/MF: o Job On usa a sua Referência de Produto e Máquina/Linha para filtrar as opções registadas de BQ/Lote. O master de BQ permanece propriedade de Ferramentas.
- BQ é herdado pelo Controlo a partir do Job On exatamente como o contexto de lote CM/MF.

BQ participa normalmente no planeamento de produção, no contexto de Armazém e como o lote BQ exato de uma produção do Job On. A diferença-chave é o ownership da reparação: CM/MF podem usar Reparação Interna; BQ nunca.

### 5.15 Impressão / documentos do Job On

O Job On produz os documentos/impressão de produção. Os documentos impressos representam o contexto de produção/referência relevante.

As folhas de produção conhecidas incluem:

- Ficha de Artigo;
- Job-On Moldes;
- Trabalho de Equipa;
- a folha duplicada/variante exigida onde aplicável.

As folhas impressas são documentos operacionais e devem refletir o contexto exato de produção/referência. Não inventar campos a partir de capturas de ecrã, nem remover campos apenas porque parecem visualmente redundantes.

Se o ownership de algum campo de folha for incerto, mantê-lo como evidência de produção/impressão; não classificá-lo automaticamente como master de Ferramentas.

### 5.16 Histórico / revisões

- A configuração de produção do Job On deve preservar o contexto histórico/revisão.
- Alterações posteriores de dados master não devem alterar silenciosamente registos de produção passados.
- Dados de produção históricos não devem ser reescritos silenciosamente.
- A impressão deve representar o estado de produção/revisão relevante.
- Correções devem preservar o que foi registado antes; o contexto histórico nunca é reinterpretado.

Cada gravação cria uma nova revisão; as revisões mais antigas permanecem exatamente como guardadas; as correções criam sempre novas linhas.

### 5.17 Exemplos

**Exemplo A — Produção + seleção de tooling**

```
Produção 202601
Referência de Produto 5447T173

selecionado:
  CM + Lote
  MF + Lote
  BQ + Lote
  FF
  Calibres
  Pinças
  PU          (configuração específica de produção — manual)
  CS          (configuração específica de produção — manual)
  TP / Tampão (configuração específica de produção — manual)
```

Estes valores pertencem a essa exata produção/revisão. São dados de produção/referência do Job On e não se tornam automaticamente dados master em Ferramentas. Edições posteriores em Ferramentas não reescrevem estes dados históricos do Job On.

**Exemplo B — Controlo Caso A vs Caso B**

```
CASO A:
  O Controlo controla os lotes CM / MF / BQ já previstos no Job On 202601.
  Recebe esse contexto automaticamente; os lotes não são re-selecionados.

CASO B:
  O Controlo controla um lote recém-chegado que não está selecionado no Job On 202601.
  Cria um registo de controlo para esse lote, mas NÃO altera
  o tooling planeado no Job On 202601.
```

**Exemplo C — Filtro de tooling por Máquina/Linha**

```
Job On: Produção 202601, Referência 5447T173, Máquina B3

O Responsável abre o seletor de CM.
O Job On usa Referência 5447T173 + Máquina B3 para filtrar as opções registadas
de CM/tool-Lote e apresenta apenas as opções relevantes para essa Referência
e registadas para B3.

O Responsável seleciona então o CM + Lote pretendido a partir dessas opções filtradas.
```

A filtragem restringe as opções; não escolhe a ferramenta e não cria uma identidade composta.

---

<a id="p2-controlo"></a>
## 6. Controlo

### 6.1 Visão geral

O **CONTROLO** é um módulo lógico único. Concentra as atividades de medição, verificação, registo, revisão e decisão funcional relacionadas com o controlo de produção. Não é um conjunto de módulos independentes, mas uma área funcional coerente organizada em áreas internas especializadas.

Áreas internas do CONTROLO:

- **Peso** — controlo de Capacidade/Volume e peso do vidro por CM;
- **Pegamentos** — controlo dimensional de componentes;
- **Resumo / Folha de Controlo** — visão consolidada das peças controladas, avaliação técnica e decisão;
- **Histórico** — preservação dos registos, decisões, eventos e contexto de controlo.

A **Comparação** não é um módulo separado. É um fluxo de trabalho e um tipo de registo dentro do Peso, complementar ao controlo inicial, usado durante a produção.

O CONTROLO existe para apoiar a avaliação técnica e a decisão humana. O sistema calcula, compara, alerta, organiza e preserva histórico, mas não substitui a decisão final do Responsável. Resultados técnicos, médias, alertas dimensionais ou validações automáticas não podem ser interpretados como autorização automática de produção.

### 6.2 Acesso e variantes

O acesso ao CONTROLO é determinado pela atribuição funcional do módulo. Um utilizador só entra no CONTROLO se tiver acesso ao módulo.

A experiência dentro do CONTROLO varia conforme o perfil/função. Estas variantes não são módulos diferentes; são formas diferentes de usar o mesmo módulo.

**Operador / Controlador.** Utiliza sobretudo a experiência de medição e registo:

- preparar e preencher o controlo;
- registar medições;
- editar o Resumo / Folha de Controlo onde permitido;
- registar OK/NOK técnico;
- adicionar observações/comentários;
- adicionar/atualizar/abrir ligação MCaliper onde aplicável;
- submeter explicitamente o Resumo / Folha de Controlo para revisão.

O Controlador não é a autoridade final de produção. O seu OK/NOK é um resultado técnico.

**Responsável.** Utiliza sobretudo a experiência de revisão, aprovação e decisão:

- revê a Folha de Controlo;
- aprova ou rejeita a folha submetida;
- decide individualmente CMs medidos em Comparação;
- aprova ou rejeita o controlo inicial antes de produção;
- toma a decisão final de produção no âmbito do controlo.

O Controlador prepara e regista; o Responsável decide.

### 6.3 Relação com Job On

O contexto de produção/revisão usado pelo CONTROLO provém do Job On. Esta relação é de consumo, não de duplicação ou substituição.

O Job On é proprietário do contexto de produção: planeamento, revisão, configuração produtiva e contexto herdado de ferramentas/componentes. O CONTROLO consome esse contexto para criar e ancorar os seus próprios registos de controlo.

**Entrada no Controlo.** Selecionar ou abrir um Job On não abre automaticamente o CONTROLO. O contexto do Job On só é usado pelo CONTROLO quando um utilizador com acesso ao CONTROLO entra efetivamente no módulo. O CONTROLO não tem um seletor independente de Job On.

**Estado "Nenhum Job On carregado".** Quando não existe um contexto de produção válido disponível para o CONTROLO, o módulo pode apresentar "Nenhum Job On carregado". Este é um estado de contexto do CONTROLO, não um estado global da aplicação.

**Planeamento e contexto produtivo.** O Job On mantém a autoridade do planeamento de produção. O CONTROLO não reconstrói, não redefine e não substitui o planeamento do Job On, nem cria um sistema paralelo de planeamento.

**Contexto herdado de ferramentas/componentes.** O contexto produtivo pode incluir ferramentas/componentes previstos, como CM, MF, BQ, PU, CS e outros elementos relacionados com a produção.

- **Caso A — controlar as ferramentas previstas.** O utilizador controla as ferramentas/componentes previstos no contexto de produção. É a situação normal.
- **Caso B — controlar outro lote válido sem alterar o Job On.** O utilizador pode controlar outro lote válido. É um ato de controlo, não uma alteração do Job On. Não seleciona esse lote como ferramenta oficial de produção do Job On, nem altera o planeamento, a configuração produtiva ou a revisão do Job On.

Regras negativas da relação com Job On:

- O CONTROLO não redefine o planeamento do Job On.
- O CONTROLO não cria um planeamento paralelo.
- O CONTROLO não transforma controlo de lote em seleção de ferramentas de produção.
- O CONTROLO não infere CM↔MF automaticamente.
- O CONTROLO não altera a configuração produtiva do Job On.
- O CONTROLO não é proprietário de PU/CS/TP/Pinças/Calibres enquanto configuração de produção.

### 6.4 Controlo Operador / Controlador

O Operador / Controlador é responsável pela preparação técnica dos registos de controlo, antes da decisão do Responsável:

- preparar e preencher o controlo;
- registar medições;
- editar o Resumo / Folha de Controlo onde permitido;
- registar OK/NOK técnico;
- adicionar observações/comentários;
- adicionar/atualizar/abrir ligação MCaliper onde aplicável;
- submeter explicitamente o Resumo / Folha de Controlo para revisão.

A submissão é um ato explícito. A folha não é considerada submetida apenas por ter dados preenchidos. Enquanto estiver em Rascunho, pode ser editada. Depois de submetida, entra no circuito de decisão do Responsável.

### 6.5 Controlo Responsável

O Responsável é a variante de decisão dentro do CONTROLO:

- revê a Folha de Controlo;
- aprova ou rejeita a folha submetida;
- decide individualmente cada CM medido em Comparação;
- aprova ou rejeita o controlo inicial antes de produção.

No controlo inicial de Peso, o Responsável aprecia o conjunto controlado antes de produção. A aprovação inicial é geral para o conjunto controlado. Essa aprovação não transforma automaticamente qualquer medição em decisão final permanente, mas estabelece a aceitação funcional inicial.

Em Comparação, o Responsável decide individualmente cada CM medido (ver §6.7).

No Resumo / Folha de Controlo, o Responsável apenas decide folhas submetidas. Uma folha em Rascunho não está pronta para aprovação ou rejeição; a decisão ocorre após submissão explícita do Controlador.

O Responsável pode:

- aprovar a folha submetida;
- rejeitar a folha submetida;
- reabrir uma folha submetida ou já decidida;
- devolver a folha a Rascunho para edição;
- preservar o histórico das ações anteriores.

A decisão do Responsável é final no contexto funcional do controlo, mas continua a ser uma decisão humana. O sistema suporta a decisão; não a substitui.

---

<a id="p2-peso"></a>
### 6.6 Peso

O Peso é a área do CONTROLO responsável pelo controlo de Capacidade/Volume e peso do vidro por CM. O seu propósito é fornecer valores técnicos fiáveis, individuais e comparáveis, para apoiar o controlo antes e durante a produção.

O Peso distingue dois momentos principais:

- **Controlo inicial**, antes de produção;
- **Comparação**, durante produção.

Ambos usam o mesmo modelo de cálculo, mas têm finalidades funcionais diferentes.

#### 6.6.1 Entradas do Peso

O cálculo e o registo do Peso dependem de várias entradas funcionais. Estas entradas devem ser preservadas como informação relevante do controlo.

Entradas principais:

- peso de água;
- estado do molde;
- temperatura da água;
- peso nominal;
- dados anteriores de SAP ou de produção final anterior, quando estabelecido;
- notas/observações;
- valores relacionados com o processo aplicável;
- valores de referência/desenho técnico.

Estas entradas não têm todas o mesmo papel. Algumas alimentam diretamente os cálculos, outras servem para contextualizar ou complementar o registo funcional.

#### 6.6.2 Temperatura

A temperatura da água é usada no cálculo da Capacidade/Volume. A tabela de temperatura tem um intervalo canónico suportado de 5–35 °C. O valor da tabela correspondente à temperatura é usado como divisor no cálculo da Capacidade/Volume.

#### 6.6.3 Emparelhamento posicional

O número do CM é um identificador. Não é a chave de emparelhamento.

As leituras de água do mesmo controlo são emparelhadas/relacionadas pela sua posição na linha/tabela de leitura. Esta regra usa a posição da leitura/tabela; não infere o emparelhamento automaticamente pelo número do CM.

Esta regra não deve ser confundida com a associação entre o CM atual e o CM anteriormente aprovado em Comparação. Em Comparação, essa associação é explícita/validada, não inferida por emparelhamento posicional nem simplesmente pelo número do CM.

#### 6.6.4 Fórmula de Capacidade / Volume

```
Capacidade / Volume do CM = Peso de água ÷ valor da tabela de temperatura
```

A Capacidade/Volume do CM é um valor de primeira classe por CM. Não é apenas um passo intermédio. Deve permanecer visível e relevante como resultado do controlo.

#### 6.6.5 Fórmula do peso do vidro

```
Peso do vidro = (Capacidade do CM + Volume da Marisa/BQ − Volume do Punção/PU) × Densidade do vidro
```

O peso do vidro também é um valor de primeira classe por CM. Tanto Capacidade/Volume como peso do vidro são valores relevantes.

#### 6.6.6 Origem dos volumes e da densidade

O Volume da Marisa/BQ e o Volume do Punção/PU provêm de dados técnicos de referência, como desenho técnico ou dados de referência aplicáveis. Não são inventados no ato do controlo.

A densidade do vidro depende do processo aplicável, por exemplo NNPB ou PS. O processo aplicável determina o valor de densidade funcionalmente adequado.

O CONTROLO consome estes valores de referência. Não é proprietário dos dados técnicos de origem, mas usa-os para calcular o resultado do Peso.

#### 6.6.7 Apresentação decimal

Onde estabelecido, aplica-se a regra de apresentação com máximo de duas casas decimais. Esta regra não altera o cálculo funcional, mas define a apresentação do resultado quando aplicável.

#### 6.6.8 Controlo inicial

O controlo inicial acontece antes da produção. O seu objetivo é avaliar o conjunto controlado antes de este ser considerado funcionalmente aceitável para produção.

No controlo inicial:

- existe Capacidade/Volume individual por CM;
- existe peso do vidro individual por CM;
- existe uma média global do peso do vidro como informação adicional de comparação;
- a média não substitui os valores individuais;
- o conjunto é enviado para apreciação do Responsável;
- o Responsável aprova ou rejeita de forma geral antes de produção.

A média é informativa e comparativa. Não pode ocultar um resultado individual problemático. A aprovação inicial é geral para o conjunto controlado, mas não elimina a necessidade de decisões individuais onde o fluxo funcional as exige, como em Comparação.

#### 6.6.9 Comparação

A Comparação é um workflow dentro do Peso. É **opcional** e usada **durante a produção**. É complementar ao controlo inicial e **não** substitui o controlo anteriormente aprovado.

Regras confirmadas da Comparação:

- uma nova ocorrência de comparação recebe um novo **`comparacao_id`**;
- uma comparação **reutiliza o `cm_id` existente** do CM na mesma produção — isto é, reutiliza o contexto de Ferramenta congelado do CM já estabelecido nessa ocorrência de produção, em vez de criar uma identidade paralela;
- repetir a comparação mais tarde cria outro **`comparacao_id`**, mas mantém o **mesmo `cm_id`**;
- os CMs comparados podem ser **1 ou vários**;
- as medições usam o **mesmo cálculo do Peso**;
- cada CM comparado recebe uma **decisão humana individual**: **Manter** / **Colocar de parte**;
- **Colocar de parte exige justificação**;
- a Comparação **não altera**:
  - as medições originais do Peso;
  - a média original do Peso;
  - a aprovação original;
  - o PDF original;
  - os factos originais congelados;
- a Comparação **não** muta automaticamente o estado da Ferramenta, do Armazém ou do Job On.

**Não existe `previous_peso_id`.** A comparação não é medida contra um Peso de produção anterior. A Comparação pertence ao workflow do Peso e reutiliza o contexto do CM dentro da mesma produção.

O registo de Comparação preserva rastreabilidade histórica. Se um registo de Comparação revelar um problema, esse problema fica registado e pode ser decidido pelo Responsável, mas não reinterpreta nem apaga o controlo anterior.

#### 6.6.10 Tampão / Calote

O Tampão/calote pertence ao contexto de referência do Peso. É uma terceira dimensão técnica informativa, distinta do Punção/PU.

Fórmula:

```
Volume do Tampão = π × s² × (3r − s) / 3
```

onde:

- `s` = sagitta/profundidade;
- `r` = raio.

O resultado do Tampão/calote pode ser apresentado para consulta, com a regra de apresentação aplicável quando estabelecida.

Este valor existe para apoio técnico e consulta. Não é parte do cálculo principal do peso do vidro e não altera os resultados funcionais do Peso. O Tampão/calote não altera:

- o resultado do peso do vidro do CM;
- o resultado individual do CM;
- a média geral;
- a Capacidade/Volume;
- o cálculo de aprovação;
- o cálculo de Comparação.

Também não deve ser confundido com o Punção/PU. O Punção/PU entra na fórmula do peso do vidro; o Tampão/calote é apenas informativo.

O Tampão/calote do Peso não deve ser confundido com TP/Tampão do Job On. TP/Tampão no Job On é configuração produtiva/ferramenta específica da produção. O Tampão/calote no Peso é um valor técnico informativo calculado.

---

<a id="p2-pegamentos"></a>
### 6.7 Pegamentos

Os Pegamentos são a área do CONTROLO responsável pelo controlo dimensional de componentes. A lógica funcional parte de uma secção circular, com medições em dois eixos perpendiculares.

#### 6.7.1 Eixos e medições

- **Costura** = 0°;
- **Contra costura** = 90°.

Os dois eixos são perpendiculares. As medições são registadas por linha/componente, preservando a identificação do componente medido.

#### 6.7.2 Fórmula da Ovalização

```
Ovalização = Costura − Contra costura
```

O sinal da Ovalização é preservado. O sinal pode ser funcionalmente relevante e não deve ser descartado ou normalizado silenciosamente.

#### 6.7.3 Fórmula da Média

```
Média = (Costura + Contra costura) / 2
```

A Média representa o valor médio entre as duas medições. É informação útil, mas não substitui as medições individuais.

#### 6.7.4 Medição de um só eixo (ausência de Contra costura)

Uma medição nunca é bloqueada apenas porque um dos lados/eixos não está presente. Quando Contra costura não é medida ou não se aplica, a medição é válida e fica registada com Contra costura em falta:

- Ovalização fica indefinida/ausente (não é calculada);
- Média assume o valor da Costura (valor único) — `Média = Costura`;
- o corredor de tolerância aplica-se à Média nesse caso.

Quando Contra costura é fornecida, o comportamento normal mantém-se (`Ovalização = Costura − Contra costura`; `Média = (Costura + Contra costura) / 2`). A ausência de Contra costura não pode originar bloqueio na validação.

#### 6.7.5 Aplicação independente

O controlo aplica-se independentemente a:

- CM;
- BQ;
- MF.

Cada componente tem o seu próprio nominal e as suas próprias medições. Componentes diferentes não devem ser misturados como se partilhassem automaticamente o mesmo nominal ou os mesmos limites.

Costura, Contra costura e Média são verificadas independentemente. A Média não pode ocultar uma medição individual má.

O uso de BQ em Pegamentos não altera o registo mestre da ferramenta. O controlo dimensional regista resultados operacionais; a identidade mestre da ferramenta pertence a FERRAMENTAS.

#### 6.7.6 Corredor de tolerância

O corredor de tolerância é definido por:

```
Nominal − 0.20 até Nominal + 0.20
```

Regras de fronteira:

- atingir o limite cria alerta;
- cruzar o limite cria alerta;
- igualdade no limite conta como alerta.

Um valor exatamente sobre o limite não é tratado como silenciosamente aceitável; entra na condição de alerta.

#### 6.7.7 Visualização e autoridade

O mapa/visualização é uma projeção ou apresentação do modelo dimensional. Ajuda a interpretar, mas não é a autoridade de validação. A autoridade funcional permanece nas regras escritas, nos valores nominais, nas medições e nos estados definidos.

Os alertas dimensionais não bloqueiam automaticamente a produção. Avisam, destacam e suportam a análise humana.

#### 6.7.8 Artefactos visuais e regras de negócio

O contrato escrito de Pegamentos contém nominais por componente, limites por componente e dados por medição: Costura, Contra costura, Ovalização, Média e estado.

Alguns artefactos visuais podem sugerir regras adicionais, como espaçamentos, folgas ou limites específicos derivados da apresentação. Esses artefactos **não** devem ser promovidos a regras de negócio sem autoridade explícita.

Não devem ser assumidos como regras funcionais independentes:

- um limiar separado de aceitação de Ovalização;
- um valor do tipo `ovalMax = 0.20` como regra autónoma;
- uma tolerância do tipo `gapTol = 0.05` como regra de negócio;
- um espaçamento esperado entre componentes como regra independente;
- uma regra autónoma de montagem/folga;
- o nominal de um componente vizinho como nova fronteira de negócio.

Estes elementos podem ser úteis para apresentação ou leitura visual, mas não substituem o contrato escrito. A regra funcional confirmada baseia-se nos nominais e limites por componente e nas medições registadas, não em derivações visuais de espaçamento ou montagem.

---

<a id="p2-resumo"></a>
### 6.8 Resumo / Folha de Controlo

O Resumo é a apresentação consolidada da Folha de Controlo. Reúne a informação das peças controladas e serve de base à avaliação técnica e à decisão do Responsável.

O Resumo / Folha de Controlo cobre exatamente:

- CM;
- BQ;
- MF;
- PU;
- CS.

Não deve ser expandido para outras entidades sem autoridade funcional explícita.

**Informação por peça.** Por peça, o Resumo / Folha de Controlo contém funcionalmente:

- resultado técnico OK/NOK;
- observação/comentário;
- ligação MCaliper, quando aplicável.

A ligação MCaliper pode ser adicionada, atualizada ou aberta pelo utilizador onde aplicável. Não é automaticamente importada.

**Origem de PU/CS.** PU e CS apresentados no Resumo / Folha de Controlo vêm do contexto exato de produção/revisão do Job On. Não são obtidos do Armazém. O CONTROLO consome PU/CS, mas não possui nem mantém a sua configuração de produção.

**Quem edita e quem revê.** O Controlador edita e prepara o Resumo / Folha de Controlo. O Responsável revê e decide.

**Relação com o contexto do Job On.** Quando existe contexto de produção válido, o Resumo / Folha de Controlo está ancorado ao contexto exato do Job On e da revisão aplicável. Esta ancoragem permite que o histórico preserve corretamente o que foi controlado, em que contexto e sob que revisão. O Resumo não reinterpreta o Job On; apresenta a avaliação de controlo dentro do contexto recebido.

**Nota de persistência.** O Resumo / Folha de Controlo **não requer um `resumo_id` persistido** por imposição funcional. A existência e a finalidade do Resumo são funcionais (visão consolidada + avaliação + decisão); a forma concreta de persistência não é uma regra funcional.

### 6.9 Decisões e aprovações

O fluxo de decisão do Resumo / Folha de Controlo é explícito e baseado em estados funcionais.

**Estados principais:**

```
Rascunho
→ Submetida
→ Aprovada / Rejeitada
```

**Rascunho.** A folha está em preparação. O Controlador pode editar, preencher, corrigir e completar a informação técnica. Uma folha em Rascunho não está pronta para decisão; o Responsável não deve aprovar ou rejeitar uma folha que ainda não foi submetida.

**Submetida.** A folha passa a Submetida por ação explícita do Controlador. A submissão não é automática. Entra no circuito de revisão/decisão do Responsável.

**Aprovada.** A folha é Aprovada quando o Responsável aceita funcionalmente o controlo apresentado.

**Rejeitada.** A folha é Rejeitada quando o Responsável não aceita o controlo apresentado. A rejeição não apaga o histórico.

**Reabertura.** Uma folha Submetida ou já decidida pode ser reaberta para Rascunho:

```
Submetida ou decidida
→ Rascunho
```

Após rejeição, o fluxo esperado é:

```
reabrir → editar → voltar a submeter
```

A folha pode ser corrigida e novamente submetida. Os eventos anteriores não são apagados silenciosamente. A reabertura serve para corrigir sem destruir histórico.

**Resultado técnico vs decisão final.** O resultado técnico é o OK/NOK indicado pelo Controlador. A decisão final de produção é do Responsável. Consequências:

- um NOK técnico não para automaticamente a produção;
- um OK técnico não autoriza automaticamente a produção;
- a decisão final pode não coincidir mecanicamente com o resultado técnico;
- o sistema apoia a decisão, mas não decide sozinho.

---

<a id="p2-historico-controlo"></a>
### 6.10 Histórico do Controlo

O Histórico interno do CONTROLO preserva a memória funcional dos controlos. Não é apenas uma lista de eventos soltos; é a continuidade do que foi controlado, decidido e alterado.

**Contexto exato.** Os registos históricos de controlo preservam o contexto exato do Job On e da revisão quando esse contexto é usado. Revisões posteriores não reinterpretam o histórico anterior. Um registo aprovado ou histórico permanece com o contexto que tinha quando foi produzido.

**Correções e continuidade.** Correções não apagam o passado. Quando algo é corrigido, o histórico preserva a sequência funcional. A correção cria continuidade histórica, não substituição silenciosa.

**Eventos append-only.** Onde estabelecido, eventos e histórico são append-only. Novos eventos são adicionados sem apagar indevidamente eventos anteriores.

**PDF e fonte de verdade.** O PDF é derivado do registo estruturado. É imprimível e regenerável, mas não é a fonte de verdade. A fonte oficial é o registo estruturado/snapshot funcional. Se houver diferença entre o PDF e o registo estruturado, o registo estruturado prevalece funcionalmente.

O Histórico interno do CONTROLO não se confunde com uma vista histórica/transversal de eventos, apenas leitura, designada HISTÓRIA (ver §14.4).

### 6.11 Documentos

O CONTROLO pode produzir ou usar documentos associados aos controlos. A regra central é que o documento oficial é o registo estruturado/snapshot, não um ficheiro solto sem contexto.

**PDF.** O PDF é derivado. Pode ser gerado novamente a partir do registo estruturado. A sua função é apresentação, impressão ou distribuição, não autoridade de dados.

**Envio de documentos.** O envio de documentos é explícito e confirmado. O Operador / Controlador pode enviar o documento relevante quando o fluxo funcional o permite, mas esse envio não acontece automaticamente apenas porque o controlo foi concluído ou aprovado.

Antes do envio, deve existir confirmação funcional adequada. O destino depende da Máquina/Linha quando aplicável. Por exemplo, Line B e Line C podem ter grupos destinatários diferentes.

A localização concreta da configuração dos destinatários não é uma regra funcional do CONTROLO. O CONTROLO define que o envio é explícito, confirmado e orientado por contexto de Máquina/Linha; a configuração externa dos destinatários pertence ao local funcional adequado.

**Diretórios.** A estrutura de diretórios segue um princípio de referência antes de produção:

```
Root/
└── Reference/
    └── Production/
        ├── Peso/
        ├── Pegamentos/
        └── Resumo/
```

Princípios funcionais:

- Reference precede Production;
- uma Reference pode ter muitas Productions;
- apenas a raiz é configurada manualmente pelo utilizador;
- as pastas inferiores são criadas ou reutilizadas automaticamente;
- a criação/reutilização deve ser idempotente;
- o Job On acede à mesma relação exata de documento de produção/revisão;
- não existe uma árvore de documentos duplicada propriedade do Job On.

O CONTROLO não deve criar uma estrutura documental paralela para o Job On. O documento está relacionado com o contexto de produção/revisão e deve ser acedido de forma coerente pelos módulos autorizados.

### 6.12 Regras não-bloqueantes

O princípio não-bloqueante é central no CONTROLO. Separa validação estrutural/dados, resultados técnicos e decisão automática de negócio.

**Validação estrutural/dados.** Pode rejeitar uma operação objetivamente inválida quando uma regra funcional confirmada exige dados válidos. Exemplos:

- campo obrigatório estruturalmente em falta;
- formato de valor não suportado;
- transição de estado impossível;
- requisito explícito de fluxo não satisfeito.

Esta validação protege a integridade dos dados e do fluxo. Não é uma decisão automática de produção.

**Resultados técnicos, avisos e valores de controlo.** Não podem automaticamente:

- parar produção;
- remover um CM de produção;
- decidir se a produção continua;
- converter OK/NOK técnico em estado operacional automático;
- rejeitar produção automaticamente por causa de uma tolerância;
- substituir silenciosamente ferramenta/contexto;
- tomar a decisão de negócio do Responsável.

**Validação/aviso vs decisão automática de negócio:**

- validação / aviso = permitido;
- decisão automática de negócio / bloqueio duro de produção = não permitido, salvo regra de negócio explícita.

O sistema pode avisar, destacar, validar, mostrar tolerâncias, informações de comparação, médias, valores calculados, alertas dimensionais, pedir correção, guiar o utilizador e evidenciar contexto em falta ou inconsistente.

**Não-bloqueante não significa aceitar dados inválidos.** O sistema deve destacar, pedir correção e impedir operações inválidas quando a regra funcional assim o determinar. A diferença é que o sistema não transforma automaticamente um alerta técnico ou resultado de controlo numa decisão de negócio. A decisão permanece humana, suportada pelo sistema.

### 6.13 Ownership e relações cross-module

O **CONTROLO** é proprietário dos seus próprios registos e resultados de controlo, incluindo **Peso**, **Pegamentos**, **Resumo / Folha de Controlo**, avaliações técnicas, comentários, ligações MCaliper, decisões e histórico interno.

Os restantes módulos apenas fornecem ou recebem contexto relacionado:

- **Job On** fornece o contexto exato de produção/revisão e a configuração produtiva usada pelo CONTROLO.
- **Ferramentas** mantém o registo mestre das ferramentas; o CONTROLO apenas as usa no contexto do controlo.
- **Boquilhas** regista os movimentos operacionais e os movimentos associados à reparação externa das BQ; o CONTROLO apenas consome a BQ necessária ao controlo.
- **Armazém** mantém a localização e os movimentos físicos; o CONTROLO não gere stock nem movimentos.
- **História** é uma vista transversal de eventos e não se confunde com o Histórico interno do CONTROLO.

O CONTROLO não altera o planeamento do Job On, o registo mestre das ferramentas, os movimentos do Armazém ou os históricos operacionais de outros módulos.

---

<a id="p2-ferramentas"></a>
## 7. Ferramentas

### 7.1 Visão geral

**Ferramentas** é o registo mestre/tooling das ferramentas de molde usadas na produção. As famílias de ferramentas **CM (Contra Molde), MF (Molde Final), BQ, PU e CS** são ferramentas e pertencem a Ferramentas. O detalhe funcional documentado aqui foca-se em **CM** e **MF**; os campos específicos exatos de todas as famílias permanecem documentados/validados separadamente.

O cabeçalho do módulo nomeia-o "Configuração mestre de Contra Moldes e Moldes Finais".

### 7.2 Regra central — Ferramentas registra o que os utilizadores introduzem

> **Ferramentas NÃO é um motor de decisão.** Não infere a ferramenta correta, não infere relações CM/MF automaticamente, não infere compatibilidade de máquina a partir de códigos, não escolhe tooling automaticamente e não cria identidades técnicas desnecessárias.
>
> Guarda e expõe a informação CM/MF registada. Apenas dados explicitamente registados, ou explicitamente definidos por uma regra de negócio, devem ser persistidos ou interpretados.

### 7.3 Fronteiras entre domínios

| Domínio | Possui | NÃO possui |
|---|---|---|
| **FERRAMENTAS** | Informação registada estável das famílias de ferramentas (CM, MF, BQ, PU, CS): identidade, Lote, Máquinas/Linhas, Owner/Plant, estado técnico, % utilização, configuração de verificação, outros campos master/detalhe realmente registados. | Localização/movimentos físicos, campos específicos de produção do Job On, eventos/histórico de reparação, decisões automáticas de tooling. |
| **JOB ON** | Configuração e snapshots específicos de produção + referência: CM+Lote, MF+Lote, BQ+Lote, FF, Calibres, Pinças, outros campos de produção, snapshots/detalhes do tooling usado nessa produção. | Registo master de ferramentas, ownership de histórico de reparação, movimentos de armazém. |
| **ARMAZÉM** | Localização física e movimentos: Entrada/Saída, destino, paradeiro físico/operacional atual. | Ownership de master, ownership de estado técnico, registos de reparação. |
| **REPARAÇÃO INTERNA / EXTERNA** | Registos/histórico de reparação e eventos de reparação. | Registo master de ferramentas, snapshots de produção, ownership de localização. |

### 7.4 Conceitos fundamentais

**Referência de Produto.** Um código numérico como `5447` é a identificação interna do produto. A parte numérica da Referência de Produto corresponde à identificação interna usada para o **MF / forma final do produto**. O CM associado é tooling, mas o seu próprio número interno pode ser igual ou diferente do número do MF; **o número do CM não redefine a Referência de Produto**.

**Tipo de Marisa e Referência completa.** `T173` identifica o **tipo de Marisa** — a geometria de gargalo/acabamento da garrafa ou frasco feita pela Boquilha (a zona onde a tampa/rolha assenta). A Referência completa do produto = identificação numérica interna + tipo de Marisa:

```
5447 + T173 = 5447T173
```

**Terminologia — conceitos distintos:**

| Termo | Significado | Exemplo |
|---|---|---|
| Referência de Produto | identifica o produto internamente | `5447T173` |
| Identificação numérica interna do produto | identidade numérica, baseada no MF | `5447` |
| Tipo de Marisa | tipo de gargalo/acabamento feito pela Boquilha | `T173` |
| Identificação interna do MF | base numérica da Referência de Produto | `5447` |
| Identificação interna do CM | pode ser igual ou diferente do MF; nunca redefine a Referência de Produto | `5447` ou outro número |

**Lote.** Um **Lote** é uma substituição/reposição da mesma ferramenta/referência ao longo do tempo, por desgaste/degradação. Um novo Lote não é produção, não é fabrico, não é um novo produto e não é uma nova Referência — permanece a mesma ferramenta/referência (ex.: `CM 5447 — Lote 1 → Lote 2 → Lote 3` são todos `CM 5447`). As regras de numeração/sequência de lotes permanecem em aberto (ver §16).

**Máquina / Linha.** É dado registado pelo utilizador, associado ao tool/Lote. Um tool/Lote pode ter uma ou múltiplas associações Máquina/Linha (ex.: `B1` ou `B1` + `C3`). É editável onde permitido. Máquina/Linha não cria uma entidade Ferramentas por máquina nem requer uma identidade composta; não há inferência automática de compatibilidade. O **Responsável** escolhe o tool/Lote correto.

### 7.5 Tipos de ferramenta

**CM — Contra Molde.** Tipo de tooling distinto, nunca fundido com o MF. `MP` é um alias legado de importação do CM, não uma segunda família. O CM tem a sua própria identificação interna registada (ex.: `CM 7080`), que pode coincidir com a identificação numérica do MF ou diferir. Não é regra obrigatória que o código interno do CM seja igual ao código numérico da Referência de Produto.

**MF — Molde Final.** Tipo de tooling distinto, nunca fundido com o CM. Tem a sua própria identificação interna registada (ex.: `MF 7080`). O MF é o molde que molda/cria a forma física final do produto; por isso a sua identificação interna fornece a identificação numérica do produto usada na Referência de Produto. O **lado do Molde Final** agrupa MF, fundo final, BQ, AN (Anilha), CS (C. de Sopro) e PI (Pinças).

**CM vs MF:**

| Aspeto | CM | MF |
|---|---|---|
| Nome completo | Contra Molde | Molde Final |
| Alias legado | `MP` (alias de importação do CM, nunca segunda família) | nenhum |
| Lado no Job On | Lado do Contra-Molde (CM/MP, TP, PU, ARR) | Lado do Molde Final (MF, fundo final, BQ, AN, CS, PI) |
| Tipo de domínio | deve permanecer separado | deve permanecer separado |
| Âmbito da referência | identificação própria; pode igualar ou diferir do MF; nunca redefine a Referência de Produto | identificação interna é a base numérica da Referência de Produto |
| Também selecionado no Job On com | BQ | BQ |
| Tipo de reparação na Reparação Interna | reparável (CM) | reparável (MF) |

O sistema nunca funde CM e MF; mantêm identidades e históricos separados mesmo quando um par partilha o mesmo número interno.

**BQ, PU, CS e outras famílias.** BQ, PU e CS são ferramentas e pertencem a Ferramentas, juntamente com CM e MF. O módulo separado **Boquilhas** apenas registra os movimentos relacionados com a reparação externa de BQ e não possui o master de BQ. **BQ nunca é ferramenta de Reparação Interna.** Os campos específicos exatos de CM, MF, BQ, PU e CS permanecem documentados/validados separadamente.

### 7.6 Ficha da ferramenta

**O que pertence a Ferramentas.** Ferramentas possui a informação registada estável da ferramenta, incluindo, onde aplicável:

- Tipo: CM / MF;
- identificação interna da ferramenta;
- Lote;
- Máquinas/Linhas;
- Owner/Plant;
- estado técnico;
- % utilização mais recente;
- configuração de verificação;
- outros campos master/detalhe realmente registados.

> **IMPORTANTE:** não importar campos de folhas de produção impressas apenas porque aparecem ao lado de CM ou MF. Só campos que pertencem genuinamente ao registo da ferramenta pertencem a Ferramentas.

**O que Ferramentas NÃO possui:**

- localização física de armazém;
- movimentos de armazém;
- campos específicos de produção do Job On;
- Calibres;
- Pinças;
- FF como valor específico de produção do Job On;
- snapshots de produção do Job On;
- eventos/histórico de reparação;
- decisões automáticas de tooling.

Ferramentas não possui localização/movimentos/destino (Armazém) nem registos/eventos de reparação (Reparação). O detalhe da ferramenta pode expor informação/histórico de reparação em read-only, sem o duplicar como registo Ferramentas independentemente editável.

**Campos de folha impressa não são automaticamente master de Ferramentas.** Aparecer numa folha de produção do Job On não faz de um campo um dado master de Ferramentas.

**Dados registados por lote.** O registo mantém informação por lote, incluindo onde aplicável `Lote`, Máquinas/Linhas, estado técnico, `% utilização` e configuração de verificação. Uma referência pode ter muitos lotes. Novo lote via "Novo lote a partir deste" copia apenas configuração (nunca ocorrências/checagens/histórico) e mantém a identidade master em read-only.

**Modelo de identidade da ferramenta — registo, não inferência.** Ferramentas registra a informação CM/MF introduzida pelos utilizadores e expõe-na ao resto da aplicação. O sistema não necessita de inferir uma identidade de tooling adicional, nem decidir tooling operacional, a partir de Referência, Lote, Máquina ou da sua combinação.

`TIPO + REFERÊNCIA + LOTE + MÁQUINA/LINHA` é um conjunto de **dados/contexto operacionais registados** que permite ao Responsável reconhecer o tooling correto. É um contexto de apresentação/seleção para o Responsável no Job On — não é um requisito de registos master separados por máquina. A mesma Referência e Lote podem estar registados em múltiplas Máquinas/Linhas (ex.: `CM 5447`, Lote 3, Máquinas: `B1, C3`). Ferramentas deve registar e expor fielmente os valores de Máquina/Linha introduzidos pelos utilizadores.

Isto NÃO estabelece qualquer requisito de criar ou persistir uma entidade de domínio cuja chave de identidade seja `TIPO+REFERÊNCIA+LOTE+MÁQUINA`.

### 7.7 Estado técnico

**Estados técnicos conhecidos:** **Novo**, **Reparado**, **Por reparar**, **Sucatado**.

**Estado técnico vs estado operacional/físico.** Manter **estado técnico** e **estado operacional/físico** separados. Não colapsar condição técnica e paradeiro físico num único enum/modelo.

- **ESTADO TÉCNICO** pertence à informação da ferramenta (Ferramentas). Estados conhecidos: Novo; Reparado; Por reparar; Sucatado. Não adicionar "Em produção" como condição técnica.
- **"Em produção"** é um ESTADO OPERACIONAL/FÍSICO derivado/representado pelo contexto de movimento/localização. Exemplos operacionais: Em armazém; Em produção; Em reparação / enviado para reparação.

O estado técnico é informação editável da ferramenta; o estado operacional é propriedade do Armazém via movimentos/destino.

**Autoridade do estado técnico.** O detalhe/master da ferramenta é autoritativo para o estado técnico. O **Responsável** pode alterar o estado técnico a partir da ficha/detalhe da ferramenta. Não inferir transições automáticas de estado a partir de movimentos do Armazém.

**Sucatado.** É primariamente um aviso/estado visível que indica que o lote não deve normalmente ser usado em produção. **NÃO é um bloqueio duro.** Erros de registo podem acontecer e devem permanecer corrigíveis. A ferramenta pode mostrar Estado Técnico = `Sucatado`; os utilizadores podem ver claramente que não deve ser usada; a aplicação não deve bloquear permanentemente a correção/recuperação por causa desse estado.

**Sucatado vs Saída para Sucata.** Manter separados: estado técnico `Sucatado` vs movimento físico `Saída → Sucata`. Um lote pode estar marcado `Sucatado` e ainda estar fisicamente presente no Armazém. Após `Saída → Sucata`: deixa de aparecer como stock/localização ativa; o registo/histórico da ferramenta permanece preservado; o sistema deve mostrar que já saiu para sucata. Não apagar o seu histórico.

**Correção de erro — Sucata.** O Operador pode acidentalmente criar `Saída → Sucata` para a ferramenta errada. O Responsável deve poder corrigir esse erro operacional. A correção deve preservar histórico/auditoria. Não eliminar silenciosamente o evento original. Registar conceptualmente: evento original; correção/anulação; quem corrigiu; quando; motivo onde exigido.

### 7.8 Utilização / SAP

A utilização `% uso` é lida manualmente de SAP e introduzida manualmente; a aplicação nunca a calcula. O valor mais recente pertence ao contexto tool/Lote em Ferramentas, não a um facto do Job On / Armazém. Onde as leituras de utilização são registadas historicamente, as leituras anteriores devem ser preservadas em vez de sobrescritas silenciosamente.

O `% utilização` mais recente pode precisar de ser atualizado enquanto a ferramenta já está armazenada no Armazém. O utilizador permitido pode atualizar a informação de utilização sem exigir que a ferramenta saia primeiro do Armazém. O `% utilização` é informação introduzida de SAP / registada pelo utilizador — a aplicação não a calcula automaticamente.

Ownership: a utilização pertence ao lote, não à referência master. O Job On consome-a em read-only. Manual apenas; a automação SAP futura está fora de âmbito.

### 7.9 Verificações

As regras são configuradas no separador **Verificações** da ficha do lote (propriedade de Ferramentas); o Job On apenas apresenta/confirma as ocorrências geradas. Campos da regra: texto, frequência (`uma_vez_no_lote` / `por_fabrico`), ativa, criador/autor + timestamp, origem quando copiada.

**Ownership da regra.** Uma regra pertence a um **lote**, não à referência. Editar aplica-se ao futuro e nunca reescreve ocorrências/histórico.

**Copiada para novos lotes.** Duplicar um Lote copia a configuração de regras de verificação. Nunca copia ocorrências/checagens/histórico. Se as regras de verificação inativas/desativadas também são copiadas permanece uma questão funcional em aberto (ver §16).

**Semântica de frequência.** `uma_vez_no_lote` permanece pendente até à primeira checagem; `por_fabrico` cria uma ocorrência por novo Job On. A V1 não tem construtores de condição livres.

**CM vs MF.** Sem modelação separada; ambos os tipos usam o mesmo modelo de regra por lote. BQ está fora do âmbito — as regras aqui dizem respeito a tooling CM/MF.

### 7.10 Papéis e permissões

**Responsável.** Pode pesquisar/consultar ferramentas; abrir o detalhe da ferramenta; criar novos registos de ferramenta; editar a informação master/detalhe editável da ferramenta. Inclui, onde aplicável: `% utilização`; Máquinas/Linhas; Owner/Plant; estado técnico; outros campos de ferramenta definidos como editáveis. O detalhe permanece editável mesmo enquanto a ferramenta está fisicamente armazenada numa posição de Armazém. O Responsável controla edições de master e altera o estado técnico a partir da ficha/detalhe.

**Operador.** Pode:

- pesquisar ferramentas;
- consultar informação da ferramenta;
- abrir a página detalhada da ferramenta;
- criar/registar movimentos de Entrada;
- criar/registar movimentos de Saída;
- definir o destino operacional de uma Saída;
- fornecer a informação exigida pelo movimento;
- corrigir um registo operacional quando ocorreu um erro, sujeito a auditoria/histórico.

Quando a Reparação é selecionada como destino da Saída e o fluxo de reparação relevante usa um reparador: fornecer um dropdown de reparador; registar o reparador selecionado.

O **Operador** NÃO deve ter permissão irrestrita para alterar informação master de ferramenta arbitrária apenas porque a página de detalhe está visível.

**Correção de operador vs edição de master (auditoria).**

- **CORREÇÃO DE OPERADOR** = corrigir um registo operacional porque a informação foi introduzida incorretamente.
- **EDIÇÃO DE MASTER DO RESPONSÁVEL** = alterar deliberadamente a informação master/detalhe registada da ferramenta.

Não tratar estas como a mesma permissão. Uma correção de operador deve preservar auditoria/histórico. Não sobrescrever silenciosamente factos históricos. No mínimo, a auditoria conceptual deve preservar: o que foi corrigido; valor/estado anterior onde aplicável; valor corrigido; quem corrigiu; quando; motivo onde o fluxo de negócio o exigir.

### 7.11 Relações com Armazém

**Divisão de ownership:**

- **Ferramentas possui:** master/informação técnica (identidade, nome técnico, desenho, compatibilidade, vida/utilização, estado técnico).
- **Armazém possui:** posição/localização física e movimentos (Entrada/Saída), destino e paradeiro físico/operacional atual.

O Armazém não possui os dados master de Ferramentas. No entanto, uma página alcançada através do Armazém pode expor e, para utilizadores **Responsável** autorizados, editar dados de Ferramentas na sua fonte de verdade. **O ponto de entrada da UI não altera o ownership dos dados.** Deve permanecer uma única fonte de verdade para a informação da ferramenta.

Os conceitos de estado físico (posição, em produção, fora para reparação, regressada, movimento, disponibilidade) são propriedade do Armazém, apresentados ao Job On apenas para planeamento. Não são atributos de Ferramentas.

**O detalhe da ferramenta permanece editável enquanto armazenada.** Um registo de ferramenta não fica congelado por a ferramenta estar fisicamente armazenada. Exemplo: posição `3113` — `CM 5447`, Lote 3, máquinas `B1`, estado técnico `Por reparar`, utilização `54%` — a ferramenta pode estar armazenada em `3113` e a sua informação ainda pode precisar de mudar mais tarde. Exemplos de informação que pode mudar: `% utilização`; associações Máquina/Linha; Owner/Plant; estado técnico; outros campos master/detalhe editáveis. Owner/Plant é informação editável da ferramenta.

**Armazém search → detalhe da ferramenta.** Ao pesquisar/navegar uma ferramenta no Armazém, o utilizador deve poder abrir o detalhe da ferramenta associado. Essa página combina informação útil (Tipo; identificação interna; Lote; Máquinas/Linhas; Owner/Plant; estado técnico; % utilização mais recente; localização física atual; último reparador; histórico de reparação; informação de movimento/histórico onde apropriado). Isto não significa que o Armazém possua todos esses campos; a UI pode expô-los em conjunto mantendo o ownership separado.

**Criar novo registo de ferramenta a partir do Armazém (+ Criar novo).** O Armazém deve fornecer um fluxo "+ Criar novo" para um registo de ferramenta que ainda não exista. Conceitualmente preservar a divisão de ownership: (1) criar/registar a informação CM/MF (Ferramentas); (2) registar a Entrada/localização física (Armazém). O utilizador pode experienciá-lo como um único workflow. Não criar um master de ferramenta duplicado propriedade do Armazém.

**Divisão de perfil Armazém — Operador vs Responsável.** O Armazém deve ser dividido operacionalmente entre **Operador** e **Responsável**. Não copiar cegamente permissões do Controlo.

**Saída — destino operacional.** Exemplos confirmados: `Saída → Produção`; `Saída → Reparação`; `Saída → Sucata`. São factos operacionais/de movimento.

Não equiparar automaticamente a transições de estado técnico:

- `Saída → Produção` NÃO significa um estado técnico "Em produção" — porque "Em produção" não é um estado técnico;
- `Saída → Reparação` não estabelece automaticamente uma transição específica de estado técnico de Ferramentas, a menos que separadamente definido (em aberto — ver §16).

Registar primeiro o evento físico/operacional.

### 7.12 Relações com Job On

Ferramentas fornece informação registada ao Job On: Referência, Lote, nome técnico, estado técnico, % utilização e linhas/máquina permitidas. O Job On não pede a Ferramentas para decidir a ferramenta correta — o **Responsável** escolhe o tooling (`Tipo + Referência + Lote + Máquina`).

O Job On mantém o snapshot/histórico de produção/revisão do tooling usado nessa produção. Lê o estado vivo da ferramenta mas nunca sobrescreve o master. Edições posteriores em Ferramentas não reescrevem os dados históricos do Job On.

Campos específicos de produção pertencem ao contexto de produção/revisão do Job On (Calibres; Pinças; FF / Fundo Final; campos de componente específicos de produção; valores técnicos específicos usados nas folhas do Job On; detalhes/snapshots do tooling selecionado). Não mover estes para o master de Ferramentas apenas porque aparecem nas folhas impressas.

- **FERRAMENTAS** descreve a ferramenta registada.
- **JOB ON** descreve como essa ferramenta e os outros componentes são usados numa produção/referência específica.

### 7.13 Relações com Reparação

**Diretório/configuração de reparadores.** Deve existir um diretório/configuração registada de reparadores. O sistema deve fornecer uma página de configuração onde os reparadores são criados/adicionados por nome. Esta fonte de reparadores é reutilizada pelos fluxos de reparação. Não modelar reparadores como texto livre arbitrário em cada registo de reparação.

**Reparação Interna.** Aplica-se apenas a CM e MF. **BQ nunca é ferramenta de Reparação Interna.** A RI pertence ao contexto exato de produção / Job On onde aplicável e não reconstrói tooling. BQ pode aparecer no contexto de identificação de produção apenas (ex.: a Referência completa `5447T173`), em read-only — isso não significa que BQ seja reparada internamente.

**Reparador, histórico de reparação, último reparador.** Quando o movimento/workflow de reparação aplicável exige um reparador, o sistema deve registar qual o reparador associado. O utilizador deve poder selecioná-lo a partir do dropdown de reparadores registados.

Uma ferramenta deve manter histórico de reparação. A vista detalhada da ferramenta deve expor a informação/histórico de reparação relevante para essa ferramenta/lote. Ownership preservado: a REPARAÇÃO possui os registos/eventos de reparação reais; o detalhe da ferramenta pode expor/ler esses registos como informação relacionada. Não duplicar o evento de reparação como registo Ferramentas independentemente editável.

O sistema deve manter e mostrar quem reparou a ferramenta mais recentemente. "Último reparador" deve ser derivado de / suportado pelo histórico de reparação. Não deve ser um campo de texto livre não relacionado que possa divergir do histórico real.

**Regra de reparador externo CM/MF:**

- o reparador é selecionado no dropdown de reparadores registados;
- ao criar um novo registo de reparação, pré-selecionar o ÚLTIMO REPARADOR conhecido para essa ferramenta;
- o utilizador pode alterar o reparador selecionado antes de guardar.

Após guardar esse registo de reparação, o novo reparador torna-se o reparador mais recente para sugestões futuras.

### 7.14 Relações com Boquilhas

BQ é uma ferramenta cujo **master pertence a Ferramentas**. O módulo separado **Boquilhas** apenas **registra os movimentos relacionados com a reparação externa de BQ**.

**Associação de reparadores às Boquilhas:**

- os reparadores podem ser associados a Máquinas/Linhas;
- um reparador pode trabalhar com múltiplas linhas;
- ao enviar um tipo BQ/Marisa de uma linha para reparação, o reparador configurado pode ser automaticamente sugerido/associado.

Exemplo: `T173`, Linha `B1` → reparador associado a `B1`.

Isto difere da reparação externa CM/MF, onde o default é o último reparador conhecido com sobreposição manual.

### 7.15 Relações com Controlo

Dois conceitos distintos:

- **TOOLING DE PRODUÇÃO (Caso A)** — o CM/MF/BQ selecionado no Job On, herdado pelo Controlo como contexto, nunca reconstruído.
- **SUJEITO DE CONTROLO (Caso B)** — um tool/lote a ser inspecionado no Controlo. O Controlo pode identificar outro lote válido (ex.: um lote recém-chegado a controlar antes de ser selecionado num Job On) como sujeito de um registo de Controlo.

O Caso B **NÃO** altera o tooling planeado no Job On.

### 7.16 Histórico / auditoria

| Histórico | Owner |
|---|---|
| Alterações de master da ferramenta | Ferramentas (preservado onde definido) |
| Histórico de lote (incl. duplicações) | Ferramentas (preservado onde definido) |
| Histórico de utilização | Ferramentas (deve ser preservado) |
| Histórico de condição técnica | Ferramentas (preservado onde definido) |
| Histórico de regras de verificação + confirmações de ocorrência | regras = Ferramentas; ocorrências confirmadas no Job On |
| Histórico de utilização SAP | Ferramentas (deve ser preservado) |
| Histórico de reparação (Interna + Externa) | Reparação; exposto read-only no detalhe da ferramenta — nunca duplicado como registo Ferramentas editável |
| Histórico de movimentos de armazém | Armazém |
| Auditoria de correção de operador | módulo dono do registo corrigido |
| Histórico de snapshot/revisão do Job On | Job On (revisões/snapshots preservados) |
| Histórico de documento/snapshot do Controlo | Controlo |

Ferramentas não é owner dos históricos de movimento de armazém, reparação, snapshot de produção ou Controlo, mesmo onde fornece a identidade da ferramenta que esses registos referenciam. Correções relevantes devem permanecer auditáveis e o histórico de alterações técnicas/master deve ser preservado onde definido.

---

<a id="p2-armazem"></a>
## 8. Armazém

> Regra de ouro: **não reinterpretar limitações de implementação como regras funcionais.**

### 8.1 Objetivo e papel

O **Armazém** é o módulo funcional responsável pela **localização física** e pelos **movimentos físicos** das ferramentas.

Finalidade real:

- guardar a **posição física atual** onde cada ferramenta/lote está, ou registar que está fora do Armazém;
- registar o **histórico de movimentos** de Entrada/Saída;
- expor **disponibilidade, presença, paragem física e paradeiro** às outras áreas, em particular ao Job On para planeamento;
- registar o **destino operacional** de uma Saída: **Fabricação**, **Reparação** ou **Sucata**;
- quando a Saída tem destino **Reparação**, registar o **reparador selecionado**, associado ao movimento e preservado no histórico;
- permitir **consulta/pesquisa** de ferramentas e posições;
- permitir **correção de localização** quando a realidade física difere do registado;
- alimentar alertas operacionais (ferramenta sem localização operacional; conflito de referências na mesma posição; pendência de atualização da % de uso).

### 8.2 Destino da Saída ≠ Estado técnico

Regra fechada:

- **Destino da Saída** é operacional e responde a "para onde vai a ferramenta?" — Fabricação / Reparação / Sucata.
- **Estado técnico** é a condição técnica da ferramenta e responde a "qual é a condição da ferramenta?" — Novo / Reparado / Por reparar / Sucatado / Arquivado.

Portanto:

- `Saída → Reparação` **não implica automaticamente** estado técnico "Por reparar" ou "Reparado";
- `Saída → Sucata` **não implica automaticamente** estado técnico "Sucatado";
- um movimento de Armazém **nunca altera silenciosamente** o estado técnico.

### 8.3 Separação central de ownership

| Conceito | Owner funcional |
|---|---|
| Paradeiro físico: localização, presença, em produção, em reparação, fora | **ARMAZÉM** via movimentos/destino |
| Estado técnico da ferramenta | **Ferramentas** |
| Identidade / master record da ferramenta | **Ferramentas** |
| Atribuição / alocação de produção | **Job On** |
| Workflow / registo de reparação | **Reparação** |

O Armazém não é o master das ferramentas. Pode consumir esses dados como contexto read-only para identificar uma ferramenta, mas não os possui nem os altera como módulo owner.

### 8.4 Âmbito

Módulo funcional de topo, atribuível individualmente por utilizador em Admin. Variantes de perfil Operador / Controlador e Responsável confirmadas como requisito. Não existem módulos separados "Armazém Operador" e "Armazém Responsável": são variantes do mesmo módulo ARMAZÉM. O Admin não é um perfil operacional do Armazém e não recebe implicitamente o módulo operacional.

**No âmbito:** posição física atual de ferramenta/lote; movimentos físicos de Entrada/Saída; destino operacional da Saída; seleção de reparador quando a Saída é para Reparação; histórico de movimentos físicos; consulta de localização e contexto; correção física auditável; alertas operacionais relacionados com localização física.

**Fora do âmbito:** master record da ferramenta; estado técnico; % de uso como dado editável; planeamento ou atribuição de produção; workflow de reparação; movimentos de reparação externa de BQ; saldos de Tampões; histórico transversal de auditoria como owner.

### 8.5 Ferramentas / tipos abrangidos

- **CM**, **MF** e **BQ** usam o mesmo modelo normal de ferramenta/master/armazém: master em **Ferramentas**; registo/localização/movimentos físicos em **Armazém**.
- A diferença funcional relevante do **BQ** é o workflow de reparação externa, que pertence a **Boquilhas**.
- **BQ nunca é Reparação Interna.**
- **PU** e **CS** são ferramentas em Ferramentas, mas **não estão integrados/registados no Armazém**; são configuração específica de produção do Job On.

| Tipo | Ferramentas possui master? | Armazém funcional? | Fluxo de reparação especial |
|---|---:|---:|---|
| CM | Sim | Sim | modelo de reparação CM |
| MF | Sim | Sim | modelo de reparação MF |
| BQ | Sim | Sim (modelo normal) | Boquilhas: fluxo de reparação externa; BQ nunca RI |
| PU | Sim | Não | — (config de produção Job On) |
| CS | Sim | Não | — (config de produção Job On) |
| Pinças / Calibres / equivalentes | config | Não | config específica de produção do Job On |

A ausência de suporte técnico a BQ no Armazém é uma matéria de reconciliação técnica, **não** um adiamento funcional.

**Identidade no Armazém.** O Armazém guarda o **ID estável** da ferramenta/lote (tool_id). A identidade é resolvida através do mecanismo próprio do Armazém. A representação de identidade pode distinguir o domínio de origem, mas isso não implica que BQ fique fora do modelo normal de Armazém.

### 8.6 Utilizadores, perfis e acesso

Exatamente três perfis. Sem quarto perfil, sem perfil read-only autónomo.

| Perfil | Acesso ao Armazém | Comportamento por perfil |
|---|---:|---|
| Admin | Não, não operacional | — |
| Operador / Controlador | Sim quando atribuído | experiência operacional |
| Responsável | Sim quando atribuído | experiência de revisão/gestão |

**Operador / Controlador** pode, onde suportado:

- pesquisar ferramentas;
- consultar ferramentas;
- abrir/ler o detalhe da ferramenta;
- criar/registar movimentos de Entrada;
- criar/registar movimentos de Saída;
- definir o destino operacional da Saída: Fabricação / Reparação / Sucata;
- selecionar o reparador quando Saída → Reparação, a partir do diretório canónico;
- fornecer a informação requerida pelo movimento;
- confirmar movimentos físicos;
- corrigir um registo operacional quando houve erro, preservando auditoria/histórico.

Limites: abrir o detalhe da ferramenta **não dá ao Operador permissão descontrolada para editar master**; o Operador não altera silenciosamente estado técnico através de movimento físico.

**Responsável** pode, quando o módulo estiver atribuído: aceder ao Armazém; pesquisar e consultar informação física/localização; abrir o detalhe da ferramenta a partir do Armazém; editar master/detalhe da ferramenta quando autorizado. **MASTER OWNER = Ferramentas. MASTER SOURCE OF TRUTH = Ferramentas. UI ENTRY POINT pode ser Armazém.** Se o Responsável edita a ficha aberta a partir do Armazém, a operação e a persistência permanecem em Ferramentas.

**Correção de Operador vs Edição de Master do Responsável** — não são a mesma permissão.

- **Correção operacional do Operador:** corrige registo físico/operacional do Armazém; preserva auditoria/histórico; não apaga silenciosamente movimentos anteriores; não se torna manutenção irrestrita de master.
- **Edição de master pelo Responsável:** altera deliberadamente master/detalhe da ferramenta; pertence a Ferramentas; pode ser iniciada através de detalhe aberto via Armazém; é persistida na fonte de verdade Ferramentas.

A **existência** da divisão Operador/Responsável no Armazém está confirmada e fechada. A **distribuição exata** das ações entre Operador e Responsável permanece em aberto (ver §16).

### 8.7 Estrutura interna do módulo

Áreas internas:

- **Registo** — workflows de Entrada/Saída;
- **Consulta** — pesquisa/localização;
- **Programadas** — área interna; alvo funcional exato em aberto (ver §16);
- **Histórico** — histórico de movimentos de localização.

Não criar dentro do Armazém áreas de "Definições" para reparadores, estados ou vida útil.

A existência funcional de **Saídas Programadas** é do workflow partilhado Reparação ↔ Armazém; não é um módulo separado.

### 8.8 Modelo de localização física

**Sem hierarquia.** O modelo não possui hierarquia de armazém, zona, corredor, prateleira, pallet ou slot. Existe apenas uma **posição física** identificada por um código único. Não inventar hierarquia de localização.

**Código de posição.** Exatamente **4 dígitos** (padrão `^\d{4}$`). Validação: "A posição deve ter exatamente 4 dígitos." Exemplos válidos: `2421`, `0001`. Inválidos: `242`, `24211`, `24A1`, string vazia.

**Lista plana de posições.** A localização é uma lista plana de posições. Uma posição existe quando criada; a criação pode ser automática quando necessário. Não existe flag de "posição ativa/inativa". A libertação é do **stock/ocupação**, não da posição enquanto entidade.

**Ocupação 1:1.** Uma posição é ocupada por, no máximo, **um lote de ferramenta ativo** de cada vez:

- a ocupação representa o facto posição ↔ lote de ferramenta;
- ocupação ativa = linha de ocupação sem data de libertação;
- libertar mantém a linha histórica com a data de libertação preenchida;
- factos históricos são preservados;
- não podem coexistir duas ocupações ativas da mesma posição/lote.

**Conflito de referências.** Duas referências diferentes ativas na mesma posição não são permitidas. Se coexistirem, a consulta por posição devolve aviso de qualidade de dados. Nunca normalizar silenciosamente o conflito.

### 8.9 Funcionamento geral

1. **Entrada / Repor** — a ferramenta ocupa uma posição de 4 dígitos; cria movimento `in`; a posição passa a ser a localização atual.
2. **Saída imediata / Retirar** — a ferramenta deixa de ocupar a posição; cria movimento `out`; regista destino operacional Fabricação / Reparação / Sucata; quando Reparação, seleciona e registra reparador. A posição só é libertada após persistência com sucesso.
3. **Consulta** — pesquisa por tipo, referência, lote ou posição; mostra contexto de localização: `armazem`, `fora` ou `nao_registado`, posição e avisos de conflito.
4. **Histórico** — movimentos por ferramenta/lote: Entrada/Saída, posição, destino, reparador quando aplicável, observações, data/hora e operador.
5. **Saídas Programadas** — workflow funcional partilhado com Reparação. A lista é criada na Reparação e executada fisicamente no Armazém.
6. **Correção de localização** — quando o operador encontra diferença física, pode abrir Corrigir localização.
7. **Alertas** — localização operacional não registada; conflito de contexto; atualização de % uso pendente.

**Interação:** cartões abrem inline em Registo/Consulta, sem nova página/modal no fluxo normal. A interface só mostra sucesso depois da persistência. Não deve existir falso sucesso.

### 8.10 Entrada / Repor

**Trigger.** Registo manual pelo utilizador.

- **Entrada** = ocupação inicial ou registo de presença.
- **Repor** = reocupação após Saída.

**Validações:**

- Posição obrigatória.
- Posição deve ter exatamente 4 dígitos.
- Referência ou lote obrigatórios.
- Ferramenta deve existir.
- Posição não pode estar ocupada por outra ferramenta.

**Efeito funcional.** A Entrada: cria stock ativo / ocupação; cria movimento de entrada; persiste ocupação e movimento na mesma operação atómica; regista o evento de auditoria; torna a posição introduzida a localização atual da ferramenta/lote. A operação protege a ocupação contra operações concorrentes na mesma posição.

**Re-entrada na mesma posição.** Reentrada da mesma ferramenta na mesma posição já ocupada deve ser tratada como conflito controlado, não como violação cega de índice único.

**Máquina / Estado na Entrada.** A Entrada pode apresentar Máquina e Estado (Reparado | Por reparar | Novo), para além de Posição, Referência, Lote e Observações. A presença funcional de Máquina/Estado na Entrada carece de reconciliação; a classificação exata do campo Estado na Entrada permanece questão funcional genuína (ver §16). Está fechado que o Armazém não altera silenciosamente o estado técnico.

### 8.11 Saída / Retirar

**Validações:**

- Ferramenta deve existir.
- Ferramenta deve estar registada como presente no Armazém.
- Destino operacional: suporte funcional a Fabricação / Reparação / Sucata.
- A obrigatoriedade do Destino em todas as Saídas permanece questão funcional genuína (ver §16).

**Efeito funcional.** A Saída: cria movimento de saída; liberta a ocupação ativa; regista a data/hora e o executor da libertação, apenas se a posição ainda não estiver libertada; só liberta a posição após persistência com sucesso; regista o evento de auditoria.

**Destino operacional** — Fabricação; Reparação; Sucata. Responde a "para onde vai a ferramenta?" e não é uma transição automática de estado técnico.

**Fabricação.** A ferramenta vai operacionalmente para produção/fabricação; o Armazém regista o movimento físico e o destino; "Em produção" ou "Em fabrico" é contexto operacional/físico, não estado técnico; não altera automaticamente o estado técnico. Se houver necessidade de alterar estado técnico, isso pertence ao workflow de Ferramentas.

**Reparação.** O utilizador deve poder selecionar o reparador, a partir do **diretório canónico de reparadores registados**; se o reparador necessário não existir, deve ser possível registá-lo/adicioná-lo na fonte canónica; não deve ser texto livre arbitrário no movimento; o reparador selecionado fica associado ao movimento físico e permanece historicamente rastreável.

- `Saída → Reparação` **não muda automaticamente** o estado técnico para "Por reparar" nem para "Reparado".
- A seleção de reparador na Saída → Reparação é dado do movimento físico/ciclo, não edição de master da ferramenta.
- O ciclo/registo de reparação pertence à Reparação.
- Na Reparação Interna, o reparador é o utilizador autenticado; não é selecionado manualmente.

**Sucata.** A ferramenta vai operacionalmente para sucata; o Armazém regista o movimento físico e o destino operacional; o destino **não implica automaticamente** estado técnico "Sucatado". Se o estado técnico "Sucatado" tiver de ser atribuído, isso ocorre no workflow próprio de Ferramentas, nunca por mutação silenciosa do movimento de Armazém.

### 8.12 Saídas Programadas

As **Saídas Programadas existem funcionalmente**. A sua existência funcional está fechada; a ativação completa da superfície/UI pode ser escopo de implementação.

**Trigger:** um utilizador autorizado no módulo de Reparação cria a lista de Saída programada; o Armazém recebe a lista como pendente; o operador do Armazém executa a recolha/saída física; a criação/seleção dos lotes não é responsabilidade do Armazém.

**Estados operacionais da lista** (sem alterar estados técnicos): Pendente de saída; Em reparação; Retorno parcial; Concluída.

**Regras funcionais:**

- checkboxes são confirmação de recolha, não seleção;
- receber, abrir ou imprimir a lista não cria Saídas nem liberta posições;
- a posição só muda quando a fase de Saída é fechada pelo último check e persistida;
- o fecho deve ser atómico: falha ⇒ nenhuma posição libertada; lista permanece pendente;
- entrada de retorno fecha a linha;
- a lista só fica Concluída quando todas as linhas tiverem Entrada.

**Posição na criação vs posição atual.** Se a posição atual diferir do snapshot existente na criação da lista: mostrar as duas; apresentar alerta; não corrigir/substituir silenciosamente o snapshot.

**Reparador no fluxo programado.** O reparador é definido na Reparação, na lista de envio; deve ser preservado como facto histórico (snapshot do reparador); a recolha no Armazém confirma apenas o movimento físico; o reparador permanece rastreável.

**Impressão.** A impressão é opcional; nunca é condição para a lista ficar disponível; não altera estado da lista nem das posições.

O workflow de saída programada partilha com a Reparação um mecanismo de confirmação física atómica (recolha/retorno) gerido pelo Armazém.

### 8.13 Correção de localização

Requisito funcional confirmado. Quando o operador encontra uma diferença física (ferramenta numa posição mas não registada, ou registada mas não fisicamente presente), pode abrir **Corrigir localização**, separada de uma Entrada normal, mostrando valores registados vs encontrados.

Regras: a correção não reescreve silenciosamente movimentos anteriores; usa mecanismo auditável (nova linha/facto); preserva histórico; distingue-se da edição de master do Responsável.

### 8.14 + Criar novo

O workflow **+ Criar novo** pode ser iniciado a partir do Armazém:

- permite criar uma nova ferramenta a partir do contexto Armazém;
- a criação do master record pertence a **Ferramentas**;
- depois da criação do master, ocorre Entrada física no **Armazém**;
- não duplicar master.

Fluxo: (1) criação do master da ferramenta → Ferramentas; (2) registo de Entrada física → Armazém. A UI pode começar no Armazém, mas o ownership do master permanece em Ferramentas.

### 8.15 Consulta, pesquisa e filtros

Pesquisa suportada por: tipo; referência; lote; posição. A chamada exige pelo menos um critério.

- Por posição: devolve ocupante(s) e pode mostrar aviso de conflito de referências.
- Por tipo/referência/lote: pesquisa identidades e devolve estado de localização.

**Resultado mínimo:** Tipo; Referência; Nome técnico; Lote; Localização/contexto; Posição; Último movimento/contexto relevante. Quando a ferramenta não está no Armazém, a posição atual aparece como `—` ou equivalente; a posição anterior permanece no histórico.

**Filtros:** Tipo (CM/MF/BQ); Localização/contexto; Posição; intervalo de datas do movimento; apenas com alertas. Não duplicar filtros pertencentes a outros domínios (vida útil; estado técnico; máquina/linha; reparador).

**Interação na lista:** clique seleciona; duplo clique abre histórico de localização; filtros nunca selecionam automaticamente um resultado.

**Colunas read-only.** Vida útil e estado técnico podem aparecer como colunas read-only, com origem no domínio Ferramentas. Não são dados próprios do Armazém; não são filtros próprios do Armazém em V1. Percentagem de uso pode ser coluna read-only; edição de % uso ocorre apenas na ficha da ferramenta.

**Deep-link.** Permite abrir a Consulta já filtrada por posição (ex. a partir de alertas).

### 8.16 Estados, destinos, validações e avisos

**Estado físico / paradeiro.** Pertence ao Armazém e é derivado dos factos/movimentos de localização: `armazem` (presente); `fora` (fora do Armazém); `nao_registado` (localização operacional não registada); destinos operacionais Fabricação, Reparação, Sucata. "Em produção"/"Em fabrico" é estado operacional/físico, não estado técnico.

**Destino operacional.** Responde "para onde vai a ferramenta?". Destinos fechados: Fabricação; Reparação; Sucata. É registado no movimento/histórico; não é estado técnico; não altera silenciosamente o master; pode ser obrigatório ou opcional conforme decisão ainda em aberto.

**Estado técnico.** Pertence a Ferramentas (Novo; Reparado; Por reparar; Sucatado; Arquivado). O Armazém pode mostrá-lo como contexto read-only, mas não cria, não altera, não recalcula, não sincroniza silenciosamente.

**Estado na Entrada.** O campo Estado na superfície de Entrada — Reparado | Por reparar | Novo — está fechado em que o Armazém não cria/altera/recalcula estados técnicos e um movimento de Armazém não pode mutar silenciosamente o estado técnico. A classificação exata do campo Estado na Entrada permanece em aberto (§16).

**Validações:**

| Mensagem/Significado | Classificação |
|---|---|
| Posição deve ter exatamente 4 dígitos | Validação estrutural |
| Indique referência ou lote | Validação estrutural |
| Ferramenta não encontrada; verifique referência e lote | Validação estrutural / não encontrado |
| Posição já está ocupada por outra ferramenta | Bloqueio duro de negócio/estrutural — ocupação 1:1 |
| Ferramenta não está registada como presente no Armazém | Bloqueio estrutural/negócio |
| Indique tipo/referência/lote/posição | Validação estrutural |
| Ferramenta não registada como presente para ser libertada | Bloqueio estrutural/negócio |
| Posição desta ferramenta já foi libertada | Bloqueio estrutural/negócio |
| Posição de retorno deve ter 4 dígitos | Validação estrutural |
| Posição de retorno já está ocupada por outra ferramenta | Bloqueio duro de negócio/estrutural |
| Módulo não autorizado | Estrutural — fail closed |

**Avisos** (não são bloqueios automáticos de produção):

- **Conflito de referências na mesma posição** — duas referências diferentes ativas na mesma posição geram aviso de qualidade de dados; nunca normalização silenciosa.
- **Localização operacional não registada** — ferramenta sem contexto operacional válido (nem posição ativa, nem Fabricação ativa, nem Reparação ativa); o Armazém sinaliza; não inventa estado nem cria movimento.
- **Ferramenta em mais de um contexto** — mostrar conflito e encaminhar para correção humana; não aplicar prioridade automática.
- **Atualização de % uso pendente** — quando uma ferramenta sai de produção e recebe Entrada no Armazém com flag de uso pendente, pode ser apresentado alerta idempotente; abrir o alerta, consultar SAP ou entrar no Armazém não limpa a flag; só a gravação de nova % uso na ficha da ferramenta limpa; % uso não é editada no Armazém.

Regra transversal: warning ≠ decisão automática de produção; o Armazém aplica bloqueios estruturais duros; não inventar bloqueios de negócio adicionais.

### 8.17 Informação introduzida pelo utilizador

**Entrada:** Posição (4 dígitos); Referência; Lote; Observações; Tipo (CM/MF/BQ). No modelo de dados, a Entrada pode também apresentar Máquina e Estado (ver §16 para a classificação do Estado). Destino não é pedido na Entrada.

**Saída:** Referência; Lote; Destino operacional (Fabricação / Reparação / Sucata); Reparador, quando Destino = Reparação, selecionado do diretório canónico; Observações.

**Consulta:** tipo; referência; lote; posição.

### 8.18 Informação gerada ou derivada pelo sistema

Não deve ser tratada como entrada manual: ID estável da ferramenta/lote, resolvido; ator (executor), derivado do utilizador autenticado; data/hora (timestamps) das operações; contexto de localização `armazem`/`fora`/`nao_registado`; o facto "fora", derivado; a proveniência de reparação, quando aplicável; o conflito de referências, derivado; o último reparador, derivado dos factos históricos; disponibilidade/presença derivadas para o Job On (planeamento); flags/alertas, quando suportadas.

O reparador selecionado é resolvido a partir da escolha do utilizador e pode ser preservado como snapshot no histórico.

### 8.19 Quantidades / stock / saldos

O modelo de stock do Armazém é por **identidade individual de ferramenta/lote**, não por quantidade/saldo agregado:

- a ocupação é um facto posição ↔ lote;
- uma linha ativa representa uma posição ocupada por um lote;
- não representa quantidade de stock livre;
- a quantidade registada no movimento não é usada como saldo;
- "fora" é derivado e nunca é saldo contabilístico.

O Armazém não calcula saldos de Tampões (Tampões), saldo do fluxo de reparação de BQ (Boquilhas) nem quantidade planeada de produção (Job On). Portanto:

- quantidade de Armazém ≠ saldo Boquilhas;
- quantidade de Armazém ≠ saldo Tampões;
- quantidade de Armazém ≠ quantidade planeada Job On.

### 8.20 Histórico e auditoria

O histórico do Armazém guarda apenas localização/movimentos da ferramenta/lote: Entrada/Saída; posição; destino/origem (Fabricação / Reparação / Sucata); reparador quando Destino = Reparação; observações; data/hora; operador.

**Append-only.** Movimentos são factos imutáveis; não podem ser alterados nem eliminados; correções não apagam movimentos anteriores; correção de localização usa mecanismo auditável.

**Saídas programadas no histórico.** A Saída programada entra no histórico de movimentos apenas quando a fase de recolha/Saída é fechada pelo último check e persistida. A criação/impressão da lista pertence ao histórico operacional da lista, não ao histórico de localização da ferramenta.

**Reparador no histórico.** O histórico de uma ferramenta deve preservar, por ciclo: ferramenta/lote; reparador; data/hora; contexto/movimento de reparação. Ordem cronológica. Reparações/históricos anteriores nunca são sobrescritos por uma reparação nova. O "último reparador" é determinado a partir dos factos históricos (movimento/ciclo mais recente com reparador); não é um campo mutável mantido como única verdade.

**O que não repetir no histórico do Armazém:** ciclo de reparação completo; vida útil; alterações de estado técnico; arquivo/sucata; histórico de produção.

**Auditoria transversal.** O Armazém regista eventos de auditoria para as suas operações (entrada, saída, correção, saída programada concluída). História lê estes eventos read-only; não se torna owner dos movimentos do Armazém.

Ator derivado do utilizador autenticado (server-derived). Timestamps em UTC.

### 8.21 Documentos / impressão / exportação

**Picking / Saída programada.** A impressão da lista de recolha é opcional.

Conteúdo funcional referido: identificação da saída programada; data de criação; Tipo; Referência; lote; posição; espaço de confirmação física; observação.

Regras: imprimir não altera o estado da lista; imprimir não liberta posições; o fluxo deve ser executável integralmente no computador sem impressão.

**Etiquetas.** Etiquetas/labels de posição: **não presentes** na V1.

**Exportação.** Exportação PDF/CSV de stock/movimentos: **não presente** na V1.

**Não inventar** funcionalidades não evidenciadas: barcodes/QR; gestão de pallets; listas de picking avançadas; PDF próprio do Armazém fora dos casos evidenciados.

### 8.22 Relações com outros módulos

**Ferramentas.** Divisão de ownership já descrita em §8.3 e §7.11. Estado técnico e localização são independentes: a ferramenta pode estar armazenada enquanto o master continua editável; uma mudança de posição não muda o master; uma edição de master não muda a posição. `Saída → Reparação/Sucata` não altera automaticamente estado técnico. O reparador selecionado é dado do movimento físico, não edição de master. O Armazém guarda o ID estável do lote e não cria identidades paralelas.

**Job On.** Produção/planeamento = Job On; localização física = Armazém. O Armazém fornece contexto de localização/presença/disponibilidade para planeamento.

- Selecionar/associar ferramenta no Job On NÃO cria movimento de Armazém, NÃO cria reserva e NÃO infere movimento físico.
- O filtro da lista de ferramentas no Job On baseia-se em Referência, Máquina/Linha e tool/lot registado; o Responsável faz a seleção final.
- Posição/localização atual é live, proveniente do Armazém; o snapshot histórico/revisão do Job On é guardado para produção. Em modo consulta, a folha do Job On mostra apenas a associação guardada. A impressão do Job On nunca consulta dados live para substituir valores do snapshot.
- O Job On pode mostrar o último reparador da ferramenta/lote como contexto read-only; não possui nem edita histórico.
- O Armazém não fornece ao Job On PU, CS, TP, Pinças ou Calibres.
- "Em produção" é contexto/paradeiro operacional do Armazém, não estado técnico de Ferramentas.

**Boquilhas.** Ownership fechado: BQ master owner = Ferramentas; BQ localização/movimentos físicos normais = Armazém; BQ external repair movement/history workflow = Boquilhas; BQ Reparação Interna = não. Funcionalmente, BQ usa o modelo normal de Armazém (localização, movimentos, entrada/saída); a diferença funcional relevante é o fluxo de reparação externa. Ferramentas = BQ master; Boquilhas = movimentos de reparação externa de BQ, saldos e histórico próprio; Armazém = localização/movimento físico. Boquilhas não é um módulo de Armazém.

**Reparação Interna / Externa.** Reparação possui o registo/workflow de reparação interna e externa; Armazém possui o movimento físico/localização.

- Enviar ferramenta para reparação não cria movimento físico automaticamente; só cria/remove/altera localização do Armazém através de confirmação física explícita.
- Em `Saída → Reparação externa`, o reparador é selecionado do diretório canónico e permanece no histórico da ferramenta (ferramenta/lote · reparador · data/hora · ciclo), em ordem cronológica, sem sobrescrita; o último reparador é derivado dos factos históricos.
- Na Reparação Interna, o reparador é o utilizador autenticado; não é selecionado manualmente. A seleção manual de reparador tratada na Saída → Reparação do Armazém diz respeito ao fluxo de reparação externa CM/MF.
- Estado de reparação não muda automaticamente a partir de movimento de Armazém. Retorno de reparação cria Entrada/reocupação física e movimento de entrada, com proveniência de reparação quando aplicável.
- Qualquer confirmação que mude simultaneamente o estado do ciclo de reparação e o estado físico corre numa única operação atómica, através do mecanismo de confirmação física do Armazém.
- Saída programada: criada na Reparação por um utilizador autorizado; executada no Armazém pelo Operador; fecho atómico; retorno parcial → estado Retorno parcial; só Concluída quando todas as linhas tiverem Entrada.

**Tampões.** Tampões é independente do modelo de Armazém: possui saldos, movimentos e configurações próprios; não reutiliza o modelo de warehouse; não tem posições/lotação no Armazém. Regra transversal: planear ≠ reservar.

**História.** A História é uma superfície transversal de leitura — não é um módulo funcional atribuível. Lê eventos de auditoria read-only; não possui eventos; mostra apenas eventos dos módulos concedidos ao utilizador; eventos administrativos podem exigir permissão específica de auditoria. O Armazém gera eventos de auditoria, mas a História não se torna owner dos movimentos do Armazém. Distinguir **História** (leitura transversal de auditoria) de **Histórico do Armazém** (factos append-only de localização/movimento).

### 8.23 Regras negativas — ARMAZÉM NÃO...

**Master e ownership**

- O Armazém **não é owner** do master da ferramenta.
- O Armazém pode expor dados master e permitir abrir a ficha da ferramenta.
- Quando um Responsável autorizado edita essa ficha, a operação e persistência permanecem em **Ferramentas**.
- UI entry point não transfere ownership.
- Operador não ganha permissão irrestrita de edição master apenas porque o detalhe está visível.

**Estado técnico**

- Movimento de Armazém **não altera automaticamente** estado técnico.
- `Saída → Reparação` **não muda** automaticamente para "Por reparar" nem para "Reparado".
- `Saída → Sucata` **não muda** automaticamente para "Sucatado".
- "Em produção", "Em reparação" ou "Em fabrico" são contextos operacionais/físicos, não estados técnicos.

**Reparador**

- A seleção de reparador na `Saída → Reparação` **não é edição de master**.
- Reparador é dado do movimento físico/ciclo; o ciclo/registo de reparação pertence à Reparação.
- Reparadores anteriores **não são sobrescritos**; o último reparador é derivado dos factos históricos.

**Job On**

- Selecionar ferramenta no Job On **não cria movimento** nem **reserva**.
- Job On **não cria/edita/possui** histórico de reparação.

**Reparação**

- Registo de reparação **não se torna registo de warehouse**.
- Enviar para reparação **não cria movimento físico automaticamente**; só confirmação explícita persistida move o estado físico.

**Produção**

- Localização do Armazém **não é atribuição de produção**.
- Armazém **não calcula quantidade planeada** de produção.

**BQ / Boquilhas**

- Armazém **não possui** BQ master nem movimentos de reparação externa de BQ.

**Histórico**

- Localização histórica **não é sobrescrita/apagada**.
- Movimentos são append-only; posição atual não apaga histórico.
- Correção **não reescreve silenciosamente** movimentos anteriores.

**Substituir**

- O alvo funcional atual **não inclui** a ação normal Substituir.

**Normalização silenciosa**

- Duas referências na mesma posição geram aviso.
- Nunca fusão/substituição automática.

**Invenção de dados**

- Armazém não inventa estado, condição, reparador ou localização.
- Armazém não cria movimento automaticamente ao sinalizar inconsistência.
- `fora` é derivado, nunca persistido.

---

<a id="p2-boquilhas"></a>
## 9. Boquilhas

**Âmbito:** Boquilhas existe apenas para registar os movimentos relacionados com a **reparação externa de BQ**.

### 9.1 Objetivo

Boquilhas é o módulo onde a fábrica regista, diariamente e a alta frequência, o fluxo de movimentos de reparação externa das boquilhas (BQ): quais os lotes de BQ que saem para reparação, com que reparador, e quais os que voltam, com as quantidades envolvidas e o histórico desses movimentos.

**O que é rastreado (apenas relacionado com a reparação):**

- o lote de BQ envolvido no movimento de reparação (referência + lote);
- a saída para reparação (lote enviado);
- o reparador selecionado/associado;
- a associação reparador ↔ máquina/linha quando aplicável;
- o retorno/entrada de reparação, com reconciliação (incluindo retorno a mais);
- as quantidades envolvidas nesses movimentos de reparação;
- o histórico desses movimentos de reparação por lote e de forma transversal.

**Natureza — registo operacional MANUAL:** o operador introduz manualmente o movimento; a aplicação guarda a informação no servidor; o movimento permanece no histórico; a aplicação apresenta o saldo/diferença resultante. O módulo não decide operacionalmente por si — regista e mostra o que aconteceu.

**Porque a quantidade/histórico importam:** o envio/retorno de reparação é de alta frequência e por quantidade. É preciso lançar um envio para reparação, receber de volta e perceber quando voltou mais do que o esperado (discrepância) sem nunca bloquear a operação nem inventar quantidades.

### 9.2 Âmbito funcional

- **FERRAMENTAS** = BQ tool/master, identidade, lote, estado técnico e dados normais de ferramenta.
- **ARMAZÉM** = localização física geral e movimentos normais de armazém.
- **BOQUILHAS** = apenas o fluxo de movimentos de reparação externa de BQ (diário/contínuo): saída para reparação; lote de BQ enviado; reparador selecionado/associado; associação reparador ↔ máquina/linha quando aplicável; retorno/entrada de reparação; quantidades envolvidas; histórico.

**Regra de paragem (não alargar):** Boquilhas não trata de BQ como ferramenta geral (Ferramentas), não trata da localização física geral de BQ (Armazém), não gere o ciclo de vida geral da BQ nem o master de BQ, e não gere movimentos gerais de armazém. BQ comporta-se como CM/MF no modelo normal de ferramenta/master/armazém; a única diferença funcional relevante é o fluxo de reparação.

**Fechado:** `BQ NUNCA REPARAÇÃO INTERNA`.

### 9.3 Posição na aplicação

- Módulo funcional de topo, atribuível individualmente a cada utilizador.
- Quando atribuído, aparece na navegação e pode ser aberto; quando não atribuído, não aparece e não está funcionalmente acessível.
- Os operadores arrancam no Job On (landing operacional) e entram em Boquilhas quando precisam de registar os movimentos de reparação externa de BQ.
- O trabalho genérico de BQ como ferramenta (master, localização física) pertence a Ferramentas/Armazém.
- Dentro de Boquilhas existem áreas internas (tabs): Registo, Boquilhas, Histórico e Definições, mais um painel lateral fixo de linhas de produção. Nenhuma destas é um módulo separado.

### 9.4 Utilizadores / perfis / acesso

Modelo global com exatamente três perfis: Admin, Operador / Controlador e Responsável.

- **Acesso:** controlado por atribuição individual do módulo ao utilizador, definida no Admin. Sem o módulo → sem acesso (não aparece, não funciona).
- **Admin:** não é perfil operacional de Boquilhas. Pode atribuir/gerir o módulo, mas não opera Boquilhas.
- **Operador / Controlador e Responsável (Q1 fechada):** comportamento funcional **sem diferença**. Em Boquilhas, `OPERADOR / CONTROLADOR = RESPONSÁVEL` quanto às ações operacionais dentro do módulo: se qualquer um dos perfis tiver Boquilhas atribuído, ambos têm exatamente as mesmas ações de Boquilhas.
  - Não existe variante Operador específica de Boquilhas; não existe variante Responsável específica; não existe workflow de aprovação/revisão apenas por o utilizador ser Responsável; nenhuma ação funcional fica escondida do Operador mas exposta ao Responsável; o perfil não altera a experiência operacional de Boquilhas.
  - O único gate é: `BOQUILHAS MODULE ASSIGNED?` Se sim e o perfil for Operador / Controlador ou Responsável → mesmas operações funcionais.
  - A manutenção de registos BQ/Lote já existentes no Armazém pelo Responsável é uma regra do módulo Armazém (Q4) e não cria qualquer distinção de perfil dentro de Boquilhas.

### 9.5 Estrutura interna

| Área | O que o utilizador faz ali | Classificação |
|---|---|---|
| **Registo** | Pesquisa um lote de BQ (por referência/BQ, lote e/ou máquina/linha); se existir, seleciona e abre o fluxo de reparação; se não existir, cria a identificação em falta (referência + lote + máquina(s)/linha(s) registadas) e continua de imediato para o registo; regista os movimentos de reparação (Saída para reparação, Entrada/retorno de reparação, Não reparadas, Corrigir contagem, Editar ficheiro — correção do registo do fluxo de reparação, Fechar); vê o estado atual e os movimentos desse lote (lista paginada com tipo, quantidade, saldo, data e operador). | Área interna (tab) |
| **Boquilhas** | Vista de grelha/cartões de lotes, com filtros (referência/lote/linha, estado, linhas por página). "arquivados" mantém-se como resultado do fecho do registo de reparação; "sucata" como lifecycle genérico da ferramenta é design superseded. Os cartões mostram quantidade, linha/localização/reparador e percentagem de vida utilizada; a percentagem representa tempo de vida/desgaste, não quantidade; nunca como barra de progresso. Os totais `Na fábrica`, `Em reparação` e `Em produção` não pertencem a esta página — passam para o Histórico. | Área interna (tab) |
| **Histórico** | Vista transversal/agregada dos movimentos de reparação: calendário à esquerda, cartões de resumo à direita, filtros em largura total e tabela de movimentos em largura total (colunas: Referência, Lote, Movimento, Quantidade, Saldo, Reparador, Linha, Data e hora, Operador — sem colunas Detalhe nem Ficheiro). Uma linha selecionada ativa `Corrigir movimento` e `Eliminar movimento`; duplo clique abre o lote no Registo. | Área interna (tab) |
| **Definições** | Configuração de reparadores: tabela compacta com uma linha por B1–C3 (reparador predefinido e reparadores permitidos por linha); o predefinido é sugerido ao criar uma saída, mas pode ser alterado no movimento; `Sem associação` é permitido e visível; secção separada com a lista de reparadores (desativar, não eliminar — preserva histórico); se o predefinido for desativado, a linha exige nova associação. | Área interna / aba isolada (configuração) |
| **Linhas de produção (painel lateral)** | Leitura rápida das linhas B1–C3 e do que cada linha está a produzir (cartão com linha, referência, lote, quantidade e hora de início quando aplicável); clicar na referência de produção completa abre o Job On ativo associado àquela referência/linha; clicar no restante cartão abre o lote no Registo; menu `…` com `Substituir`/`Remover` (linha ocupada) e `Adicionar` (linha livre); alerta de conflito quando há referências diferentes na mesma linha; ligação ao Job On. | Área interna fixa (contexto) |

- Tab Fabrico → não existe (regra explícita).
- O Registo mostra o lote selecionado e os seus movimentos; o Histórico é a vista geral. Ambas usam a mesma fonte de dados; muda apenas o âmbito da consulta.
- As tabs/estruturas de apoio (lote, linhas, config de reparadores) existem para suportar o fluxo de movimentos de reparação externa de BQ; não implicam que Boquilhas possua o master de BQ nem a localização física geral de BQ.

### 9.6 Modelo funcional central

O ciclo operacional do fluxo de reparação de BQ:

1. O utilizador entra em Boquilhas. Vê o painel lateral com as linhas B1–C3 e, quando relevante, o contexto de produção de cada linha (lido do Job On).
2. Seleciona ou cria o lote de BQ que vai a reparação — `EXISTE → SELECIONA` / `NÃO EXISTE → CRIA`. A pesquisa encontra o lote por referência/BQ, lote e/ou máquina/linha. Se o lote existir, é selecionado e aberto; se não existir, a identificação em falta é criada (referência + lote + máquina(s)/linha(s)) e o utilizador continua de imediato para o registo. A criação em falta não é bloqueada por o master completo de Ferramentas ainda não estar preenchido, e não transfere a posse do master (Ferramentas continua dono).
3. Regista os movimentos que vão acontecendo: Saída (envia o lote de BQ para reparação, indicando o reparador), Entrada (recebe de volta, com reconciliação), Não reparadas (declara irrecuperável), Corrigir contagem (acerta a quantidade), Fechar (encerra o registo).
4. Cada movimento altera o saldo de reparação e fica guardado no histórico.
5. Quando um retorno é maior do que o esperado, o utilizador recebe um aviso e o sistema não bloqueia: regista o retorno completo e abre uma discrepância (entrada excecional) para ser tratada depois.
6. Quando o fluxo do lote termina, o utilizador fecha; o sistema guarda um snapshot final imutável do resumo/estado e o lote passa para o histórico/arquivados.

**Princípio operacional — registo manual (preservado):** o operador introduz manualmente cada movimento; a aplicação guarda a informação no servidor; o movimento permanece no histórico; a aplicação apresenta o saldo/diferença resultante. O módulo não toma decisões operacionais automáticas: não bloqueia, não aprova, não corrige automaticamente, não esconde nem reescreve o que foi registado. Tudo o que é registado fica visível no lote e no Histórico — incluindo o saldo/diferença de cada movimento.

**Em todo o tempo:**

- o saldo de reparação é calculado a partir dos movimentos de reparação (não é introduzido à mão);
- os movimentos históricos não são reescritos; correções criam registos novos;
- o módulo emite confirmações/histórico para o utilizador e para o sistema de História transversal;
- nenhuma ação de Boquilhas altera a produção do Job On, a localização física do Armazém, nem o master de Ferramentas.

### 9.7 Identificação BQ / lote

- **Identidade do lote BQ** = `REFERÊNCIA + LOTE` (ex.: `T173` · `Lote 5`).
- **`MÁQUINA/LINHA(S)`** = contexto operacional registado, obrigatório (≥ 1), MULTI-VALOR (B1–C3; seleção múltipla; `B1 + C3` é válido).
- **NÃO existe identidade composta** `REFERÊNCIA + LOTE + MÁQUINA`.
- `T173 · Lot 5 · [B1, C3]` = UM lote BQ registado para trabalhar em B1 e C3 — não são dois lotes, nem dois registos master, nem duas identidades.
- **Referência:** formato `^[A-Z][0-9]{3}$` (ex.: T173). (1 letra + 3 dígitos).
- **Lote:** formato livre compacto; parte da identidade do lote.
- Cada lote BQ deve ter o seu contexto de máquina/linha válido registado.

### 9.8 Seleção / criação

- **`EXISTE → SELECIONA`** — pesquisa por referência/BQ, lote e/ou máquina/linha registada; se existir, seleciona e abre o fluxo de reparação. A pesquisa não se reduz a apenas referência nem a apenas lote (o contexto de máquina/linha registado no lote pode servir de filtro). Sem resultado: `Nenhuma boquilha encontrada`.
- **`NÃO EXISTE → CRIA → CONTINUA`** — se não existir, o utilizador cria a identificação em falta e continua de imediato para o registo do fluxo de reparação. A criação é inline, na própria página Registo (não abre nova página nem modal); o botão muda de `Criar novo lote` para `Fechar criação` enquanto o painel estiver aberto.
- A ausência de um master completo de Ferramentas não bloqueia o trabalho diário; não existe onboarding, wizard de migração, workflow de reconciliação, aprovação separada nem pré-requisito de importação de master.
- Criar a identificação em falta NÃO transfere a posse do master — Ferramentas permanece dono master.
- **Campos de criação (ordem documentada):** 1) Boquilha/Referência (obrigatória) → 2) Lote (obrigatório, compacto) → 3) Máquina(s)/Linha(s) (escolha múltipla B1–C3, pelo menos uma obrigatória; `B1 + C3` válido) → 4) Total do lote (quantidade inicial do registo do fluxo de reparação — não é o stock físico do Armazém) → 5) Utilização inicial (`%`, valor manual).

### 9.9 Fluxo de reparação

O fluxo de reparação é o ciclo de envio/retorno de BQ para reparação externa. Movimentos documentados:

- **Saída para reparação** — envia o lote de BQ para reparação, indicando o reparador;
- **Entrada / retorno de reparação** — recebe de volta, com reconciliação; pode incluir retorno a mais;
- **Não reparadas** — declara a quantidade irrecuperável;
- **Corrigir contagem** — acerta a quantidade;
- **Editar ficheiro** — limitado à correção do registo do fluxo de reparação (notas/movimentos), não à edição master de BQ;
- **Fechar** — encerra o registo do fluxo de reparação.

### 9.10 Movimentos documentados / quantidades / saldos

Modelo de movimentos:

- Entrada/Saída com **Data, Quantidade, Motivo, Detalhe, Observações**;
- Motivos: `Movimento normal`, `Movimento anterior não registado`, `Correção operacional`, `Outro`;
- o placeholder de Detalhe depende do Motivo: Normal → `Opcional`; Movimento anterior → `Ex.: saída de 12 BQ em 12/08`; Correção → `Ex.: quantidade registada incorretamente`; Outro → `Indique brevemente a razão`;
- **coluna `Saldo`** = saídas menos entradas; negativos a vermelho; derivada dos movimentos (nunca introduzida à mão).

Regras de saldo:

- o saldo negativo (ex.: SAÍDA 10; ENTRADA 15 → SALDO −5 a vermelho) é uma diferença apresentada na coluna `Saldo`; o movimento é registado, a quantidade real é preservada, o histórico mostra o que aconteceu e o valor negativo aparece a vermelho — sem bloqueio, sem rejeição, sem workflow obrigatório e sem discrepância obrigatória.

Retorno a mais — o caso "20→25":

- é a exceção central do fluxo de reparação; o retorno é aceite na íntegra, reconciliado com o esperado, e o excesso é registado como entrada excecional com discrepância aberta;
- nunca bloqueia; nunca soma automaticamente o excesso;
- é um mecanismo separado do saldo negativo (o saldo negativo por si só não cria nem exige discrepância).

Snapshot final:

- ao fechar, o sistema guarda um snapshot final imutável do resumo/estado; um fecho falhado mantém o registo ativo sem snapshot parcial apresentado como válido;
- só o último fechado e sem outro ativo pode ser reaberto.

Outros comportamentos:

- **Referências diferentes na mesma linha:** alerta de conflito no painel lateral (requer remover/substituir uma referência); vários lotes da mesma referência na mesma linha são permitidos sem alerta; o cartão em conflito não abre lote.
- **Vida/desgaste:** é tempo de vida, não quantidade; valor perto do limite é aviso, não bloqueio.
- **Sem integração SAP automática:** a vida é manual; não há leitura/escrita SAP automática.
- **Não apresentar o indicador `Contagem reconciliada`** (design: não apresentar).

### 9.11 Reparadores

- Configuração de reparadores por linha B1–C3 (reparador predefinido e reparadores permitidos por linha).
- O predefinido é sugerido ao criar uma saída, mas pode ser alterado no movimento.
- `Sem associação` é permitido e visível.
- Desativar reparadores, não eliminar (preserva histórico); se o predefinido for desativado, a linha exige nova associação.

### 9.12 Máquina / linha

- Máquina/linha é escolha múltipla (B1–C3); pelo menos uma deve ser selecionada na criação em falta.
- A associação reparador ↔ máquina/linha é configurável.
- O contexto de máquina/linha registado no lote pode servir de filtro na pesquisa.
- Não inferir compatibilidade de máquina a partir da referência.

### 9.13 Utilização %

- `% UTILIZAÇÃO = MANUAL VALUE` (Q2): o valor é introduzido/atualizado manualmente por um utilizador.
- O sistema nunca calcula, incrementa, deriva, sincroniza nem atualiza `% utilização` automaticamente.
- A atualização **manual** de `% utilização` pode ser realizada pelo perfil **RESPONSÁVEL** na superfície de manutenção do **Armazém** (registo existente), apenas onde a característica estiver exposta/confirmada como editável — sem automatização de escrita, sem cálculo automático e sem transferência de posse (Ferramentas permanece o domínio master).
- `MASTER OWNERSHIP ≠ AUTOMATIC VALUE` e `MASTER OWNERSHIP ≠ READ-ONLY IN EVERY OPERATIONAL SURFACE`.
- O reminder/alarme da transição Produção → Armazém NÃO altera o valor de `% utilização`; o trigger do reminder não transfere posse.

### 9.14 Data de abertura

`Data de abertura` é um campo de data normal e EDITÁVEL na UI de criação do Registo (Q3).

### 9.15 Histórico / auditoria

- Os movimentos de reparação são append-only e o histórico não é reescrito.
- `Corrigir movimento` cria um novo movimento; `Eliminar movimento` no Histórico é uma anulação registada (com confirmação), nunca remoção física.
- Os movimentos anteriores não são reescritos.
- O módulo emite confirmações/histórico para o utilizador e para o sistema de História transversal.
- A **História** apenas lê eventos; não é dona dos registos operacionais de Boquilhas.

### 9.16 Ownership

- **BQ tool/master, identidade, lote, estado técnico e dados normais de ferramenta** → Ferramentas.
- **Localização física geral e movimentos normais de armazém de BQ** → Armazém.
- **Fluxo de movimentos de reparação externa de BQ** → Boquilhas.
- **BQ nunca Reparação Interna.**
- **História** → leitura de eventos.

### 9.17 Relações com outros módulos

- **Ferramentas:** dono do master BQ, identidade, lote, estado técnico e dados normais de ferramenta. Boquilhas regista apenas os movimentos de reparação.
- **Armazém:** BQ comporta-se como CM/MF no modelo normal de ferramenta/master/armazém; Boquilhas não move stock físico nem altera a verdade física do Armazém.
- **Job On:** Job On é dono da produção/planeamento/revisões/contexto e da seleção de ferramenta. BQ é uma ferramenta principal selecionada pelo RESPONSÁVEL no Job On. Boquilhas NÃO altera o Job On. Selecionar BQ+lote no Job On NÃO cria automaticamente um movimento de Boquilhas. Boquilhas NÃO decide qual BQ vai à produção. Filtragem por máquina/linha registada. Não existe consulta ao vivo do Job On a partir de Boquilhas: a integração usa snapshots imutáveis do Job On/BQ. Consumo de contexto: Boquilhas mostra no painel lateral o estado das linhas. O lote BQ usado numa produção é guardado como snapshot/contexto histórico.
- **Controlo:** usa o lote de BQ exato que foi selecionado no Job On para a produção, como contexto/snapshot. Boquilhas não fornece ao Controlo valores técnicos/volume. Um resultado de Controlo (ex. NOK/aviso) NÃO altera automaticamente o estado ou saldo de Boquilhas.
- **Reparação Externa:** a Reparação Externa gere os batches de reparação externa CM/MF. O fluxo de reparação externa desenhado para BQ está adiado (não ativo no modelo atual): a BQ não entra no processo de batch externo CM/MF da Reparação Externa.
- **Reparação Interna:** `BQ NUNCA REPARAÇÃO INTERNA`. A RI repara CM e MF apenas.
- **História:** leitura transversal dos eventos de auditoria; não é dona dos registos operacionais de Boquilhas.

### 9.18 Casos especiais

- **Voltou mais do que o esperado (o caso "20→25"):** retorno aceite na íntegra, reconciliado com o esperado, excesso registado como entrada excecional com discrepância aberta; nunca bloqueia; nunca soma automaticamente o excesso; mecanismo separado do saldo negativo.
- **Saldo negativo:** diferença na coluna `Saldo`; sem bloqueio, sem rejeição, sem workflow obrigatório e sem discrepância obrigatória.
- **Referências diferentes na mesma linha:** alerta de conflito no painel lateral; vários lotes da mesma referência na mesma linha são permitidos sem alerta; o cartão em conflito não abre lote.
- **Fecho falhado:** o registo permanece ativo, sem snapshot parcial apresentado como válido.
- **Reabrir:** só o último fechado e sem outro ativo.
- **Vida/desgaste:** é tempo de vida, não quantidade; valor perto do limite é aviso, não bloqueio.
- **Sem integração SAP automática.**

### 9.19 Regras negativas

- Boquilhas NÃO é dono do master BQ (Ferramentas é).
- Boquilhas NÃO altera o master da ferramenta ao registar movimentos; a consulta do registo BQ/Lote existente e a manutenção das características confirmadas como editáveis são feitas a partir do Armazém pelo Responsável (Q4).
- Boquilhas NÃO é o dono do ciclo de vida geral da BQ como ferramenta.
- Movimento de reparação de Boquilhas NÃO é movimento físico de Armazém; Boquilhas não gere localização física nem movimenta stock de Armazém.
- Boquilhas NÃO gere movimentos gerais/operacionais de BQ fora do fluxo de reparação.
- Boquilhas NÃO altera a produção/planeamento do Job On nem reescreve revisões/snapshots históricos.
- Selecionar BQ+lote no Job On NÃO cria automaticamente um movimento de Boquilhas.
- Controlo NOK/aviso NÃO altera automaticamente o estado/saldo de Boquilhas.
- Retorno a mais NÃO bloqueia e NÃO é somado automaticamente (é aviso + discrepância).
- Saldo negativo na coluna `Saldo` NÃO é bloqueio, rejeição, correção automática, exceção de permissão, workflow obrigatório nem discrepância obrigatória.
- O aviso de correspondência (formulário Entrada/Saída) NÃO bloqueia e NÃO altera a quantidade física do lote.
- Eliminar movimento no Histórico = anulação registada (com confirmação), nunca remoção física.
- Não apresentar o indicador `Contagem reconciliada`.
- Aviso ≠ bloqueio.
- Histórico NÃO é reescrito.
- `BQ NUNCA REPARAÇÃO INTERNA`.
- História NÃO é dono dos registos operacionais de Boquilhas; apenas lê eventos.
- Boquilhas não usa ao vivo o Job On para decidir saldos (usa contexto/snapshot).
- Não existe módulo separado "Boquilhas Operador/Responsável/BQ master".
- Sem tab `Fabrico`.
- Vida/utilização NÃO é quantidade; não se mostra como barra de progresso.
- Admin não é operacional de Boquilhas.
- Não se bloqueia a criação em falta apenas porque o master de Ferramentas não está totalmente preenchido.
- `Referência + Lote` é a identidade do lote; Máquina/Linha é contexto registado, NÃO identidade composta.
- Não reduzir a pesquisa a apenas referência ou apenas lote.
- Máquina/linha é escolha múltipla; pelo menos uma deve ser selecionada na criação em falta.
- Não inferir compatibilidade de máquina a partir da referência.
- `Fechar criação` NÃO é `Fechar registo de reparação`.
- Criar a identificação em falta NÃO torna Boquilhas dono do master de BQ.
- Boquilhas NÃO é a superfície normal de manutenção de uma BQ/Lote já existente.
- O `Total`/quantidade inicial do Registo é a quantidade do registo do fluxo de reparação, NÃO a verdade física total do stock do Armazém.
- Em Boquilhas, Operador / Controlador e Responsável têm as MESMAS ações funcionais (Q1).
- `% UTILIZAÇÃO = MANUAL VALUE` (Q2). O sistema nunca calcula, incrementa, deriva, sincroniza nem atualiza `% utilização` automaticamente.
- `MASTER OWNERSHIP ≠ AUTOMATIC VALUE` e `MASTER OWNERSHIP ≠ READ-ONLY IN EVERY OPERATIONAL SURFACE` (Q2).
- O reminder/alarme da transição Produção → Armazém NÃO altera o valor de `% utilização` (Q2).
- O trigger do reminder não transfere posse (Q2).
- `Data de abertura` é um campo de data EDITÁVEL (Q3).
- Existente → consultar/manter no Armazém (Q4).

### 9.20 Regras superseded / refined

| Item | Classificação |
|---|---|
| Regra do retorno em excesso (aceitar na íntegra, entrada excecional, nunca bloquear) | Regra funcional atual |
| Registo manual Entrada/Saída com coluna `Saldo` (saídas menos entradas; negativos a vermelho) | Preservado (nunca superseded) |
| BQ NUNCA REPARAÇÃO INTERNA | Regra funcional atual |
| Ciclo de vida geral / domínio BQ genérico / movimentos operacionais gerais de BQ associados a Boquilhas | Substituído por clarificação posterior |
| Boquilhas como "BQ OPERATIONAL FLOW" genérico | Substituído por clarificação posterior |
| Boquilhas = dono do master BQ | Substituído por clarificação posterior |
| Antiga lógica "bloquear retorno sem correspondência" + permissão "entrada excecional" como autorização | Histórico / superseded |
| Mockups com divisão "Responsável · Metrologia" / perfil separado | Histórico / superseded |
| Percentagem de utilização (vida) | Regra funcional atual |
| Fluxo completo de reparação externa de BQ (batch) | Histórico / adiado |
| Registo — criar/select (`EXISTE → SELECIONA` / `NÃO EXISTE → CRIA`) | Regra funcional atual |
| Máquina/linha multi-select + não identidade composta + filtragem no Job On | Regra funcional atual |
| Total = quantidade inicial do fluxo de reparação, não stock físico do Armazém | Regra funcional atual |
| `Fechar criação` (painel) ≠ `Fechar registo de reparação` (trace) | Regra funcional atual |
| Ciclo de vida genérico do BQ tool/master em Boquilhas: arquivar/sucatar/restaurar | Substituído por clarificação posterior |
| Filtros/estados de "ficheiro" ligados ao lifecycle genérico | Substituído — "arquivados" corresponde ao fecho do registo de reparação; "Não reparadas" é a declaração atual de irrecuperável |
| "Editar ficheiro" genérico (editar referência/lote/linhas permitidas como master) | Substituído — "Editar ficheiro" limita-se à correção do registo do fluxo de reparação |
| Perfil em Boquilhas — UNKNOWN Operador vs Responsável | Fechado (Q1) |
| Utilização % no Registo — snapshot introduzível vs read-only do Ferramentas | Fechado (Q2) |
| Data de abertura — automática vs editável | Fechado (Q3) |
| "Armazém does not own or edit the utilisation record" | Refinado por Q2 + Q4 (sem automatização; atualização manual possível pelo Responsável na superfície de manutenção do Armazém) |

Comportamentos de design preservados: formulário Entrada/Saída (Data, Quantidade, Motivo, Detalhe, Observações; motivos Normal/Movimento anterior/Correção/Outro); aviso de correspondência no formulário (oculto em "Movimento normal"; nunca bloqueia; não altera quantidade física); coluna `Saldo` na lista de movimentos do lote e na tabela do Histórico; Corrigir movimento / Eliminar movimento no Histórico; resumo do lote ativo em três blocos; snapshot final imutável ao fechar; painel lateral (cartões por linha, navegação para Job On/Registo, menu `…`, alerta de conflito); Definições (reparadores predefinidos/permitidos por linha B1–C3; "Sem associação"; sugerido na saída; desativar não eliminar); não apresentar o indicador `Contagem reconciliada`.

### 9.21 Resumo funcional final

Boquilhas é o módulo onde a fábrica registra, por referência + lote, o fluxo de movimentos de reparação externa de BQ — quantas BQ de um lote saem para reparação, com que reparador, quantas voltam e qual o balanço de reparação — num fluxo diário/contínuo com histórico permanente e sem nunca bloquear quando um retorno vem a mais.

**O utilizador:** abre Boquilhas → vê as linhas e o contexto de produção; seleciona ou cria o lote de BQ; registra manualmente cada movimento; vê o saldo de reparação atualizado, a diferença de cada movimento (coluna `Saldo`) e o histórico; trata discrepâncias de retorno a mais; configura reparadores por linha.

**O sistema:** guarda no servidor cada movimento introduzido manualmente; apresenta o saldo/diferença resultante; preserva todo o histórico, com quem/quando; aplica apenas os cálculos já explicitamente documentados (saldo de reparação derivado dos movimentos registados; snapshot final imutável ao fechar); nunca bloqueia um retorno a mais (aviso + discrepância); não toma decisões operacionais automáticas.

**Fronteiras:** Ferramentas = BQ tool/master; Boquilhas = movimentos de reparação externa de BQ; Armazém = localização física / movimentos normais de armazém. BQ comporta-se como CM/MF no modelo normal de ferramenta/master/armazém. Boquilhas não altera produção, não altera o master, não gere stock físico. BQ nunca em Reparação Interna. História apenas lê.

---

<a id="p2-ri"></a>
## 10. Reparação Interna

### 10.1 Objetivo

A Reparação Interna (RI) é o registo operacional rápido de factos de reparação interna de ferramentas CM e MF durante a produção, executado pelo reparador de turno.

A RI regista ocorrências de reparação efetivamente realizadas por operadores durante os turnos. Cada registo RI responde a:

- que CM ou MF foi reparada;
- qual o número individual da ferramenta;
- quem reparou;
- em que Linha;
- sob que contexto de Produção/Referência;
- quando a reparação ocorreu.

A RI regista ocorrências de reparação. Não gere um ciclo de vida de reparação. Não existe pré-registo obrigatório. A RI não gere `Por reparar` / `Reparado`, nem início/fim, fila, duração ou transições de estado de reparação. Esses estados pertencem à gestão/estado da ferramenta armazenada no contexto Armazém (stored-tool) e não são estados de registo RI. Registar uma ocorrência na RI não escreve, transiciona, infere nem altera automaticamente nenhum deles. A RI é dona apenas dos registos de ocorrências de reparação e da cadeia de correção/histórico.

A mesma ferramenta CM/MF individual pode ser reparada várias vezes na mesma produção ou turno. Cada ocorrência efetiva cria um novo registo RI independente. Nunca deduplicar nem fundir reparações repetidas.

O registo mínimo exige apenas: Linha (B1–C3); Tipo (CM/MF); número individual da ferramenta. Referência, lote, produção e contexto do Job On são associados automaticamente quando disponíveis. A falta de contexto nunca bloqueia o registo operacional.

O registo serve de evidência factual, por exemplo para diário anual de auditoria. O sistema não calcula pontuação, ranking, produtividade ou avaliação automática.

### 10.2 Âmbito e classificação

**Ferramentas abrangidas:** apenas CM e MF.

**Exclusão de BQ:** BQ nunca é selecionável, reparável ou processada como input de reparação na RI. BQ pode aparecer apenas como identificação contextual dentro da referência completa de produção. Exemplo: `5447T173` — `T173` é contexto apenas.

RI e Boquilhas são módulos independentes. Não existe dependência funcional, nem dependência técnica afirmada, nem reutilização afirmada de serviço/API/contexto entre RI e Boquilhas. O master BQ pertence a Ferramentas.

**Classificação:** a RI é um módulo funcional de topo, atribuível por utilizador no Admin, no primeiro nível de navegação operacional. Não é área interna, workflow ou variante de perfil de outro módulo. As tabs Registo e Consulta são áreas internas do módulo RI, nunca módulos independentes.

**Independência:** a RI é independente do Controlo; ambos consomem o contexto do Job On diretamente. A RI é distinta da Reparação Externa; o workflow externo pertence à sua própria documentação.

### 10.3 Utilizadores e acesso

**Reparador.** É sempre o utilizador autenticado. Nunca é selecionado manualmente a partir de qualquer diretório. "Reparador de turno" é o título funcional do papel, não um quarto perfil.

Existem exatamente três perfis globais: Admin; Responsável; Operador / Controlador.

O acesso é controlado pela atribuição individual do módulo. Módulo atribuído ≠ perfil. O Admin não é operacional por omissão.

**Operador / Controlador (quando a RI lhe está atribuída):**

- é o utilizador operacional de registo da RI;
- o utilizador autenticado é o reparador;
- regista as próprias ocorrências de reparação CM/MF;
- a vista operacional baseia-se na própria atividade;
- pode corrigir/anular os próprios registos.

**Responsável.** Tem visibilidade adicional read-only ao nível da Produção através do Job On. Para uma Produção/run de fabrico, pode consultar:

- Controlo dessa Produção;
- ocorrências de reparação RI durante essa Produção;
- quem executou essas reparações RI;
- informação relevante de reparação/movimentos BQ associada a essa Produção;
- atores onde o registo de origem os fornece.

Esta vista é read-only, contextual à Produção, exposta através do Job On, e é informação adicional para o Responsável.

Não está estabelecido que o Responsável repara, registra ocorrências RI, corrige registos RI, anula registos RI ou edita registos RI de outros utilizadores. A única diferença de perfil confirmada é a consulta adicional read-only ao nível da Produção através do Job On. Esta consulta não confere poderes de escrita sobre registos RI e não transfere posse para o Job On.

**Permissões funcionais:**

- Registar: Operador / Controlador com RI atribuída, enquanto utilizador autenticado/reparador.
- Consultar própria atividade: registos do utilizador autenticado.
- Consultar a partir do Job On (Produção): perfil Responsável — read-only de Controlo + RI (reparações e atores) + informação relevante de reparação/movimentos BQ daquela Produção.
- Corrigir/Anular: apenas os próprios registos; regra de autorização no backend, não apenas UI.
- Consultar todos os utilizadores: apenas no Admin/Auditoria.

### 10.4 Áreas internas e fluxo de registo

**Tab Registo.**

1. Seletor de Linha: cartões B1–C3, de largura total. Cada cartão mostra Linha, Referência completa (ex.: `5447T173`, sem truncar) e Produção correspondentes ao contexto RI aplicável à Linha.
2. Contexto automático: após selecionar a Linha, o ecrã mostra apenas Linha, Referência e Produção. O painel de contexto interno resolve ID do Job On, revisão exata, lote, IDs estáveis e data/hora. Estes elementos internos não ocupam o ecrã de Registo.
3. Tipo e Número: seleção CM/MF e introdução do número individual.
4. "Os meus últimos registos": lista recente com paginação. Botões de Corrigir/Apagar fora da tabela.
5. Layout obrigatório: seletor de Linha em cartão horizontal no topo; painel de contexto em linha própria por baixo. Nunca lado a lado; sem scroll horizontal.

**Tab Consulta.**

1. Filtros: Datas, Linha, Produção, Referência/lote, Tipo, número e "apenas corrigidos".
2. Lista e Detalhe: coluna Estado (`Atual` / `Corrigido`). Duplo clique abre detalhe read-only com histórico completo de correções.
3. Correção inline: cartão com Linha, Tipo, Número e Nota opcional. O contexto original permanece read-only.

**Fluxo de registo operacional:**

1. Escolher a Linha, por exemplo B1. O sistema resolve automaticamente o Job On aplicável e mostra ao reparador Linha, Referência completa e Produção. A revisão exata, lote e restantes relações de contexto são resolvidos e preservados internamente quando disponíveis.
2. Escolher CM ou MF.
3. Introduzir o número individual.
4. Confirmar `OK · Registar`.

**Pós-persistência:** é criado um registo RI independente; Linha e Tipo permanecem selecionados onde estabelecido; o campo número é limpo; o foco regressa ao campo número; o sucesso só é mostrado após persistência real.

O utilizador não volta a introduzir manualmente: Referência; Produção; Job On; revisão; lote; reparador; data/hora.

### 10.5 Contexto de produção

Associação conceptual:

```
Linha
→ produção aplicável
→ Job On exato
→ revisão exata do Job On
→ Produção
→ Referência completa
→ relações de lote/ferramenta quando resolvíveis
```

**Contexto visível operacional:** Linha; Referência completa; Produção.

**Contexto resolvido/guardado internamente quando disponível:** Job On ID; revisão exata; relações de lote; relações de ferramenta; Production ID; identificadores estáveis; metadados de auditoria. Os IDs internos não devem ser apresentados como UI visível obrigatória.

**Regra temporal 06:00 / 09:00:**

- a mudança/preparação física da produção ocorre às 06:00;
- entre 06:00 e 08:59, a RI mantém como contexto a produção anterior dessa Linha;
- às 09:00, a RI passa automaticamente para o novo contexto indicado pelo Job On;
- a data final, isolada, não provoca troca de contexto;
- esta regra é exclusiva da projeção de contexto da RI;
- esta regra não altera datas, estados, planeamento ou calendário do Job On.

**Sem contexto / contexto ambíguo.** A falta de contexto Job On/Produção não bloqueia o registo. Se não houver contexto resolvível: o registo continua; a UI pode mostrar `Sem associação`; a RI mantém o facto operacional de reparação; nenhuma relação é inventada. Se o contexto estiver ambíguo: não auto-selecionar; não inventar certeza; o registo continua sem associação inequívoca.

Não introduzir seleção manual de Produção/Referência.

`Editar contexto` **não faz parte da verdade funcional**. Se existir na implementação, deve ser tratado apenas como divergência da implementação atual.

### 10.6 Identificação e validação

**Identificação operacional:** Tipo (CM ou MF); número individual.

**Números repetidos:** são ocorrências independentes válidas. Nunca deduplicar.

**Validação — princípio NO operational hard blocks:**

Bloqueios estruturais:

- Linha inválida;
- Tipo fora de CM/MF, incluindo BQ;
- Número vazio;
- Ator não autenticado;
- Módulo não atribuído.

Não bloqueante:

- falta de contexto;
- número não encontrado;
- tipo divergente do master;
- número repetido.

O sistema aceita o facto introduzido pelo reparador. Não cria ferramentas automaticamente. Não troca CM↔MF. Não inventa lotes.

### 10.7 Ocorrências repetidas vs correções

A mesma ferramenta CM ou MF individual pode ser reparada múltiplas vezes. Exemplo: CM 45 reparada às 10:00 → regressa à produção → volta a sair às 12:30 → é reparada novamente. Isto cria dois registos RI independentes.

Regras:

- cada reparação efetiva cria um novo registo RI;
- reparações repetidas são normais;
- nunca deduplicar;
- nunca fundir reparações repetidas;
- nunca bloquear porque a mesma ferramenta foi reparada anteriormente;
- não modelar um único "ciclo de vida de reparação" persistente para a ferramenta.

**Ocorrência repetida de reparação:** a mesma ferramenta é genuinamente reparada de novo mais tarde → novo registo RI independente.

**Correção:** um registo RI anterior contém informação incorreta → nova versão append-only desse mesmo registo histórico.

Os dois conceitos permanecem completamente separados. Não fundir ocorrência repetida com correção.

### 10.8 Modelo de correção e anulação

**Correção — append-only.** As correções são append-only. Uma correção cria uma nova versão; o original nunca é sobrescrito nem desaparece. Correções sucessivas são suportadas:

```
Original → Correção 1 → Correção 2 → Correção 3 → ...
```

Um registo já marcado `Corrigido` pode ser corrigido novamente. A versão válida mais recente é operacional; a sequência completa permanece historicamente preservada.

Não introduzir: limite de correção a um nível; edição in-place; sobrescrita; colapso de histórico. O reparador original e a data/hora originais permanecem read-only. Mudança de Linha na correção: recalcula o contexto para a nova Linha sem alterar o Job On original.

**Anulação.** `Apagar registo` é uma anulação auditável:

- remove o registo da vista operacional ativa;
- não executa hard delete do facto histórico;
- apenas os próprios registos;
- confirmação em 2 passos na UI;
- sem motivo obrigatório.

### 10.9 Dados, histórico e auditoria

**Campos do registo:** ID estável; Linha; Tipo; Número; Operador (servidor); Data/hora (servidor); Job On/revisão (snapshot); Produção/Referência (snapshot); Lote efetivo, quando resolvido; Motivo da correção, opcional.

**Imutabilidade.** Revisões posteriores do Job On não reinterpretam registos antigos. O contexto exato fica historicamente preservado.

**Auditoria.** Cada ação preserva: ator canónico; nome legível em snapshot; data/hora; módulo; ação; entidade; resultado. As ações integram o diário global de auditoria, append-only.

A **História/Auditoria** lê e apresenta; não é dona dos factos RI.

### 10.10 Fronteiras e ownership

| Domínio factual | Owner funcional | Fronteira |
|---|---|---|
| Registos de reparação interna e histórico | Reparação Interna | Dona das ocorrências de reparação e da cadeia de correção. |
| Master da ferramenta / identidade / dados-mestre técnicos | Ferramentas | A RI consome identidade em leitura; não altera master. |
| Estado stored-tool (`Por reparar` / `Reparado`) | Armazém — contexto stored-tool | Gerido fora da RI; a RI não escreve, transiciona, infere nem gere estes estados. |
| Localização física + movimentos | Armazém | Registar reparação não move a ferramenta fisicamente; a RI não infere movimentos. |
| Contexto de produção / referência | Job On | A RI consome o contexto; não cria, não altera, não reconstrói o Job On. |
| Registos de Controlo | Controlo | O Controlo é dono dos seus registos. |
| Registos de reparação/movimentos BQ | Boquilhas | A Boquilhas é dona dos seus registos BQ; a RI não depende da Boquilhas. |
| Leitura de factos de auditoria | História / Auditoria | Lê e apresenta; não possui os factos RI. |
| BQ no contexto da RI | — | BQ nunca é reparada na RI; pode aparecer apenas como contexto da referência completa. |
| Vista de Produção read-only | Job On — integração/apresentação | O Responsável pode consultar, a partir do Job On, Controlo + ocorrências RI + atores + informação relevante de reparação/movimentos BQ daquela Produção. Apenas leitura. |

A consulta read-only de Produção disponibilizada ao Responsável através do Job On não transfere posse.

### 10.11 Regras negativas — RI NÃO...

1. NÃO repara BQ.
2. NÃO torna BQ selecionável na RI.
3. NÃO processa BQ como input de reparação RI.
4. NÃO é dona do master da ferramenta; Ferramentas é.
5. NÃO é dona do estado stored-tool `Por reparar` / `Reparado`.
6. NÃO escreve, transiciona, infere nem sincroniza `Por reparar` / `Reparado`.
7. NÃO altera automaticamente o estado stored-tool após uma reparação.
8. NÃO move ferramentas fisicamente.
9. NÃO infere movimentos físicos.
10. NÃO participa no ciclo físico de Armazém.
11. NÃO seleciona o reparador manualmente; reparador = utilizador autenticado.
12. NÃO inventa associações Job On/Produção/Referência.
13. NÃO resolve ambiguidade automaticamente.
14. NÃO bloqueia o registo por falta de contexto.
15. NÃO deduplica números repetidos.
16. NÃO deduplica ocorrências de reparação repetidas.
17. NÃO funde ocorrências repetidas com correções.
18. NÃO altera o Job On.
19. NÃO reescreve factos históricos.
20. NÃO faz hard delete.
21. NÃO faz do Job On dono do histórico RI.
22. NÃO faz da História dona dos factos RI.
23. NÃO cria um quarto perfil; "reparador de turno" não é perfil.
24. NÃO transforma screen/tab em módulo; Registo/Consulta são áreas internas.
25. NÃO calcula pontuação, ranking, produtividade ou avaliação.
26. NÃO gere máquina de estados de reparação.
27. NÃO gere início/fim de reparação.
28. NÃO gere fila, reparação ativa ou duração.
29. NÃO tem modelo de quantidades.
30. NÃO tem documentos, impressão, PDF ou exportação.
31. NÃO permite corrigir/anular registos de outros; regra de backend.
32. NÃO cria dependência RI↔Boquilhas.
33. NÃO transfere ownership pela consulta read-only de Produção.
34. NÃO dá ao Responsável poderes de escrita na RI a partir da vista de Produção.

### 10.12 Resumo funcional final

A Reparação Interna regista ocorrências de reparação interna efetivamente realizadas de CM e MF durante a produção.

O reparador autenticado seleciona a Linha (B1–C3), o Tipo (CM/MF) e o número individual. A Referência não é escolhida manualmente. A partir da Linha, a RI resolve automaticamente o contexto aplicável no Job On e apresenta Linha + Referência completa + Produção quando disponível. A regra temporal é 06:00/09:00: entre 06:00 e 08:59 mantém-se a produção anterior; às 09:00 a RI muda automaticamente para o novo contexto. A referência completa, por exemplo `5447T173`, é preservada; `T173` é contexto apenas.

Se o contexto estiver ausente ou ambíguo, o registo continua permitido sem associação inequívoca, sem inventar relações e sem bloqueio.

Cada ocorrência de reparação efetiva cria um registo RI independente. Não existe deduplicação, não existe fusão e não existe bloqueio por reparação anterior. Não existe um único "ciclo de vida" de reparação.

Ocorrência repetida e correção são conceitos completamente separados:

- ocorrência repetida: a mesma ferramenta é genuinamente reparada de novo → novo registo RI independente;
- correção: um registo anterior contém informação incorreta → nova versão append-only desse registo histórico.

As correções são versões append-only que formam uma sequência histórica completa. As correções sucessivas são suportadas e a versão válida mais recente é usada operacionalmente.

A anulação remove o registo da vista operacional ativa sem apagar o facto histórico. Não existe hard delete.

A RI é dona dos seus registos e do seu histórico. História/Auditoria apresentam em leitura. O Job On integra para consulta sem posse. RI e Boquilhas são módulos independentes. BQ nunca é reparada, selecionável ou processada na RI.

A RI não gere `Por reparar` / `Reparado`, nem qualquer máquina de estados de reparação. Esses conceitos pertencem ao estado da ferramenta armazenada (contexto Armazém / stored-tool). Registar uma ocorrência RI não escreve, transiciona, infere nem sincroniza esses estados.

O Responsável tem, adicionalmente, uma consulta read-only ao nível da Produção através do Job On: Controlo + ocorrências RI e respetivos atores + informação relevante de reparação/movimentos BQ dessa Produção, com o ator quando a origem o fornecer. Esta consulta não altera registos, não confere poderes de escrita extra e não transfere ownership.

**Detalhes adiados (não bloqueantes):** formato/intervalo do número individual de CM e MF; futura exigência de observação/motivo (hoje opcional); mais de um lote do mesmo tipo ativo na Linha; campo "turno"; representação exata da anulação em listas/consultas; superfícies de UI em aberto; política de relógio/offset de fábrica (DST).

---

<a id="p2-re"></a>
## 11. Reparação Externa

### 11.1 Decisões confirmadas

- **D1 — quem usa o módulo:** a Reparação Externa é um módulo do Responsável; o Operador não gere batches na Reparação Externa; o Operador usa o Armazém para movimentos físicos individuais de reparação (destino = reparação; associação do reparador).
- **D2 — estrutura:** áreas internas Registo / Ferramentas / Histórico / Definições; CM e MF são seleções/fluxos de tipo separados; Boquilhas é módulo de topo separado, não é área interna da Reparação Externa; a antiga navegação combinada "Reparação" e a composição de seis áreas estão superseded.
- **D3 — edição:** um batch de Reparação Externa é sempre editável pelo Responsável, em qualquer fase do ciclo de vida; o estado nunca bloqueia nem remove opções de edição.

### 11.2 Regras explícitas preservadas

1. REPARAÇÃO EXTERNA = módulo do RESPONSÁVEL para gestão de batches de reparação externa CM/MF.
2. OPERADOR não gere batches da Reparação Externa. O Operador pode usar o Armazém para movimentos físicos individuais de reparação: saída física de ferramenta individual; destino = reparação; associação do reparador externo.
3. BATCH MANAGEMENT = Reparação Externa. INDIVIDUAL PHYSICAL MOVEMENT = Armazém.
4. INTERNAL AREAS: Registo; Ferramentas; Histórico; Definições.
5. CM e MF: tipos separados; fluxos separados; nunca combinados num único tipo; nunca combinados num batch misto.
6. BQ: fora da Reparação Externa; sem tab BQ; sem tipo de batch BQ; a reparação externa de BQ pertence ao módulo Boquilhas.
7. BATCH EDITING: sempre editável pelo Responsável; em todas as fases do ciclo de vida; o estado nunca remove opções de edição; sem estados congelados; sem locks de aprovação; sem restrições de UI baseadas em estado.
8. OWNER PRINCIPLE: nunca bloquear ou remover opções do utilizador, salvo se o Owner estabelecer explicitamente uma impossibilidade de negócio genuína.

### 11.3 Objetivo do módulo

REPARAÇÃO EXTERNA = gestão de batches de reparação externa de CM/MF, módulo do Responsável.

O módulo existe primariamente para o Responsável:

1. preparar um batch de reparação;
2. escolher as ferramentas CM ou MF desse batch;
3. associar o reparador;
4. despachar o batch;
5. acompanhar o progresso;
6. gerir os retornos;
7. concluir o batch;
8. manter o histórico do batch.

O módulo não deve ser descrito primariamente como um ecrã genérico de movimentos de armazém. Os movimentos físicos individuais pertencem ao Armazém. A Reparação Externa resolve o problema real de a fábrica ter de enviar ferramentas de moldes, CM e MF, para reparadores externos:

- que ferramentas saem;
- para qual reparador;
- quando voltam;
- o que fica registado.

É a gestão de batches / listas de saída programada para reparação externa:

- preparados com antecedência;
- com confirmação física explícita de saída e retorno;
- coordenada com o Armazém.

Ponto operacional chave:

- o fluxo não parte da ferramenta que está em produção — essa associação pertence exclusivamente à Reparação Interna;
- a Reparação Externa parte de uma produção futura planeada;
- o Responsável prepara o batch dias antes do início previsto do fabrico;
- a reparação externa não carrega a produção atualmente ativa;
- a lista parte de uma produção futura e mostra a data prevista de início.

O BQ não participa na Reparação Externa. O fluxo de movimentos de reparação externa de BQ é o módulo Boquilhas.

### 11.4 Posição no modelo global

- Módulo de topo, atribuível por utilizador no Admin.
- Uma única page id: `reparacao_externa.listas`.
- Sem atribuição, o módulo não aparece na navegação nem dá acesso.
- Acesso = módulo atribuído; sem capability adicional documentada.

Posição relativa:

- **Ferramentas:** dono do master CM/MF, peças/lotes; a Reparação Externa consome identidade em leitura.
- **Armazém:** dono único do estado físico (stock, posições e movimentos); a Reparação Externa nunca escreve tabelas do Armazém, consome o porto de movimentos; detém também o fluxo de movimentos físicos individuais de reparação do Operador.
- **Job On:** contexto de produção/planeamento; relação explícita e pontual, campo no batch; não automática.
- **Reparação Interna:** módulo irmão, separado; intervenções internas de turno vs ciclos externos; fronteira explícita.
- **Boquilhas:** módulo de topo separado; dono do fluxo de movimentos de reparação externa de BQ; o BQ não participa na Reparação Externa; sem tab BQ; sem batches BQ; sem mistura com CM/MF.

### 11.5 Perfis, utilizadores e acesso

Exatamente três perfis: Admin; Responsável; Operador / Controlador. A Reparação Externa é um módulo do **RESPONSÁVEL**.

**Responsável.** O fluxo de preparação, edição, disponibilização, acompanhamento e conclusão de batches é executado pelo Responsável com o módulo atribuído. O Responsável gere o batch durante todo o ciclo. O batch é sempre editável pelo Responsável.

**Operador.** O Operador NÃO usa a Reparação Externa para gerir batches. Não existe comportamento de Operador dentro da Reparação Externa. O Operador pode usar o Armazém para registar uma saída física individual de ferramenta, marcar o destino como reparação e associar o reparador externo. Essa é uma operação de Armazém, segundo as regras do Armazém.

**Admin.** Não é operacional dentro do módulo. Atua ao nível da atribuição do módulo ao utilizador.

**Confirmação física.** Quem confirma fisicamente é o operador do Armazém; confirma cada retirada/entrada de posição; o efeito físico pertence ao Armazém.

### 11.6 Estrutura interna

Áreas internas do módulo:

1. **Registo** — criação/edição de batches e listas programadas; construtor de batches CM e MF; tipos separados; pode apresentar as listas programadas.
2. **Ferramentas** — seleção/pesquisa das ferramentas CM ou MF que compõem os batches.
3. **Histórico** — consulta transversal do ciclo (saída/entrada; operadores; reparador; estado).
4. **Definições** — reparadores; associações por tipo; associações por Linha/máquina.

**CM vs MF.** Dentro das áreas relevantes, CM e MF são seleções/fluxos de tipo separados. CM e MF permanecem funcionalmente separados; nunca combinados num único tipo; nunca num batch misto.

**Envios.** Não inventar um tab independente "Envios". "Envios" é tratado funcionalmente como o ciclo de vida do batch/lista.

**O que não pertence ao módulo.** Boquilhas NÃO é uma área interna deste módulo. Não existe tab Boquilhas na Reparação Externa. O fluxo BQ é o módulo de topo Boquilhas. A antiga composição de seis áreas (Boquilhas; Contra moldes; Moldes finais; Envios; Histórico; Definições) está superseded / do not implement.

### 11.7 Conceito central do batch

**O que é um batch.** Representa uma saída programada de ferramentas para um reparador externo:

- batch/envio de ferramentas para reparação externa;
- preparado para uma produção futura;
- não parte da ferramenta atualmente em produção;
- preparado com antecedência;
- o Responsável prepara, compõe, associa reparador, despacha, acompanha, gere retorno e conclui.

**Dados do batch.**

Cabeçalho: código da lista; tipo (CM ou MF — nunca misturados); reparador; data prevista; criado por/data; estado.

Itens (CM/MF): Referência; lote; número individual; máquina/linha; posição atual, quando conhecida.

**Ferramentas do batch.**

CM: unidade operacional = ferramenta CM individual; seleção por Referência, lote, máquina permitida, número individual; estado e localização vêm dos domínios respetivos; a saída programada referencia IDs estáveis de CM; retorno pode incluir observação; a observação não altera dados mestres automaticamente; batch preparado para produção futura.

MF: segue exatamente o mesmo ciclo externo do CM; usa exclusivamente ferramentas MF; usa os respetivos IDs, campos, reparadores e histórico; CM e MF são tipos separados; partilhar UI não autoriza combinar CM e MF.

**Edição permanente.** O batch é sempre editável pelo Responsável, em qualquer fase do ciclo de vida, incluindo antes da criação, após ficar disponível ao Armazém, em "A retirar", em "Enviado", em retorno parcial, e em qualquer outro estado. O estado nunca bloqueia nem remove opções de edição, incluindo a composição/ações relevantes do batch. Nenhuma regra limita a edição a "antes de criar/enviar". Nunca bloquear nem remover opções sem uma regra de negócio explícita do Owner.

**Identidade e duplicação.**

- CM e MF usam listas e coleções temporárias separadas;
- mudar o seletor nunca converte nem mistura ferramentas já adicionadas;
- ao regressar ao tipo anterior, o rascunho é preservado;
- CM e MF nunca são fundidos num único tipo;
- criar uma lista CM/MF exige pelo menos uma ferramenta individual adicionada;
- o mesmo item lógico, tipo + ferramenta/lote, não pode figurar duas vezes no mesmo batch/contexto de saída aberta (prevenção de duplicidade de dados, identidade — não é um bloqueio de workflow);
- dentro da mesma lista, o construtor evita/não persiste o duplicado.

**Lifecycle e permanência.** O estado deriva de confirmações persistidas, nunca de abrir a página. Um batch concluído não desaparece; passa para o Histórico.

Momentos de ciclo: disponibilizado → "A retirar"; todas as retiradas confirmadas → "Enviado", saída concluída; retorno parcial → "Retorno parcial"; todos os itens de volta → "Concluído", ciclo fechado.

### 11.8 Fluxo operacional completo

1. **Preparação** — o Responsável prepara o batch vários dias antes do início previsto do fabrico; parte de uma produção futura planeada; não parte da ferramenta em produção; a página não mostra cartões de produções ativas; começa diretamente pela seleção CM/MF e pesquisa de ferramentas; quando o batch precisar de associação a uma produção prevista, essa escolha aparece como campo compacto dentro do formulário do batch.
2. **Criação do batch** — o Responsável escolhe o tipo (CM ou MF); pesquisa por Referência, lote, nº individual; seleciona Referência, lote, número individual; escolhe um reparador permitido; define a data prevista; adiciona itens. Sem pelo menos uma ferramenta, o botão "Criar lista" fica desativado. O batch pode ser editado em qualquer fase.
3. **Associação do reparador** — o reparador é escolhido no batch de envio, a partir do diretório registado; o dropdown é filtrado por tipo e Linha/máquina; pode existir default do último reparador conhecido; o utilizador pode alterar manualmente antes de guardar; o envio guarda snapshot do reparador usado.
4. **Disponibilização ao Armazém** — o batch fica disponível no Armazém; estado associado "A retirar"; pode ser impresso, conforme suporte V1; a edição permanece disponível.
5. **Retirada física** — o operador do Armazém confirma cada retirada com um check; confirmação explícita persistida; os efeitos físicos ocorrem através do porto do Armazém.
6. **Envio** — quando todos os itens estão confirmados, a saída é concluída; as posições ficam livres; estado "Enviado"; o ciclo ainda não fechou.
7. **Acompanhamento** — o Responsável acompanha o envio; não duplica os movimentos do Armazém; os movimentos físicos são do Armazém; a Reparação Externa acompanha o ciclo de vida do batch.
8. **Retorno parcial** — parte dos itens regressou; ainda há itens fora; estado "Retorno parcial"; o progresso é mostrado explicitamente; o ciclo só fecha quando todos os itens regressarem.
9. **Retorno completo** — no retorno, o Armazém confirma cada entrada e posição; confirmação explícita; o retorno pode incluir observação, no caso CM; a observação não altera dados mestres automaticamente; o retorno pode ocorrer item a item.
10. **Conclusão** — quando todos os itens regressam, o ciclo fecha; estado "Concluído"; o batch concluído passa para o Histórico; os factos históricos persistidos não são reescritos.

Regra transversal: qualquer confirmação que altere simultaneamente o estado do ciclo de reparação e o estado físico do Armazém corre num único unit of work. Nenhum efeito físico é inferido. Só confirmações explícitas persistidas movem ferramentas.

### 11.9 Estados do batch

- **Preparação:** o batch está a ser construído; ainda não concluído para retirada.
- **A retirar:** o batch está disponível no Armazém; retiradas em curso; confirmação item a item.
- **Enviado:** todas as retiradas confirmadas; saída concluída; posições livres; aguarda retorno.
- **Retorno parcial:** parte dos itens regressou; ainda há itens fora.
- **Concluído:** todos os itens regressaram; ciclo fechado.

**Estado Cancelado.** É estado apenas de compatibilidade de schema. A funcionalidade de cancelamento, `CancelarLista`, está adiada. Não fazer do Cancelado uma regra operacional ativa.

**Transições.** Não são inferidas pela abertura da página; cada transição corresponde a confirmações persistidas: primeira recolha → A retirar; todas recolhidas → Enviado; parcial → Retorno parcial; todas → Concluído.

**Estado não bloqueia edição.** Nenhum estado remove ou bloqueia a edição do batch pelo Responsável. Avançar o batch não remove ações de edição. Não existem bloqueios de aprovação, estados congelados, nem restrições de interface baseadas em estado sem regra de negócio explícita.

### 11.10 Reparadores

- **Diretório.** Existe um diretório canónico de reparadores (vocabulário partilhado), reutilizado pelos fluxos de reparação. Não modelar reparadores como texto livre dentro de cada registo.
- **Seleção.** O reparador é escolhido no batch de envio, a partir do dropdown do diretório registado, filtrado por tipo (CM/MF) e por Linha/máquina.
- **Defaults.** Para reparação externa CM/MF: pré-preencher o último reparador conhecido da ferramenta, com possibilidade de alteração manual antes de guardar.
- **Tipos, máquina e linha.** Um reparador pode suportar múltiplos tipos. Associação reparador ↔ tipo CM/MF. A capacidade por tipo é separada da associação por linha.
- **Ativo e inativo.** Os reparadores têm estado ativo/inativo. Desativar, não eliminar.
- **Snapshot histórico.** Alterar uma associação não reescreve listas ou movimentos antigos. Cada envio guarda snapshot do reparador usado.

### 11.11 Relação com Armazém

**Movimento individual.** O Operador pode usar o Armazém diretamente para dar saída a uma ferramenta individual, marcar o destino como reparação e associar o reparador externo. Isto não exige usar a gestão de batches da Reparação Externa; é uma operação de Armazém. Não implica que toda a ferramenta enviada para reparação externa tenha de provir de um batch.

**Fluxo de batch.** Quando é usado um batch da Reparação Externa: a Reparação Externa é dona do batch (plano, ciclo, itens, estado, histórico); o Armazém é dono do estado/movimentos físicos; as confirmações físicas continuam a ser do Armazém (recolha/retorno item a item; posições).

**Ownership físico.** O Armazém é o dono único do estado físico — stock, posições e movimentos — e da libertação/reocupação de posições.

**Confirmações físicas.** As confirmações de recolha/retorno executam os efeitos físicos através do porto do Armazém, no mesmo unit of work da alteração do estado do ciclo. Nenhum efeito físico é inferido; só confirmações explícitas persistidas movem ferramentas.

**Regras de integridade.** A Reparação Externa NUNCA escreve diretamente tabelas do Armazém; consome o porto detido pelo Armazém; os efeitos ocorrem no mesmo unit of work.

### 11.12 Relação com Ferramentas

O master CM/MF pertence a Ferramentas (identidade, referência, lotes, peças, características). A Reparação Externa não possui o master de ferramentas nem o general lifecycle. Lê do domínio Ferramentas a identidade das peças CM/MF (referência, lote, número individual) via porto. Posição/estado vêm dos domínios respetivos. Não modifica o master. Nenhuma vista cria cópias divergentes das ferramentas. A saída programada referencia IDs estáveis de CM/MF.

### 11.13 Relação com Job On

O batch é preparado para uma produção futura planeada; mostra a data prevista de início; a associação a uma produção prevista aparece como campo compacto dentro do formulário do batch; não é leitura automática da produção ativa; a página não carrega a produção atualmente ativa.

Job On: não cria registos de reparação externa; não seleciona ferramentas reparadas para produção futura; a relação é contexto de produção/prazo, apenas quando existe relação explícita.

A Reparação Interna é aberta a partir do Job On ("Ver reparações"). Na Reparação Externa não há lookup vivo do Job On. Os snapshots imutáveis são o padrão histórico transversal.

O Job On pode exibir, read-only, o último reparador relevante da ferramenta/lote (contexto derivado dos factos, sem edição).

### 11.14 Relação com Reparação Interna

| Dimensão | Reparação Interna | Reparação Externa |
|---|---|---|
| Objetivo | Intervenções internas durante a produção, registos rápidos de turno | Ciclos de envio a reparadores externos preparados com antecedência |
| Atores | Reparador de turno, utilizador autenticado | Responsável, dono do módulo; operador de Armazém confirma física |
| Identidade do reparador | Sempre o utilizador autenticado; nunca selecionado manualmente | Diretório canónico; selecionado manualmente no batch, com default do último usado |
| Tipos | Apenas CM e MF; BQ nunca reparação interna | Apenas CM e MF, como tipos separados; BQ fora do módulo |
| Unidade operacional | Registo individual, tipo + nº individual, com contexto de produção opcional | Batch/lista de itens, peças individuais, para uma produção futura |
| Movimento físico | Sem movimentos de Armazém; intervenção em produção | Movimento físico sim, via porto do Armazém, recolha/retorno |
| Edição | — | Batch sempre editável pelo Responsável, em qualquer fase |
| Validações | Sem bloqueios operacionais | Validações de consistência: duplicado; retorno sem saída; mínimo 1 item; tipos fora de âmbito; o estado nunca bloqueia edição |

### 11.15 Relação com Boquilhas

BOQUILHAS = fluxo de movimentos de reparação externa de BQ; módulo de topo separado. Inclui saída/retorno, quantidades, saldos, reparador, histórico, discrepância 20→25 não-bloqueante e snapshot de fecho.

Fronteira: a Reparação Externa NÃO possui nem executa o fluxo BQ. O BQ não participa na gestão de batches desta autoridade. Não há tab BQ aqui. O BQ nunca é misturado com CM/MF. O master do BQ pertence a Ferramentas. As Boquilhas registam apenas os movimentos de reparação. Nunca reparação interna para BQ. Não inferir participação BQ em batches/listas CM/MF.

### 11.16 Dados introduzidos pelo utilizador / derivados

**Preparação do batch:** tipo do batch (CM/MF); Referência; lote; número individual; posição atual, opcional; produção prevista (campo compacto, quando aplicável); reparador; data prevista ("Enviar até").

**Ações:** editar o batch em qualquer fase; adicionar/remover itens e composição relevante a qualquer momento (não limitado a antes de criar/enviar); Criar lista; Disponibilizar a saída; confirmações de recolha; confirmações de retorno; observação opcional no retorno CM; fecho quando completo.

**Definições:** criar reparador; associar tipos (CM/MF); associar Linha/máquina permitida; estado ativo/inativo.

**Histórico (filtros):** período; tipo; Referência; lote; reparador; estado; Linha/máquina; operador.

**Dados derivados e automáticos:**

- estado do batch: derivado das confirmações persistidas, nunca de abrir a página;
- saída/entrada + operadores + datas por item: registados nas confirmações; ator autenticado; cabeçalho "criado por/data";
- snapshot do reparador por envio: preservado no momento do envio; alterações futuras de associação não reescrevem o passado;
- posição atual / estado / localização: lidos dos domínios respetivos (Ferramentas; Armazém), não inventados;
- reparador predefinido sugerido: último reparador conhecido, com alteração manual;
- movimentos físicos no Armazém: criados pelo porto do Armazém, no mesmo UoW, quando há confirmações explícitas.

**Fórmulas/quantidades:** não existem cálculos documentados. CM/MF trabalham por peça individual. Sem saldos específicos nesta autoridade — o conceito "saldo" é do fluxo BQ/Boquilhas. Não está documentada qualquer automatização adicional.

### 11.17 Quantidades e identidade das peças

A Reparação Externa, CM/MF, trabalha por peça individual identificada: tipo; Referência; lote; número individual. Não há "quantidade" para CM/MF nos batches. O conceito de quantidades/saldo pertence ao fluxo BQ/Boquilhas.

### 11.18 Recolha e retorno

**Recolha.** Confirmação explícita de que o item saiu fisicamente da posição para o reparador externo. Executada item a item pelo Armazém, check. Efeitos: posição liberta, quando todas confirmadas; estado avança. Vai ao Armazém através do porto, no mesmo UoW.

**Retorno.** Confirmação explícita de que o item voltou e ocupou a posição indicada. Regista entrada e operador. Encerra o ciclo ao chegarem todos. Fecho item a item. Pode incluir observação, CM; a observação não altera dados mestres automaticamente.

Estas são as únicas ações documentadas que mudam estado de ciclo + estado físico. Por isso correm atómicas, um único UoW. Nenhum efeito físico é inferido.

### 11.19 Casos parciais e excecionais

- **Retorno parcial:** estado "Retorno parcial"; o progresso é mostrado explicitamente; apenas fecha com todos os itens de volta.
- **Item que não retorna / fecho com itens em aberto:** não há regra ativa documentada para encerrar com itens em falta; adiado.
- **Item duplicado:** o mesmo item lógico, tipo + ferramenta/lote, não pode figurar duas vezes no mesmo batch/contexto de saída aberta. Prevenção de duplicidade de dados, identidade — não é um bloqueio de workflow.
- **Cancelamento:** funcionalmente adiado; o estado Cancelado é compatibilidade de schema.
- **Retorno sem saída correspondente:** registado/mostrado como inconsistência; permitir correção; aviso, não bloqueio duro.
- **Item sem localização conhecida:** aviso; sem localização inventada; não bloqueia.
- **Falha de persistência:** manter a seleção; não mostrar sucesso.
- **Discrepância/reparador errado:** não há regra específica documentada para reparador diferente do planeado; a preservação histórica (snapshot) garante que o reparador efetivo fica registado.

### 11.20 Validações de consistência, avisos e limites de âmbito

Regra do Owner: NUNCA BLOQUEAR OU REMOVER OPÇÕES DO UTILIZADOR, a menos que o Owner estabeleça explicitamente uma impossibilidade de negócio genuína.

Explicitamente NÃO bloqueia / NÃO remove:

- o estado do batch nunca bloqueia a edição;
- a edição está sempre disponível ao Responsável, em qualquer fase;
- avançar o batch nunca remove ações de edição (disponibilizar; "A retirar"; "Enviado"; retorno parcial);
- sem bloqueios de aprovação, estados congelados ou restrições de interface baseadas em estado.

Validações de consistência / limites de âmbito preservados:

1. **Duplicação de item** — prevenção de duplicidade de dados, identidade; não é um bloqueio de workflow.
2. **Retorno sem saída correspondente** — inconsistência registada para correção; não é um bloqueio duro; não se inventa um fluxo de aprovação/resolução.
3. **Mínimo de uma ferramenta** — condição de criação do batch; não é um bloqueio de workflow/estado.
4. **BQ fora de âmbito** — o BQ não é um tipo de batch; simplesmente não aparece como opção.
5. **Efeitos físicos só via porto do Armazém, no mesmo UoW** — invariante de execução, integridade física.

Avisos que NÃO bloqueiam: item sem localização conhecida → aviso; sem inventar localização.

Sem aprovações documentadas: sem fluxos de aprovação; sem correções automáticas; não inventar esses mecanismos.

### 11.21 Histórico e auditoria

Um batch concluído não desaparece; passa para o Histórico.

**Campos mínimos do Histórico:** Lista; Tipo; Referência; Lote; Qtd./N.º; Reparador; Saída; Operador saída; Entrada; Operador entrada; Estado.

Preserva: saída e entrada com datas e operadores; reparador efetivo, snapshot, por envio; estado.

A reparação externa escreve registos de auditoria; a História lê transversalmente esses factos (módulo de leitura).

O histórico não é reescrito: alterações posteriores de associações de reparadores não modificam batches antigos (snapshot preservado). A edição do batch pelo Responsável aplica-se ao batch/ciclo corrente. Os factos históricos persistidos não são reescritos.

### 11.22 Documentos, impressão e outputs

**Impressão da lista programada:** a lista fica disponível no Armazém e pode ser impressa; a impressão é suporte V1.

**PDF / etiqueta / exportação:** não especificado na autoridade funcional do módulo; não documentado.

### 11.23 Ownership

| Dado / conceito | Dono |
|---|---|
| Master CM/MF: identidade, referência, lotes, peças | Ferramentas |
| Estado físico / stock / posições / movimentos físicos | Armazém, dono único; efeitos só via porto |
| Movimento físico individual de reparação: saída, destino = reparação, reparador, via Armazém | Armazém, fluxo do Operador, sem exigir batch |
| Batch/lista de reparação externa: plano, ciclo, itens, estado | Reparação Externa, Responsável |
| Diretório de reparadores / associações por tipo e linha, vocabulário canónico partilhado | Reparação, fonte canónica; reutilizado por Boquilhas e Armazém |
| Eventos/histórico do ciclo de reparação externa, incl. snapshot do reparador | Reparação Externa, factos; leitura transversal pela História |
| Fluxo de movimentos de reparação externa de BQ | Boquilhas, módulo de topo separado |
| Planeamento/produção, contexto de produção futura | Job On, contexto; relação explícita opcional |
| História transversal / auditoria | História, leitura; factos persistidos pelos módulos originais |

### 11.24 Regras negativas

A Reparação Externa NÃO:

- NÃO escreve tabelas do Armazém (nem stock, nem movimentos físicos).
- NÃO infere efeitos físicos — só confirmações explícitas movem ferramentas.
- NÃO altera o master das Ferramentas.
- NÃO cria cópias divergentes das ferramentas; a saída referencia IDs estáveis.
- NÃO parte da ferramenta atualmente em produção — essa associação é exclusiva da Reparação Interna.
- NÃO carrega a produção atualmente ativa.
- NÃO mistura CM e MF em batches/domínio — tipos sempre separados.
- NÃO bloqueia nem remove a edição por estado — o batch é sempre editável pelo Responsável, em qualquer fase; sem estados congelados, sem locks de aprovação, sem restrições de UI por estado.
- NÃO aplica bloqueios operacionais de workflow.
- NÃO implica Reparação Interna — módulos e raízes distintas.
- NÃO duplica os movimentos do Armazém — acompanha sem duplicar.
- O cancelamento NÃO está funcional — `CancelarLista` adiado; estado Cancelado só compatibilidade.
- O BQ NÃO participa na Reparação Externa — módulo de topo Boquilhas; sem tab BQ; nunca misturado com CM/MF.
- O histórico NÃO é reescrito por alterações de configuração/reparadores — snapshots.
- NÃO inventa localização/estado/reparador quando desconhecidos — aviso.
- O Job On NÃO cria nem seleciona na Reparação Externa — relação só de contexto explícito.

### 11.25 Atual vs histórico, superseded e deferred

**Current:** módulo de topo `reparacao_externa`, para o Responsável; gestão de batches CM/MF de reparação externa (preparação, composição, reparador, despacho, acompanhamento, retorno, conclusão, histórico); áreas internas Registo / Ferramentas / Histórico / Definições; CM e MF selecionados separadamente (tipos/fluxos separados, nunca misturados); batches sempre editáveis pelo Responsável em qualquer fase; o estado nunca bloqueia nem remove a edição; o Armazém é dono dos movimentos físicos (efeitos só via porto, mesmo UoW, confirmações físicas do Armazém); o Operador pode enviar ferramentas individuais para reparação através do Armazém (saída individual, destino = reparação, reparador associado); estados Preparação / A retirar / Enviado / Retorno parcial / Concluído; histórico com snapshots; definições de reparadores (diretório canónico; tipos; linha/máquina; ativo/inativo); validações de consistência/limites de âmbito preservados.

**Deferred:** `CancelarLista`; fecho com itens em aberto / destino / outras regras de fecho parcial.

**Superseded:** navegação combinada "Reparação" com menu Boquilhas/Moldes; a página intermédia obrigatória "Reparação"; a composição Boquilhas / Contra moldes / Moldes finais / Envios / Histórico / Definições como conjunto de áreas do módulo (incluindo "tabs globais"); BQ dentro da Reparação Externa; qualquer interpretação de que o Operador gere batches da Reparação Externa; qualquer regra que remova a edição do batch apenas porque o estado avançou (inclui a redação antiga "adicionar/remover itens só antes de criar/enviar").

**Not specified:** PDF / etiqueta / exportação; eliminação de reparadores; regra específica para mismatch de reparador errado como bloqueio; detalhe adicional de transições de estado além do ciclo documentado.

### 11.26 Resumo funcional final

A Reparação Externa é o módulo de gestão de batches de CM/MF do Responsável. O Responsável prepara um batch de reparação externa, escolhe as ferramentas CM ou MF, associa o reparador, despacha, acompanha, gere os retornos e conclui o ciclo, mantendo o histórico.

O Armazém é o dono único do estado físico; os efeitos físicos só ocorrem via porto do Armazém, no mesmo unit of work. O Operador envia ferramentas individuais para reparação através do Armazém (saída individual, destino = reparação, reparador associado), sem gerir batches. CM e MF permanecem tipos separados. O BQ pertence ao módulo Boquilhas. O batch é sempre editável pelo Responsável, em qualquer fase; o estado nunca bloqueia nem remove a edição. `CancelarLista` e o fecho com itens em aberto estão adiados.

---

<a id="p2-tampoes"></a>
## 12. Tampões

### 12.1 Objetivo

Ajudar o Operador / Controlador a saber quantos TP/tampões estão disponíveis para cada configuração técnica / máquina.

### 12.2 Âmbito e classificação

O módulo Tampões é um módulo simples, autónomo e de nível superior (top-level). O seu modelo fundamental é uma tabela simples de configurações + quantidades.

Não é:

- planeamento de produção;
- integração com Job On;
- rastreamento de referências;
- rastreamento de produção;
- rastreamento individual de tampões;
- um módulo rígido de ciclo de vida/estado.

### 12.3 Utilizadores e acesso

- **Utilizador operacional confirmado:** Operador / Controlador.
- **Admin:** não operacional por defeito.
- **Responsável:** sem comportamento operacional específico confirmado para Tampões.

O módulo é de nível superior e atribuível por utilizador no Admin. Se não atribuído, não é mostrado na navegação normal e não há acesso funcional.

### 12.4 Configuração funcional

Características essenciais de configuração atuais:

- Máquina / Máquinas;
- Diâmetro;
- Calote.

Uma configuração pode aplicar-se a uma ou várias máquinas. A(s) máquina(s) faz(em) parte da própria configuração. Não tratar a Máquina como metadados incidentais.

Os campos de configuração são editáveis. O operador pode criar/editar/gerir campos e valores de configuração. O modelo deve permanecer extensível para campos futuros. Não apresentar Diâmetro e Calote como os únicos campos permanentemente hard-coded.

### 12.5 Tabela principal e interação

A UI central é uma tabela simples; cada linha representa uma configuração.

Colunas conceptuais esperadas:

| Máquina/Máquinas | Diâmetro | Calote | Quantidade / categorias |

O utilizador deve perceber imediatamente: "Quantos tampões tenho disponíveis desta configuração para esta máquina?"

**Um clique na linha:**

- seleciona a configuração;
- expõe ações rápidas de quantidade (Adicionar quantidade, Remover quantidade, escolher categoria/saldo opcional quando relevante).

**Duplo clique na linha:**

- abre o editor de configuração para essa linha;
- o operador pode editar Máquina(s), Diâmetro, Calote, etc.;
- após guardar, a configuração atualizada aparece na tabela principal.

### 12.6 Gestão de quantidades

O módulo é controlo de quantidade agregada. Não existem números individuais de tampões.

Preservar:

- quantidades inteiras;
- sem saldo negativo;
- alterações apenas confirmadas após persistência;
- atribuição de operador;
- atribuição de data/hora;
- histórico de movimentos apenas para acrescentar (append-only);
- correção auditável.

### 12.7 Categorias opcionais de quantidade

As classificações de quantidade são opcionais (ex.: Enchidos / Por encher, Maquinados / Por maquinar). Existem apenas para ajudar o operador a separar quantidades quando útil.

Não são:

- obrigatórias;
- necessárias para todas as configurações;
- um ciclo de vida rígido;
- uma máquina de estados obrigatória.

A informação essencial permanece: QUANTIDADE TOTAL DISPONÍVEL POR CONFIGURAÇÃO / MÁQUINA.

### 12.8 Edição de configuração

Duplo clique na linha → alterar metadados de configuração (Máquina(s), Diâmetro, Calote, etc.) → guardar → mesma linha de configuração atualizada.

O Operador / Controlador pode também criar uma **nova configuração**, definindo pelo menos Máquina/Máquinas, Diâmetro e Calote; depois de guardar, a nova configuração passa a aparecer como uma nova linha da tabela principal.

Não confundir edição de configuração com transformação de quantidade.

### 12.9 Movimentos / transformação de quantidade

Quando se move intencionalmente alguma quantidade de uma configuração para outra: movimento de quantidade; origem e destino preservados; histórico append-only.

### 12.10 Histórico e auditoria

Preservar histórico auditável.

Histórico de movimento de quantidade deve preservar: data/hora; configuração; movimento/ação; categoria/saldo opcional; quantidade; antes/depois; operador.

Histórico de edição de configuração deve preservar: o que mudou; valor anterior / novo valor; quem mudou; quando.

Sem sobrescrita silenciosa de factos históricos.

### 12.11 Opções e gestão de campos

Áreas preferenciais do módulo:

- **Registo / Tabela Principal:** tabela principal de configuração + quantidade.
- **Histórico:** movimentos auditáveis / alterações de configuração.
- **Opções / Configuração:** gerir campos, valores, configurações, valores de máquina, diâmetro, calote, campos futuros.

Uma área de Consulta separada é desnecessária se duplicar a tabela principal. Não preservar abas antigas apenas porque existiram historicamente.

### 12.12 Fronteiras e ownership

Tampões é proprietário de: configurações de Tampões; associação Máquina/Máquinas nessas configurações; Diâmetro, Calote e outros campos configurados; quantidades e categorias opcionais; movimentos/histórico; configurações/definições.

Tampões NÃO é proprietário de: Job On; Produção; Referência; registos de negócio de outros módulos.

### 12.13 Regras negativas

Tampões NÃO:

- associa TP a uma Referência;
- associa TP a uma Produção;
- interage funcionalmente com Job On;
- envia dados para Job On;
- consome contexto de Job On;
- planeia produção;
- reserva stock para produção;
- rastreia números individuais de TP;
- requer um ciclo de vida rígido;
- requer Enchidos/Por encher;
- requer Maquinados/Por maquinar;
- infere Máquina a partir de Referência;
- infere Referência a partir de Máquina;
- altera outros módulos;
- permite quantidade negativa;
- reescreve história silenciosamente.

### 12.14 Clarificações confirmadas

- Tampões é autónomo.
- Sem relação com Job On.
- Sem relação com Produção.
- Sem relação com Referência.
- Sem Planeamento.
- Máquina(s) faz parte da configuração.
- Configuração essencial atual = Máquina(s) + Diâmetro + Calote.
- Campos/valores/configurações editáveis.
- Um clique = ações de quantidade.
- Duplo clique = editar configuração.
- Sem numeração individual de TP.
- Categorias de quantidade opcionais.
- Sem ciclo de vida obrigatório.
- Operador / Controlador é o utilizador operacional.
- Zero questões funcionais abertas.

> **Conflito preservado (não resolvido):** as clarificações acima (Tampões autónomo; sem relação com Job On / Produção / Referência; sem Planeamento) conflitam com §5.7.1, onde **TP/Tampão é configuração específica de produção do Job On** (PU / CS / TP, configurados manualmente pelo Responsável no Job On). Ambas as afirmações são preservadas integralmente; a sua reconciliação é decisão do Owner e não é resolvida aqui. Ver §16.

### 12.15 Resumo funcional final

O módulo Tampões é uma tabela simples e autónoma de configurações (Máquina/Máquinas, Diâmetro, Calote) e quantidades agregadas. Permite ao Operador / Controlador ver rapidamente a quantidade disponível por configuração/máquina; um clique seleciona a linha e expõe as ações rápidas de adicionar/remover quantidade, enquanto o duplo clique abre a edição da configuração. Não há integração com Job On, Produção, Referências ou Planeamento. O histórico é auditável e as categorias de quantidade são opcionais.

---

<a id="p2-admin"></a>
## 13. Admin

### 13.1 Propósito

ADMIN é a **superfície de administração do portal**. A sua função é governar **quem pode usar a aplicação e como se comporta dentro dela**, gerindo:

- **Utilizadores** (utilizadores internos do sistema);
- **Templates de acesso** (conceito atual, funcional e gerido no Admin, que empacota os módulos que um utilizador pode aceder);
- **Aplicações** (catálogo de módulos / disponibilidade / ordem);
- **Auditoria** (histórico factual global das ações).

O Admin é para administração de utilizadores / perfis / templates de acesso / aplicações / auditoria. Não é, por si, uma presença operacional dentro dos módulos de produção.

### 13.2 Admin é um módulo de topo atribuível?

Sim. Admin é um módulo funcional de topo que pode ser atribuído a um utilizador, e aparece na lista de módulos atuais de topo:

> Job On, Controlo, Admin, Ferramentas, Armazém, Boquilhas, Reparação Interna, Reparação Externa, Tampões.

Admin é simultaneamente "um módulo de topo atribuível" e "uma área transversal de sistema" — é uma superfície de administração do portal, não um módulo operacional de produção, mas permanece atribuível.

**História NÃO é um módulo de topo.** História é uma tab/área de histórico interno dentro dos módulos/áreas relevantes, não uma unidade de acesso atribuível.

### 13.3 Quem pode aceder ao Admin

Apenas utilizadores cuja atribuição de acesso conceda a capability `admin.gerir` podem alcançar o workspace Admin.

| Módulo | Admin | Operador / Controlador | Responsável |
|---|---|---|---|
| **Admin** | **Sim** | Não | Não |

- Um Administrador puro entra diretamente em `/admin` e permanece na shell administrativa.
- Operador / Controlador e Responsável não acedem ao Admin.
- A sub-área **Audit** exige adicionalmente `audit.view` (ver) e, para exportação, `audit.export`.
- Toda a página/serviço Admin re-autoriza server-side e **falha fechado** (sem identidade resolvida ou sem capability ⇒ proibido). Ocultar um botão não é autorização.

### 13.4 Comportamento do perfil Admin

Admin é um de exatamente três perfis funcionais. Não existe quarto perfil, perfil read-only, nem perfil de gestão/metrologia/consulta.

- O perfil Admin é o perfil administrativo; a sua função é a administração do portal.
- Um Admin **não é implicitamente um utilizador operacional**. O perfil `admin` não concede acesso a módulos operacionais.
- Um Admin puro não deve ser automaticamente convertido em Operador / Controlador ou Responsável.
- **Um perfil nunca concede módulos automaticamente** — mesmo para Admin, o acesso a módulos depende dos templates de acesso associados a esse utilizador.

### 13.5 Áreas internas do Admin

O módulo Admin tem uma landing page e quatro sub-áreas internas (áreas do módulo Admin, não módulos de topo separados):

| Área | Rota | Propósito |
|---|---|---|
| **Landing** | `/admin` | Entrada no workspace Admin |
| **Users** | `/admin/users`, `/admin/users/create`, `/admin/users/edit` | listagem, criação, edição, ativação, reset de password |
| **Templates** | `/admin/templates`, `/admin/templates/edit` | lista/criação/edição de templates de acesso |
| **Applications** | `/admin/applications` | catálogo de módulos (disponibilidade + ordem) |
| **Audit** | `/admin/audit` | consulta do histórico global + exportação anual |

Users, Templates, Applications e Audit são áreas internas **dentro do único módulo Admin** — não são módulos separadamente atribuíveis. **Templates é uma área real e funcional do Admin** (visível e gerível) — não é opcional, não é só apresentação e não é removível do Admin.

### 13.6 Criação de utilizador

Fluxo:

> ADMIN → cria/edita o registo do utilizador/operador → associa um email (que cria/associa a identidade do utilizador) → seleciona o perfil funcional do utilizador → associa o template de acesso → o template determina quais módulos ficam visíveis/acessíveis a esse utilizador.

Ao criar um utilizador/operador, o Admin fornece: Nome/dados do utilizador; Email (associa/cria a identidade/conta); Perfil funcional; estado Ativo (ativo por defeito); template de acesso associado.

Comportamento:

- a identidade/conta do utilizador é criada/associada via email; em falha parcial a operação é reconciliada idempotentemente (sem mapeamento órfão/duplicado);
- uma identidade duplicada/conflituosa é reportada (utilizador já registado);
- o template associado determina os módulos disponíveis para esse utilizador;
- **não mostrar** o UUID de autenticação como "Email"; não expor passwords atuais;
- evento de auditoria `admin.user.created` (append-only).

### 13.7 Edição de utilizador

O Admin pode editar: Nome; Email; Perfil funcional (e título de perfil em texto livre); estado Ativo; template de acesso associado (adicionar, remover ou trocar).

Remover a associação de um template de um utilizador **não** apaga o template globalmente e não afeta outros utilizadores; o template permanece reutilizável no Admin. O template associado determina os módulos disponíveis para esse utilizador.

As edições são guardadas contra concorrência (a UI espera um snapshot "última atualização" correspondente; uma alteração concorrente é reportada como conflito). Eventos de auditoria: `admin.user.updated`, `admin.access_template.updated`, `admin.option.updated`.

### 13.8 Ativação / desativação

O Admin pode ativar ou desativar uma conta de utilizador.

- `active` é um flag por utilizador. Um utilizador inativo não resolve identidade e é negado (utilizador interno inativo ⇒ sem acesso).
- A desativação é registada (`admin.user.deactivated`) e auditada append-only.
- **Self-lockout guard:** um Admin não pode desativar (ou remover admin de) o **último administrador ativo** — o sistema conta os admins ativos remanescentes e reverte a alteração se deixaria zero. Isto evita bloquear o acesso à administração.

Conceitos separados (não fundir):

- **Estado da conta** (`active`) controla se a conta pode autenticar/ser usada;
- **Perfil** controla o comportamento dentro dos módulos a que o utilizador pode aceder;
- **Template de acesso** determina quais módulos ficam visíveis/acessíveis.

### 13.9 Perfil vs templates

| Conceito | Responde a | Determina |
|---|---|---|
| **Perfil** (Admin / Operador / Controlador / Responsável) | como o utilizador atua | a variante/experiência **dentro** dos módulos a que o utilizador pode aceder |
| **Templates** (associados ao utilizador) | que áreas estão disponíveis | **quais** módulos/áreas ficam visíveis e acessíveis a esse utilizador |

**Um perfil nunca concede módulos; associar/remover templates nunca altera o perfil funcional.** Não colapsar perfil e templates num só conceito.

### 13.10 Efeito da associação na navegação / acesso

A navegação principal do utilizador reflete os módulos concedidos através do template associado:

- um módulo concedido aparece na navegação e é funcionalmente acessível (conforme o perfil);
- um módulo não concedido não aparece na navegação normal e não é funcionalmente acessível.

Dentro de um módulo concedido, as tabs/áreas internas podem variar por perfil. A navegação reflete **acesso** (módulos concedidos via template), enquanto as tabs internas refletem **perfil** dentro desses módulos.

**Módulo não concedido:** não aparece na navegação normal; não pode ser usado funcionalmente; não deve ser diretamente alcançável por rota/atalho. Não-concessão é uma barreira de acesso funcional, não apenas ocultação de UI.

### 13.11 Reset de password

O Admin pode iniciar um reset de password para um utilizador, mas:

- o reset requer confirmação explícita;
- nunca mostra, recupera ou revela a password atual;
- usa o fluxo seguro de reset do fornecedor de autenticação;
- a operação é auditada (`admin.password_reset.requested`).

O Admin não define/mostra uma password atual; não existe superfície que devolva uma password existente.

### 13.12 Alterar email / nome / perfil

Sim:

- **Nome** — editável;
- **Email** — editável (via provisionamento da conta de identidade; o email não é armazenado como rótulo de apresentação; o UUID de autenticação não deve ser mostrado como "Email");
- **Perfil / título de perfil** — editável (seleção de perfil funcional + título em texto livre; o título é visual e nunca concede permissão; mudar templates não altera o perfil);
- **Ativo** — editável.

### 13.13 Alterar módulos de um utilizador

O Admin pode alterar os módulos de um utilizador gerindo o template de acesso associado: associar templates; remover o template previamente associado; trocar o template associado.

O template efetivo associado determina os módulos a que o utilizador pode aceder. Remover a associação de um template de um utilizador não apaga o template globalmente e não afeta outros utilizadores; o template permanece reutilizável.

Esta operação é protegida por validação de grants (regras canónicas de módulo/capability), pela regra do Job On (um template não-admin recebe `jobon.view`; um template admin não) e pelo **self-lockout guard** (o utilizador que faz a alteração não deve remover o último admin ativo).

### 13.14 Gestão de templates de acesso

**Sim — Templates são uma parte funcional atual do Admin.** Não são implementação técnica oculta e devem permanecer no modelo funcional do Admin.

Regra funcional atual:

- Templates são um conceito funcional gerido no Admin, visível e gerível;
- o Admin pode criar/editar templates;
- o Admin pode associar o template a utilizadores;
- o Admin pode remover a associação de templates de utilizadores;
- os templates determinam a disponibilidade de módulos para os utilizadores com que estão associados;
- os templates são reutilizáveis — remover a associação de um utilizador não altera o template nem outros utilizadores;
- a associação é por utilizador.

A gestão de templates é validada canonicamente (grants normalizados/validados contra o catálogo de módulos; módulos desconhecidos, entradas duplicadas e capabilities que não pertencem ao módulo concedido são descartados/rejeitados e reportados).

**Estado recordado do modelo de associação:** o modelo final é **um template efetivo por utilizador**, que transporta o perfil funcional. A regra funcional "perfil ≠ módulos atribuídos; o perfil nunca concede módulos automaticamente" mantém-se inalterada.

### 13.15 Templates: atual, histórico ou superseded

**Regra funcional atual:** Templates são atuais e funcionais. São um conceito gerido no Admin, visível e gerível; o modelo funcional global é:

```
UTILIZADOR → PERFIL + TEMPLATE ASSOCIADO → ACESSO EFETIVO A MÓDULOS
```

O perfil determina *como* o utilizador atua; o template associado determina *quais* módulos ficam visíveis/acessíveis. O perfil nunca concede módulos; a associação de template não altera o perfil.

**Superseded (não tratar como atual):**

- tratar templates como mera implementação técnica oculta — superseded;
- tratar a tab/área Templates como removível/opcional/só apresentação — superseded;
- tratar o acesso a módulos como atribuição direta solta independente de templates — superseded; o modelo funcional é a associação de template por utilizador;
- tratar **História** como módulo de topo / atribuível — superseded; História não é um módulo.

### 13.16 Gestão de aplicações / catálogo de módulos

Na área **Applications** o Admin gere o catálogo de módulos (`applications` = lista de aplicações/módulos):

- lista os módulos disponíveis;
- altera disponibilidade (ativo) e ordem (ordem de apresentação);
- associa módulos/capabilities a templates de acesso existentes;
- desativa em vez de eliminar onde existem registos históricos.

**Fronteira crítica (nunca confundir):**

- **APPLICATIONS** = o catálogo / disponibilidade / ordenação / configuração de módulos. O catálogo **nunca concede acesso**. A autorização é sempre resolvida server-side a partir do catálogo canónico ∩ grants de acesso. Alterar disponibilidade/ordem nesta área não concede nem revoga acesso funcional por si.
- **TEMPLATES ASSOCIADOS A UM UTILIZADOR** = determinam **quais módulos o utilizador pode aceder**.

Estes dois conceitos não devem ser fundidos.

### 13.17 Auditoria / histórico disponível ao Admin

A área **Audit** lê o histórico de ações global, único e append-only. Cada ação de negócio relevante de cada utilizador autenticado torna-se um evento (utilizador, módulo, ação, entidade, timestamp UTC, resultado). O Admin pode consultar este histórico por ano e filtrar por utilizador, módulo, ação, resultado e intervalo de datas, com paginação (20/40/60 linhas), um clique para selecionar e duplo clique para abrir detalhe. O detalhe mostra apenas dados factuais — não é calculada nem apresentada pontuação, ranking, produtividade ou avaliação automática.

Garantias da auditoria:

- **append-only**: correções são linhas novas; o facto original nunca é reescrito/eliminado;
- sem segredos: os eventos nunca incluem passwords, tokens, cookies, credenciais, emails completos, PDFs, imagens ou payloads arbitrários;
- o backend é a fonte autoritativa; o evento é criado server-side.

Divisão de capabilities: `audit.view` controla a visualização; `audit.export` controla a exportação.

**Exportação anual.** A área Audit suporta exportação anual autorizada para CSV (`auditoria-{Year}.csv`), controlada por `audit.export`. A exportação consulta o registo completo para os filtros selecionados e devolve as colunas factuais (data/hora UTC; ano; ator; nome do ator; módulo; código de ação; tipo de entidade; id de entidade; rótulo de entidade; resultado; motivo).

**História não é um módulo autónomo.** A auditoria global permanece um conceito Admin/Audit. Os módulos relevantes podem expor a sua própria tab/área de Histórico; não tratar "História" como um módulo independente.

### 13.18 Rótulos / perfis apresentados

- **Perfis (rótulos funcionais):** Admin, Operador / Controlador, Responsável (exatamente três).
- **Título de perfil / cargo** é um rótulo em texto livre gerido no Admin e mostrado junto ao nome do utilizador no header (ex.: Metrologia, Chefe, Engenheiro, Responsável de qualidade). Se vazio, mostra-se apenas o nome.
- O título em texto livre é **visual apenas** e **nunca concede permissão** e nunca substitui o template, o perfil ou as capabilities. Nunca inferir autorização a partir do título do header.
- Rótulos de módulo: a área Admin é rotulada "Administração". **História não é um módulo** — os módulos relevantes podem expor a sua própria tab/área de Histórico (histórico interno), que não é uma unidade de acesso atribuível.
- Login: sem escolha manual de perfil no login (o servidor decide o landing; utilizador operacional → Job On, Admin puro → Admin).

### 13.19 Criação do primeiro Admin / bootstrap

Não existe admin anónimo/default; o primeiro administrador é criado apenas através de um caminho de bootstrap explícito. O bootstrap:

- cria um template de acesso admin mínimo e o utilizador interno admin ativo ligado a ele, mais um evento de auditoria de bootstrap;
- é **idempotente** (se já existe um admin válido, não faz nada / evita duplicado);
- exige configuração explícita (configuração em falta falha a validação antes de qualquer escrita);
- reconcilia falhas parciais para que nada incompleto seja persistido.

### 13.20 Relação com autenticação (AUTHENTICATION vs ADMIN)

- **AUTHENTICATION** = login / sessão / verificação de identidade. Na aplicação é a superfície de **Login**: sign-in email + password via fornecedor de autenticação, cookie de sessão, logout, `/no-access`, `/access-denied`. É uma área transversal de sistema, não um módulo, e não é Admin.
- **ADMIN** = gestão de utilizadores internos / perfis / templates de acesso / aplicações / auditoria.

O Admin **usa** a parte privilegiada de provisionamento da autenticação (criar contas de identidade, pedir resets de password), mas o Admin não é a autenticação, e a autenticação não é o Admin. Um utilizador que consegue autenticar-se não acede necessariamente ao Admin — alcançar o Admin exige a capability `admin.gerir`.

### 13.21 O que o Admin possui

- **Administração de utilizadores** internos (criar, editar nome/email/título, ativar/desativar, iniciar reset de password, com guarda contra concorrência).
- **Gestão de templates de acesso** (criar/editar; templates são um conceito funcional atual do Admin).
- **Associação de template por utilizador** (associar, remover, trocar; o template determina o acesso a módulos do utilizador).
- **Catálogo de aplicações / módulos** (nome de apresentação, disponibilidade, ordem — apresentação apenas, nunca concede acesso).
- **Consulta + exportação anual de auditoria** sobre o histórico global (ver com `audit.view`, exportar com `audit.export`).
- **Governação administrativa de acesso** — é a única superfície que pode administrar utilizadores/acesso/configuração.
- **Bootstrap gating** — o único caminho de criação do primeiro administrador (sem admin default).

### 13.22 O que o Admin NÃO possui

- **Dados operacionais de produção** de todos os módulos operacionais (planos/revisões do Job On, registos de Controlo, medições de Peso/Pegamentos, registos master de Ferramentas, stock/movimentos do Armazém, movimentos de Boquilhas, registos de Reparação Interna/Externa, saldos/configuração de Tampões).
- **Autenticação / verificação de identidade** (Login + fornecedor de autenticação).
- **Os eventos de auditoria globais** como owner de domínio — o Admin consulta e exporta, mas todos os módulos escrevem na mesma tabela append-only; o Admin não "possui" os registos dos outros módulos.
- **A categoria de acesso operacional** — `admin.gerir` qualifica o admin puro mas não é acesso operacional e nunca implica módulos operacionais.
- O catálogo não concede **acesso** por si próprio.

### 13.23 Superfícies read-only vs editáveis

| Área | Editável? |
|---|---|
| Users list | Lista read-only com ações por linha; criar/editar são formulários editáveis |
| User create/edit | **Editável** (nome, email, perfil, ativo, template associado) |
| Templates list | Lista read-only com ações |
| Template edit | **Editável** (nome, grants, ativo) — guardado (self-lockout, grants canónicos) |
| Applications | **Editável** (disponibilidade + ordem) — apresentação apenas, nunca concede acesso |
| Audit | **Consulta read-only + exportação**; sem edição in-place (append-only) |

Toda a superfície editável do Admin re-autoriza server-side e falha fechado; um botão oculto não é autorização.

### 13.24 Regras negativas

1. **Fail closed** em cada gate Admin — sem identidade ou capability em falta ⇒ proibido.
2. **O título de perfil nunca concede permissão**; nunca inferir autorização a partir do título do header.
3. **Nunca mostrar/revelar uma password atual**; o reset requer confirmação explícita, usa o fluxo seguro de autenticação e nunca revela uma password.
4. **O UUID de autenticação não deve ser rotulado "Email".**
5. **Módulo não concedido através do template** ⇒ não aparece na navegação + não é funcionalmente acessível + não é diretamente alcançável (barreira de acesso, não ocultação de UI).
6. **Admin puro não recebe Job On / módulos operacionais**; `admin.gerir` não é acesso operacional.
7. **O perfil nunca concede módulos automaticamente** (todos os três perfis).
8. **Exatamente três perfis** — sem quarto, sem read-only, sem perfil de gestão/metrologia/consulta.
9. **Auditoria append-only** — sem UPDATE/DELETE; correções são linhas novas.
10. **Self-lockout guard** — o último admin ativo não pode ser desativado/despromovido.
11. **Sem admin anónimo/default** — bootstrap apenas via configuração explícita.
12. **Templates são um conceito funcional atual do Admin** — visíveis/geríveis, não removíveis.
13. **Login sem escolha manual de perfil** e sem confirmar se um email específico existe.
14. **O catálogo nunca concede acesso** — alterações de apresentação/ordem/disponibilidade não alteram autorização.
15. **Não tratar botões/ocultação como autorização** — o servidor valida no comando/serviço.
16. **Perfis e templates são separados** — associar/remover templates nunca altera o perfil funcional; o perfil nunca concede módulos.
17. **Remover a associação de um template de um utilizador não apaga o template** — os templates são reutilizáveis e os outros utilizadores não são afetados.

### 13.25 Questões em aberto

Não existem questões Owner genuínas não resolvidas para a funcionalidade Admin. O modelo global de módulos/perfis e o modelo Admin / Users / Access estão fechados e confirmados: templates funcionais e visíveis; separação perfil vs templates; associação de template por utilizador; História não é um módulo de topo.

O único item conhecido é um defeito de robustez a corrigir: a lista de Users do Admin mostra o UUID de autenticação sob uma coluna "Email" — o comportamento pretendido é nunca mostrar o UUID de autenticação como um rótulo de Email. Não é uma questão Owner, é um defeito a corrigir.

---

<a id="p3-shell"></a>
# Parte III — Aplicação Transversal

## 14. Shell, Navegação e Design System

### 14.1 Propósito

A aplicação funciona como uma aplicação web única e coerente, organizada em torno de uma **shell única e persistente**. A navegação entre módulos e vistas mantém uma experiência contínua sem transformar os módulos em aplicações separadas.

Hierarquia de autoridade:

1. clarificações explícitas e mais recentes do Owner;
2. o modelo canónico do design;
3. os contratos aceites do design system;
4. evidências técnicas de implementação.

Os catálogos canónicos de módulos e páginas são a fonte única de verdade, validados no arranque da aplicação. A navegação ocorre em dois níveis: Nível 1 (Módulos/Tarefas) e Nível 2 (Vistas internas do módulo). Toda a auditoria cross-module é registada de forma imutável (append-only).

### 14.2 Superfícies transversais

Nem tudo na aplicação é um módulo de negócio. Existem superfícies transversais que servem todo o sistema:

- **Login / Auth:** superfície de entrada e gestão de sessão.
- **Users / Access:** gestão de utilizadores, templates e auditoria (alojado no módulo Admin).
- **História:** superfície de consulta transversal de auditoria.
- **Design Laboratório:** superfície técnica de validação de design.

### 14.3 Shell global, header e navegação

A Shell é o invólucro comum a todas as sessões autenticadas:

```text
APP SHELL
├─ Header Global          (Logo, identidade, utilizador, logout)
├─ Navegação Primária     (Módulos concedidos à esquerda, Admin à direita)
├─ Título da Vista Ativa  (Cabeçalho da página do módulo)
├─ Toolbar / Filtros      (Ações globais da vista atual)
├─ Área de Trabalho       (Conteúdo operacional do módulo)
├─ Side Panel             (Painel de contexto lateral, quando aplicável)
└─ Camada de Feedback     (Modais, Toasts, Loading)
```

O **Header** mantém a identidade da aplicação e o contexto do utilizador: Logo (liga à raiz/landing operacional); identidade da página (título do módulo ou vista atual); utilizador (nome autenticado + título/função em texto livre); Logout. O Header é resolvido e renderizado server-side; os módulos não injetam dados de identidade diretamente no HTML do Header.

A **Navegação de Nível 1** é derivada server-side da interseção entre os grants do utilizador e o catálogo canónico. Operacionais alinhados à esquerda; Administração/Definições à direita. Tabs não executam comandos de negócio; servem apenas para trocar a vista ativa dentro do módulo. A tab ativa é indicada por tipografia e cor de destaque. Módulos não concedidos não existem na barra de navegação.

A **Navegação de Nível 2** ocorre dentro do módulo. O Nível 1 permanece sempre visível e acessível; entrar numa subárea nunca remove a capacidade de sair para outro módulo. As tabs internas podem variar dinamicamente consoante o perfil funcional. O design, espaçamento e estados das tabs são herdados do design system global.

### 14.4 História

A **História** é uma superfície transversal de leitura (read-only):

- apresenta e consulta eventos de auditoria cross-module;
- **não** é um módulo funcional atribuível;
- **não** é dona dos eventos de origem; os módulos operacionais mantêm a propriedade dos seus factos;
- o acesso é restringido pelo escopo efetivo dos módulos concedidos ao utilizador: o utilizador só vê eventos pertencentes às áreas a que tem acesso. Eventos administrativos continuam sujeitos às permissões específicas de auditoria.

### 14.5 Design Laboratório

O **Design Laboratório** é uma superfície técnica transversal de validação e regressão do design system:

- permanece permanentemente disponível para validação visual, de acessibilidade e de responsividade;
- **não** é um módulo funcional de negócio;
- **não** faz parte da navegação operacional diária dos utilizadores de chão de fábrica;
- não possui registos de negócio, workflows ou persistência de dados (usa apenas dados de demonstração).

### 14.6 Área principal de trabalho e side panel

A área principal hospeda o fluxo de trabalho do módulo. Anatomia canónica de uma página:

1. **Page Header:** título e descrição da vista.
2. **Ações Primárias / Filtros:** botões de criação e cartões de pesquisa.
3. **Contexto / Métricas:** sumários essenciais.
4. **Conteúdo Principal:** tabelas, listas ou formulários.
5. **Barra de Ações de Seleção:** ações que dependem de um item selecionado na tabela.

Exceções à anatomia (ex.: ecrãs de consulta rápida, master-detail) devem ser justificadas no design do módulo. Nunca ocorre scroll horizontal ao nível da página inteira; o scroll confina-se a cartões ou tabelas específicas quando inevitável.

O **Side Panel** é um frame global para contexto lateral contínuo: fixo no desktop (base escura, foco no estado operacional atual); transforma-se em gaveta (drawer) ou bloco recolhível em mobile. O clique num cartão abre o registo associado, conforme a regra do módulo. Conflitos são exibidos no próprio cartão, sem pop-ups bloqueantes. O Side Panel nunca substitui a navegação principal.

### 14.7 Componentes globais

| Componente | Contrato global | O módulo fornece |
|---|---|---|
| **Botões** | Estados (hover, focus, disabled, loading). Bloqueio de duplo-clique. | Verbo, posição, variante semântica. |
| **Campos (Inputs)** | Label acima, erro abaixo, limites de casas decimais. | Formatos, validações de domínio. |
| **Pills / Status** | Texto + cor semântica. Cor nunca é o único indicador. | Significado do estado de negócio. |
| **Dropdowns** | Pesquisa incremental, navegação por teclado, estado "Sem resultados". | Catálogos e fontes de dados. |
| **Alertas** | Inline junto ao problema. Info, Success, Warning, Danger. | Mensagem e ação de recuperação. |

### 14.8 Tabelas e listas

- **Um clique:** seleciona a linha (estado visual explícito e `aria-selected`).
- **Duplo clique:** abre o registo, detalhe ou editor (o módulo define o que "abrir" significa).
- **Ações de seleção:** botões que atuam sobre a linha selecionada residem fora da tabela, na barra de ações. Não há botões de ação repetidos em cada linha.
- **Mudanças de contexto:** aplicar um filtro ou mudar o tamanho da página limpa qualquer seleção invisível e retorna à página 1.

Contrato de teclado: `Space` seleciona a linha focada; `Enter` abre a linha focada; `Arrow Up`/`Arrow Down` move o foco entre linhas; `Home`/`End` move o foco para a primeira/última linha da página. Mover o foco não seleciona automaticamente a linha. `Ctrl+Enter` não faz parte do contrato canónico.

### 14.9 Ordenação e paginação

Ordenação: apenas colunas explicitamente marcadas como ordenáveis; primeiro clique ascendente (ASC); segundo clique descendente (DESC); cliques subsequentes alternam; apenas uma ordenação primária ativa por vez; mudar a ordenação reinicia a paginação para a página 1; a ordenação aplica-se à consulta completa (server-side), não apenas aos dados da página visível; o módulo define as colunas ordenáveis e a ordenação padrão; a ordenação não é persistida entre sessões por defeito.

Paginação: tamanhos padrão 20, 40, 60 registos por página; exibe o total de registos e a página atual; limites desativados quando não aplicável.

### 14.10 Formulários e validação

- **Estrutura:** Label (sempre visível) → Controlo → Helper → Mensagem de Erro.
- **Validação:** erros de obrigatórios e formato exibidos imediatamente abaixo do campo. Formulários extensos exibem um sumário de erros no topo.
- **Formulários extensos:** abrem inline (cartão expansível) com foco no primeiro campo. "Cancelar" limpa rascunhos. O estado "dirty" exige confirmação se o utilizador tentar sair sem guardar.
- **Layout:** agrupamento por tarefa; números alinhados com unidades; Cancelar precede Guardar.

### 14.11 Calendário

Existe um único componente de calendário partilhado por todos os módulos: semana começa à segunda-feira; um clique seleciona/filtra o dia; mudar de mês nunca auto-seleciona uma data; dias com registos exibem um ponto discreto; o dia atual é marcado distintamente; a ação "Mostrar todas" remove o filtro de data. O módulo é responsável por fornecer as datas que contêm registos e o significado de negócio da seleção.

### 14.12 Modais e confirmações

- A aplicação **não** utiliza APIs nativas do browser (`alert`, `confirm`, `prompt`).
- **Uso:** confirmação de ações destrutivas, perda de alterações (dirty-state), reset de passwords ou ações rápidas focadas.
- Formulários extensos ou criação de registos complexos ocorrem inline, não em modais.
- **Comportamento:** focus trap ativo; `Escape` ou clique no backdrop pode fechar o modal quando não existe trabalho por guardar; se existir estado dirty, aplica-se a confirmação de perda de alterações; ao fechar, o foco regressa ao elemento que abriu o modal; a submissão bloqueia cliques repetidos; falhas de rede mantêm o modal aberto e preservam o input do utilizador.

### 14.13 Feedback e toasts

| Situação | Componente | Persistência |
|---|---|---|
| Campo inválido | Field Error (Inline) | Até corrigir |
| Múltiplos erros | Summary + Field Errors | Até corrigir |
| Sucesso simples | Toast (`aria-live`) | Temporário (auto-dismiss) |
| Save falhado | Erro inline persistente + Toast opcional | Inline até correção/novo intento |
| Load falhado | Error State com Retry | Persistente |
| Comando em curso | Loading no trigger | Até conclusão |
| Ação destrutiva | Confirmation Dialog | Bloqueante |

O sucesso só é emitido após autorização, validação e persistência confirmada. Ações de consulta ou filtro nunca geram toasts de sucesso.

### 14.14 Loading / empty / error

- **Loading:** skeleton ou spinner que preserva o layout.
- **Empty:** mensagem explicativa e botão de "próximo passo". Sem áreas tracejadas vazias.
- **No Results:** resultado de um filtro ativo. Botão para "Limpar filtros".
- **Error:** falha de rede ou servidor. Mensagem clara e botão "Tentar novamente".
- **Forbidden:** acesso negado. Mensagem de contacto ao administrador.

A apresentação visual é global; a condição que dispara o estado é determinada pelo serviço do módulo.

### 14.15 Responsive / mobile

Breakpoints de referência: 1200px, 980px, 720px. Grelhas reordenam-se antes de reduzir o tamanho do texto; campos essenciais são preservados. Em viewports pequenas, tabelas largas ganham scroll horizontal interno (confina-se ao cartão), nunca à página inteira. O Side Panel transforma-se em drawer deslizante ou bloco recolhível. Áreas de toque cumprem os mínimos de acessibilidade.

### 14.16 Acessibilidade

Alvo mínimo WCAG AA: foco visível e lógico; operação completa por teclado; `aria-label` em botões de ícone único; `aria-expanded`/`aria-controls` em secções expansíveis; `aria-live` para feedback dinâmico; respeito pela preferência de movimento reduzido; a cor nunca é o único meio de transmitir informação ou estado.

### 14.17 Design tokens e CSS global

Consistência visual garantida por tokens CSS (`--dmo-*`) que definem cores, espaçamentos, raios, sombras e tipografia.

- **Load order:** Tokens → Foundation → Components → Layout → Utilities.
- **CSS de módulo:** apenas CSS de composição (grelhas, ordem, larguras específicas).
- Proibido criar novos códigos hexadecimais quando existe um token de cor de marca ou semântico.
- Proibido criar implementações paralelas de componentes que já existem no design system global.
- Estilos inline (`style="..."`) para design são proibidos.

### 14.18 JavaScript e interações globais

Os scripts globais fornecem os contratos de interação (seleção de listas, foco, navegação por teclado, calendários).

- O JavaScript global **não contém lógica de negócio**.
- Os módulos injetam dados e significados através de atributos `data-*` que os scripts globais leem para orquestrar a interface.
- A lógica de domínio reside estritamente no servidor ou em scripts de módulo isolados.

### 14.19 Autorização vs apresentação

- A navegação reflete o acesso, mas **ocultar um botão ou tab não é autorização**.
- Toda a ação sensível ou acesso a dados exige validação server-side (fail-closed).
- Deep-links para módulos não atribuídos resultam numa página de "Acesso Negado", não num erro 404 genérico.
- Atributos HTML de apresentação servem apenas para orquestrar a UI; a autoridade real reside no servidor.

### 14.20 Responsabilidade global vs módulo

| Preocupação | Shell global / design system | Módulo de negócio |
|---|---|---|
| Header e navegação | Frame, renderização, acessibilidade | Disponibilidade (via grants), títulos |
| Tabelas e listas | Interação, teclado, paginação, estados | Dados, colunas, significado de "abrir" |
| Formulários | Layout, validação visual, focus trap | Campos, regras de domínio, submissão |
| Side Panel | Frame, responsividade, drawer | Dados contextuais, regras de clique |
| Autorização | Infraestrutura, fail-closed, routing | Verificações funcionais específicas |
| Ownership | Nenhuma | Domínio e regras de negócio |

### 14.21 Princípios transversais de domínio

- **Aviso ≠ bloqueio:** um aviso não bloqueia automaticamente um fluxo, a menos que a regra de negócio o dite explicitamente.
- **Estado técnico ≠ estado físico:** um movimento físico no armazém não deve mutar silenciosamente o estado técnico sem o workflow adequado.
- **UI entry point ≠ ownership:** iniciar uma operação a partir da UI de um módulo não transfere a propriedade do registo master.

### 14.22 Regras negativas da aplicação

- **Não** criar design systems paralelos ou concorrentes dentro de módulos.
- **Não** simular persistência de negócio no Design Laboratório.
- **Não** colocar lógica de negócio ou domínio dentro de scripts globais de UI.
- **Não** confiar na ocultação de CSS/HTML como mecanismo de segurança ou autorização.
- **Não** utilizar APIs nativas de bloqueio (`alert`, `confirm`, `prompt`).
- **Não** permitir scroll horizontal ao nível da página inteira.
- **Não** inventar novos perfis funcionais fora dos 3 canónicos.
- **Não** promover a superfície de História ou Design Laboratório a módulos operacionais atribuíveis.
- **Não** reabrir áreas internas (ex.: Peso, Pegamentos) como módulos de topo independentes.

---

<a id="p3-documentos"></a>
## 15. Documentos e Impressão

Os documentos e a impressão são tratados por módulo. Resumo do comportamento funcional confirmado:

### 15.1 Job On

O Job On produz os documentos/impressão de produção. As folhas conhecidas incluem Ficha de Artigo, Job-On Moldes, Trabalho de Equipa e a folha duplicada/variante onde aplicável. As folhas impressas são documentos operacionais e refletem o contexto exato de produção/referência. A impressão do Job On nunca consulta dados live para substituir valores do snapshot.

### 15.2 Controlo

O documento oficial do Controlo é o registo estruturado/snapshot, não um ficheiro solto sem contexto. O PDF é derivado, imprimível e regenerável, mas não é a fonte de verdade. O envio de documentos é explícito e confirmado, orientado por contexto de Máquina/Linha, e nunca acontece automaticamente só porque o controlo foi concluído ou aprovado. A estrutura de diretórios segue o princípio Reference antes de Production (Peso / Pegamentos / Resumo), com criação/reutilização idempotente das pastas inferiores.

### 15.3 Pegamentos

Os Pegamentos usam o mesmo princípio de documento do Controlo: o registo estruturado é autoritativo e o PDF é derivado (apresentação/impressão). O conteúdo e a aparência do PDF de Pegamentos seguem o fluxo funcional de Pegamentos.

### 15.4 Armazém

A impressão da lista de recolha (Saída programada) é opcional; imprimir não altera o estado da lista nem liberta posições; o fluxo deve ser executável integralmente no computador sem impressão. Etiquetas/labels de posição e exportação PDF/CSV de stock/movimentos não estão presentes na V1.

### 15.5 Reparação Externa

A lista programada fica disponível no Armazém e pode ser impressa (suporte V1). PDF/etiqueta/exportação não estão especificados na autoridade funcional do módulo.

### 15.6 Reparação Interna

A RI não tem documentos, impressão, PDF ou exportação.

### 15.7 Nomenclatura e fluxo do operador

A nomenclatura e o fluxo do operador de cada documento pertencem ao respetivo módulo (acima). Estas são as regras funcionais dos documentos. A forma concreta de persistência dos documentos não é, por si, uma regra funcional.

---

<a id="p4-abertas"></a>
# Parte IV — Questões em Aberto

## 16. Questões Funcionais em Aberto

As questões abaixo permanecem genuinamente em aberto e requerem decisão do Owner. Não devem ser resolvidas por suposição nem inferidas da implementação.

### 16.1 Job On ↔ Tampões — TP/Tampão (conflito preservado)

**Questão:** o **TP/Tampão** (com PU e CS) é configuração específica de produção do Job On (§5.7.1), ou pertence ao módulo autónomo Tampões (§12)?

- §5.7.1 afirma que PU / CS / TP são configuração específica de produção do Job On, configurados manualmente pelo Responsável.
- §12 afirma que Tampões é autónomo, sem relação com Job On / Produção / Referência / Planeamento.

Ambas as afirmações são preservadas integralmente; a sua reconciliação é decisão do Owner. Não é resolvida neste manual e não deve ser implementada em nenhum dos sentidos até decisão explícita.

### 16.2 Job On — questões funcionais

1. **Siglas de família** — significado oficial/apresentado (expansão) das siglas de família (MP/CM, MF, BQ, PU, CAL, AN, ARR, PI, CS, TP, FO) e a lista final de campos obrigatórios por família.
2. **Estados do ciclo de vida do Job On** — o conjunto real de estados de ciclo de vida (rascunho / planeado / em fabrico / fechado / cancelado?).
3. **Template "Novo em branco"** — que famílias/campos devem aparecer num Job On "Novo em branco".
4. **Duplicar anterior** — a ordenação canónica ("anterior") usada por Duplicar anterior e a informação mínima que permite identificar um Job On histórico na lista de duplicação.
5. **Conclusão/cancelamento de verificações** — se as verificações precisam de prioridade, comentário de conclusão ou cancelamento (reset/correção num Job On já fechado; se a confirmação exige comentário; se Apagar exige motivo; comportamento de pendentes já criados quando uma regra é desativada).
6. **Stock vs quantidade** — o significado de negócio de stock e quantidade nos cartões de máquina/necessárias, onde isto afeta o fluxo do utilizador.
7. **Tipo vs Processo** — a relação de negócio entre Tipo e Processo no Job On, se for uma distinção de negócio real.
8. **Snapshot de `% usage` no Job On** — se o Job On mantém um snapshot de produção/revisão do valor de utilização e, se sim, em que momento funcional/contexto esse snapshot representa o valor. (Não reabrir o ownership de `% usage` por Ferramentas.)
9. **Movimentos de dias passados no calendário** — a origem dos movimentos in/out de dias passados mostrados pelo calendário do Job On.
10. **Elegibilidade de lote no seletor** — que estados de lote são elegíveis no seletor CM/MF/BQ (ativo / disponível / histórico).

### 16.3 Ferramentas — questões funcionais

1. **Numeração de lote** — existe uma regra de negócio para a numeração visível do lote? Deve começar em 1? Deve ser sequencial? São permitidos intervalos? Qual o seu âmbito de unicidade?
2. **Alteração de estado técnico — motivo** — o motivo é obrigatório quando o Responsável altera o estado técnico?
3. **Fluxo reparação / estado técnico** — quem altera uma ferramenta para `Por reparar`? É sempre uma ação manual do Responsável a partir da ficha/detalhe? Quando uma reparação é concluída, o que acontece ao estado técnico? A ferramenta permanece `Por reparar` até o Responsável a alterar manualmente para `Reparado`?
4. **Duplicação de verificações** — quando se usa "Novo lote a partir deste", as regras de verificação inativas/desativadas também são copiadas?
5. **Campos master reais de Ferramentas** — existem campos master reais de Ferramentas para além do conjunto central confirmado (Tipo, identificação interna, Lote, Máquinas/Linhas, Owner/Plant, estado técnico, % utilização, configuração de verificação) que os utilizadores realmente registam? (Não perguntar sobre campos de folha impressa só porque aparecem em impressões do Job On.)
6. **Sincronização do `Estado` na Entrada** — se o `Estado` da Entrada do Armazém (`Reparado | Por reparar | Novo`) sincroniza com a condição técnica do domínio Ferramentas ou permanece apenas contexto do movimento.

### 16.4 Armazém — questões funcionais

1. **Distribuição exata de ações por perfil** — a existência da divisão Operador / Responsável está fechada; permanece em aberto a distribuição exata das ações específicas do Armazém (que ações são exclusivas do Responsável; criação/gestão de posições; determinadas ações de histórico/rastreio; aprovação/cancelamento de registos físicos; que correções/configurações o Operador pode fazer).
2. **Tabs / Programadas — alvo funcional** — qual é o alvo funcional da tab Programadas; se a estrutura final de áreas deve ser 2 tabs ou 4 tabs; se Programadas deve ter fluxo completo com checkboxes, apenas indicação/shell, ou lista pendente perspetivada.
3. **Destino da Saída — obrigatoriedade** — está fechado que os destinos operacionais são Fabricação / Reparação / Sucata; permanece em aberto se o Destino é obrigatório em todas as Saídas ou opcional.
4. **Estado na Entrada — classificação exata** — read-only técnico? contexto apenas do movimento? campo que não pertence ao Armazém? Está fechado que o Armazém não deve alterar silenciosamente o estado técnico, e que a resposta não deve ser escolhida a partir da implementação atual.

### 16.5 Detalhes funcionais adiados

Detalhes não bloqueantes, adiados:

- **Lista de Saída Programada:** cancelamento da lista; quem pode cancelar; adicionar/remover linhas após publicação; confirmação final automática pelo último check vs ação adicional; encerrar/cancelar linha que não regressa; motivo obrigatório para linha que não regressa.
- **Reparação Externa:** `CancelarLista`; fecho com itens em aberto / destino; regra específica para mismatch de reparador errado; detalhe adicional de transições de estado além do ciclo documentado.
- **Reparação Interna:** formato/intervalo do número individual de CM/MF; futura exigência de observação/motivo; mais de um lote do mesmo tipo ativo na Linha; campo "turno"; representação exata da anulação em listas/consultas; política de relógio/offset de fábrica (DST).
- **Job On:** definições pormenorizadas de impressão por folha.

> **Nota de método.** Estas questões não devem ser resolvidas por reinterpretação de documentos antigos, planos, relatórios ou desenhos históricos. Permanecem em aberto até decisão explícita do Owner.
