# CURRENT STATE

Status da implementação face à referência funcional (`MANUAL.md`).

## IMPLEMENTED

- **Perfis e acesso:** exatamente três perfis (Admin, Operador / Controlador, Responsável); módulos atribuídos individualmente por utilizador via template; perfil ≠ módulos; navegação derivada dos grants; autorização server-side fail-closed; landing Job On para operacionais e Admin para Admin puro.
- **Job On:** criação, duplicação, alteração de data na mesma identidade, gravação de nova revisão, associação de ferramentas CM/MF/BQ, confirmação de verificações, ciclo de vida (rascunho → planeado → em fabrico → fechado / cancelado), impressão de produção, imagem de artigo associada à referência (não por revisão).
- **Controlo:** módulo único com áreas internas (Resumo, Peso, Comparação, Pegamentos, Histórico); cálculo de Peso server-side (Capacidade/Volume, peso do vidro); fluxo Rascunho → Submetida → Aprovada/Rejeitada com reabertura; Comparação como workflow dentro do Peso, com decisão individual por CM (Manter / Colocar de parte, com justificação) e sem alterar os factos originais; Pegamentos com medição de um só eixo; Resumo / Folha de Controlo com CM/BQ/MF/PU/CS a partir do contexto do Job On; PDF.
- **Ferramentas:** master CM/MF/BQ/PU/CS, identidade, lotes, "Novo lote a partir deste", peças/condição, regras de verificação, % utilização manual.
- **Armazém:** Entrada/Repor, Saída/Retirar, destino operacional, reparador na Saída → Reparação, correção de localização, "+ Criar novo", consulta/pesquisa, histórico append-only.
- **Boquilhas:** lotes BQ, movimentos de reparação externa, saldos, reparadores por linha, discrepância de retorno a mais não-bloqueante, snapshot de fecho.
- **Reparação Interna:** registo por Linha + Tipo (CM/MF) + número individual; contexto automático 06:00/09:00; correção append-only; anulação auditável; BQ não registável.
- **Reparação Externa:** batches CM/MF, itens, disponibilizar, recolha/retorno com confirmação física atómica, diretório de reparadores, defaults por linha.
- **Tampões:** tabela de configurações (Máquina(s) + Diâmetro + Calote), quantidades agregadas, movimentos, categorias opcionais, histórico, gestão de campos/opções.
- **Admin:** utilizadores (criar/editar/ativar/desativar), templates de acesso, aplicações, auditoria com exportação anual, bootstrap do primeiro admin, reset de password, self-lockout guard. Lista de Users já não mostra o UUID de autenticação como Email (degrada para "Email indisponível").
- **História:** superfície transversal read-only; não atribuível.
- **Shell / design system:** shell única, header, navegação de dois níveis, componentes, tabelas/listas, calendário, modais, toasts, estados loading/empty/error, tokens, acessibilidade. Design Laboratório disponível como superfície técnica.

## PARTIAL

- **Armazém — Saídas Programadas:** a superfície existe mas o fluxo completo (checkboxes de recolha, fecho atómico) não está totalmente ativo; alvo funcional da tab em aberto.
- **Armazém — BQ:** o modelo normal suporta CM/MF/BQ na camada de serviço; a materialização completa de BQ em toda a superfície é reconciliação técnica.
- **Job On — materialização de campos:** existem controlos visíveis cuja persistência (payload → serviço → persistência → recarga → impressão) não está comprovada para todos os campos; a completude da impressão de 4 folhas é uma lacuna de aceitação.
- **Verificações — geração inicial:** a confirmação está implementada; a geração inicial de ocorrências a partir das regras de Ferramentas numa nova associação precisa de verificação end-to-end.
- **Tampões — Planeamento:** existe estrutura de Planeamento na implementação, apesar de o modelo funcional a classificar como superseded; conflito por resolver.

## NOT IMPLEMENTED

- **Reparação Externa — `CancelarLista`:** adiado; o estado Cancelado é apenas compatibilidade de schema.
- **Armazém — exportação/etiquetas:** exportação PDF/CSV de stock/movimentos e etiquetas de posição não presentes na V1 (por definição).
- **Reparação Interna — documentos:** a RI não tem documentos, impressão, PDF ou exportação (por definição).
- **Design Laboratório — persistência de negócio:** não tem registos de negócio nem persistência real (por definição).

## CURRENT WORK

- **Fase M1 — correções funcionais e estruturais:** consolidação do modelo `tool_id` / `jobon_id` / `cm_id` / `mf_id` / `bq_id`; relações produção ↔ ferramenta explícitas; correções de ownership (Peso/CM, Pegamentos/produção, Resumo/produção); remoção de responsabilidades duplicadas/superseded; restauração de comportamento funcional que regrediu.
- **Regressões Job On em recuperação:** naming do módulo de topo de produção, Controlo não como subtab de Planeamento, máquina/linha editável, campos BQ/PU/Pinças/Calibres corretos e duplicáveis, tamanho do seletor de produção, menu `...` acima do header.
- **Decisões de Owner em falta** (ver `MANUAL.md` §16): conflito TP/Tampão ↔ Tampões; questões Job On, Ferramentas e Armazém; detalhes adiados de Saídas Programadas, Reparação Externa e Reparação Interna.
