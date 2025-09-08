# Requisitos Funcionais

Financeiro / Dashboard

- RF-FIN-001 Cadastrar despesas (com status e exibição no extrato/dashboard)
- RF-FIN-002 Cadastrar receitas (com status e impacto no saldo)
- RF-FIN-003 Séries recorrentes (despesa/receita)
- RF-FIN-4 Parcelamento de despesas (N parcelas)
- RF-FIN-005 Transferência interna entre contas
- RF-FIN-006 Transferência externa (Pix/TED) com 2FA
- RF-FIN-007 Limites de transferência e regras AML
- RF-FIN-008 Validação bancária (Pix, agência/conta, CPF/CNPJ)
- RF-FIN-009 Dashboard financeiro (saldo, receita, despesa, resultado, % orçamento, gráficos)
- RF-FIN-010 Filtro de período (mês/ano e intervalo customizado)

Cassino / Jogos

- RF-CAS-001 Catálogo de jogos e filtro por tipo
- RF-CAS-002 Iniciar jogo (tempo de carregamento, saldo/limites visíveis)
- RF-CAS-003 Restrições de acesso (geofencing/idade/manutenção)
- RF-CAS-004 Limites de aposta (definir, aplicar, 2FA, período de reflexão)
- RF-CAS-005 Bloqueio ao exceder limites + mensagens
- RF-CAS-006 Histórico de apostas com filtros e totais
- RF-CAS-007 Exportação do histórico (CSV)
- RF-CAS-008 Modo demonstração (créditos virtuais, rótulo DEMO)
- RF-CAS-009 Troca de DEMO para saldo real com confirmação

NFT

- RF-NFT-001 Criar (mintar) NFT de pagamento com 2FA
- RF-NFT-002 Detalhes on-chain (hash, status, emissor, validade)
- RF-NFT-003 Regras de expiração/uso único
- RF-NFT-004 Checagem de saldo para taxas de rede (gas)
- RF-NFT-005 Compartilhar por QR/Link (validade e nº de usos)
- RF-NFT-006 Marketplace: publicar anúncio (preço/royalties)
- RF-NFT-007 Comprar/vender NFT com royalties automáticos
- RF-NFT-008 Cancelar anúncio e retorno ao inventário
- RF-NFT-009 Tratativa de falhas on-chain (estorno/status)

Cripto

- RF-CRY-001 Cotações em tempo real (moedas principais)
- RF-CRY-002 Conversão de moeda base (BRL/USD)
- RF-CRY-003 Atualização automática periódica
- RF-CRY-004 Alertas de preço (push/e-mail)
- RF-CRY-005 Comprar/vender cripto com prévia (taxa/slippage)
- RF-CRY-006 Gate KYC (operações condicionadas a verificação)
- RF-CRY-007 Bloqueios por limites/regra AML
- RF-CRY-008 Portfólio consolidado (saldos, P/L, distribuição)
- RF-CRY-009 Exportação do portfólio (CSV)

Aplicativo

- RF-APP-001 Notificações de sistema (sucesso/falha, progressos)
- RF-APP-002 Validação de formulários e mensagens amigáveis
- RF-APP-003 Estados de carregamento/vazio/erro padronizados

---

# Matriz de rastreabilidade — Estórias <-> Requisitos Funcionais

- US001 (Dashboard financeiro) → RF-FIN-009, RF-FIN-010, RF-APP-003

- US002 (Nova Despesa) → RF-FIN-001, RF-FIN-003, RF-FIN-004, RF-APP-002

- US003 (Nova Receita) → RF-FIN-002, RF-FIN-003, RF-APP-002

- US004 (Transferir) → RF-FIN-005, RF-FIN-006, RF-FIN-007, RF-FIN-008, RF-APP-001, RF-APP-002

- US005 (Escolher jogos) → RF-CAS-001, RF-CAS-002, RF-CAS-003, RF-CAS-004

- US006 (Limites de aposta) → RF-CAS-004, RF-CAS-005

- US007 (Histórico de apostas) → RF-CAS-006, RF-CAS-007

- US008 (Modo demonstração) → RF-CAS-008, RF-CAS-009

- US009 (NFT de pagamento “rápido/divertido”) → RF-NFT-001, RF-NFT-002, RF-NFT-003, RF-NFT-004, RF-APP-001

- US010 (Criar NFT – campos/validação) → RF-NFT-001, RF-APP-002

- US011 (Enviar NFT por QR/Link) → RF-NFT-005, RF-NFT-002

- US012 (Marketplace NFT) → RF-NFT-006, RF-NFT-007, RF-NFT-008, RF-NFT-009, RF-APP-001

- US013 (Ver preços de cripto) → RF-CRY-001, RF-CRY-002, RF-CRY-003, RF-APP-003

- US014 (Alertas de preço) → RF-CRY-004, RF-APP-001

- US015 (Comprar/Vender cripto) → RF-CRY-005, RF-CRY-006, RF-CRY-007, RF-APP-002

- US016 (Portfólio consolidado) → RF-CRY-008, RF-CRY-009, RF-APP-003
