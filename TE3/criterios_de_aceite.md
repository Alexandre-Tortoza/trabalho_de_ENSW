# Critérios de aceite

[US001]
[Dado que] estou na página principal do aplicativo, autenticado e com o mês atual
[Quando] a página carrega
[Então] devem aparecer Saldo Atual, Despesa do Mês, Receita do Mês, Resultado do Mês, % do orçamento e gráficos de linha, pizza por categoria e barras por mês do período.

[Dado que] altero o filtro de período (mês/ano ou intervalo customizado)
[Quando] aplico o filtro
[Então] todos os dados e gráficos são recalculados para o novo período.

[Dado que] não existem lançamentos no período selecionado
[Quando] carrego a página
[Então] devo ver estados vazios com CTA para “Adicionar receita”.

[Dado que] a API está respondendo
[Quando] os dados ainda estão sendo buscados
[Então] devo ver skeletons/loader; se a busca falhar, devo ver mensagem de erro e botão “Tentar novamente”.

---

[US002]
[Dado que] estou no formulário de “Nova Despesa”
[Quando] informo valor > 0, data, categoria (obrigatória), conta (opcional), descrição (opcional) e salvo
[Então] o lançamento é criado como `PENDING` e aparece no extrato e no dashboard do período.

[Dado que] marco “Pago agora”
[Quando] salvo a despesa
[Então] o status fica `PAID` com data de pagamento = hoje e o saldo da conta selecionada é atualizado imediatamente.

[Dado que] escolho “Recorrente mensal”
[Quando] salvo a despesa
[Então] é criada uma série mensal com próxima ocorrência no mesmo dia dos meses seguintes até o fim definido (ou indefinido).

[Dado que] escolho “Parcelado em N vezes”
[Quando] salvo a despesa
[Então] são criadas N parcelas identificadas por “1/N … N/N”.

[Dado que] tento salvar sem valor ou com valor ≤ 0 ou sem categoria
[Quando] envio o formulário
[Então] o submit é bloqueado e são exibidas mensagens de validação nos campos.

---

[US003]
[Dado que] estou no formulário de “Nova Receita”
[Quando] informo valor > 0, data, origem/categoria, conta e salvo
[Então] o lançamento é criado como `RECEIVED` se a data ≤ hoje, ou `PENDING` se a data for futura; o saldo é atualizado quando o status estiver `RECEIVED`.

[Dado que] marco “Recorrente mensal” (ex.: salário)
[Quando] salvo
[Então] é criada uma série mensal no mesmo dia até cancelamento.

---

[US004]
[Dado que] seleciono “Transferir entre contas”
[Quando] informo conta de origem ≠ conta de destino, valor > 0 e data
[Então] são criados dois lançamentos: saída na origem e entrada no destino.

[Dado que] seleciono “Transferência externa (Pix/TED)”
[Quando] informo destinatário (salvo ou novo), dados bancários válidos (Pix/Agência-Conta, CPF/CNPJ), valor e confirmo 2FA
[Então] a ordem é criada com status `CREATED` → `PROCESSING` → `COMPLETED` ou `FAILED`, e recebo notificação do resultado.

[Dado que] meus limites diário/mensal foram excedidos
[Quando] tento confirmar a transferência
[Então] a operação é bloqueada, com mensagem do motivo e a data/valor de liberação.

[Dado que] informo dados bancários inválidos
[Quando] tento avançar
[Então] vejo mensagens de validação específicas (formato de chave Pix, agência/conta, CPF/CNPJ).

---

[US005] — Escolher entre os diferentes jogos da plataforma
[Dado que] estou na área de Cassino, autenticado e em região permitida
[Quando] abro a lista de jogos ou uso o filtro por tipo
[Então] devo ver um menu com os jogos disponíveis com botão “Jogar”.

[Dado que] seleciono um jogo
[Quando] confirmo a entrada no jogo
[Então] o jogo deve carregar em até **3s**, exibindo saldo disponível e limites.

[Dado que] o jogo não está disponível (manutenção/geofencing/idade)
[Quando] tento abrir
[Então] devo ver mensagem clara do motivo e, se aplicável, estimativa/CTA alternativo.

[Dado que] atingi meus limites de jogo responsável
[Quando] tento iniciar uma nova sessão
[Então] o acesso é bloqueado e vejo instruções para ajustar limites (se permitido).

---

[US006] — Definir limites de aposta (responsible gambling)
[Dado que] estou nas configurações de limites
[Quando] defino valores diário e semanal e salvo com 2FA
[Então] os limites entram em vigor imediatamente e ficam visíveis no cabeçalho do Cassino.

[Dado que] tento apostar acima do limite restante
[Quando] clico em “Apostar”
[Então] a aposta é bloqueada e vejo mensagem com o quanto ainda posso apostar e o horário de reset.

[Dado que] diminuo um limite existente
[Quando] salvo a alteração
[Então] a redução é aplicada na hora; se aumento um limite, é aplicada somente após o período de reflexão (ex.: 24h).

[Dado que] meus limites foram alcançados
[Quando] tento abrir qualquer jogo
[Então] recebo bloqueio com opção de “Sair”, “Ajustar limites” (se permitido) ou “Apoio/ajuda”.

---

[US007] — Histórico de apostas e resultados
[Dado que] estou na página “Histórico”
[Quando] filtro por jogo/período/resultado
[Então] a lista é atualizada com total apostado, total ganho e saldo do período.

[Dado que] abro um item do histórico
[Quando] visualizo os detalhes
[Então] vejo data/hora, jogo, valor, odds/RTP informativo, resultado e identificadores da sessão.

[Dado que] não há registros para o filtro
[Quando] aplico a busca
[Então] vejo estado vazio com CTA para “Ir para Cassino”.

[Dado que] exporto o histórico
[Quando] clico em “Exportar CSV”
[Então] o arquivo contém as colunas exibidas e respeita o fuso horário do usuário.

---

[US008] — Modo demonstração dos jogos
[Dado que] estou na listagem de jogos
[Quando] escolho “Jogar em modo demonstração”
[Então] o jogo abre com créditos virtuais, rótulo visível “DEMO” e sem afetar meu saldo real.

[Dado que] estou no modo demonstração
[Quando] tento sacar ou transferir ganhos
[Então] vejo mensagem informando que não é possível em DEMO.

[Dado que] desejo mudar para dinheiro real
[Quando] clico em “Jogar com saldo real”
[Então] vejo um modal de confirmação com termos/responsible gambling antes de entrar.

---

[US009] — Gerar NFTs de pagamento (experiência “rápida/divertida”)
[Dado que] estou na tela “NFT de Pagamento”
[Quando] informo valor, descrição, vencimento e escolho um tema/arte
[Então] ao confirmar com 2FA, o NFT é mintado e recebo link/QR para compartilhamento.

[Dado que] o NFT está mintado
[Quando] abro os detalhes
[Então] vejo o hash da transação, status on-chain, valor, emissor e validade.

[Dado que] o NFT expirou ou já foi usado
[Quando] alguém tenta resgatar
[Então] o resgate é bloqueado e é exibido o motivo.

[Dado que] falta saldo para taxa de rede (gas)
[Quando] tento mintar
[Então] vejo mensagem de erro e sugestão de adicionar saldo de gas.

---

[US010] — Criar (mintar) NFTs de pagamento (campos e validação)
[Dado que] estou no formulário de criação
[Quando] informo valor > 0, moeda, metadados mínimos e rede (ex.: testnet/mainnet)
[Então] o botão “Criar NFT” habilita; campos inválidos exibem erros específicos.

[Dado que] confirmo a criação
[Quando] assino a transação (custodial ou não)
[Então] o NFT entra em status `CREATED` → `MINTING` → `CONFIRMED/FAILED` com notificações do progresso.

[Dado que] a criação foi confirmada
[Quando] retorno ao meu inventário
[Então] o NFT aparece listável/transferível conforme regras definidas.

---

[US011] — Enviar NFT de pagamento por QR/Link
[Dado que] tenho um NFT de pagamento válido
[Quando] clico em “Compartilhar”
[Então] é gerado um link/QR com validade e número de usos (ex.: 1 uso).

[Dado que] o destinatário acessa o link
[Quando] confirma o recebimento
[Então] a transferência é executada on-chain e ambos recebem confirmação.

[Dado que] o link expirou ou foi usado
[Quando] alguém tenta utilizá-lo
[Então] a ação é bloqueada e o emitente pode reemitir novo link.

---

[US012] — Listar/comprar/vender NFTs (marketplace com royalties)
[Dado que] possuo um NFT negociável
[Quando] publico um anúncio com preço e royalties explícitos
[Então] o item fica visível no marketplace com taxas e royalties calculados.

[Dado que] compro um NFT listado
[Quando] confirmo a transação
[Então] a propriedade muda para minha carteira, e os royalties são distribuídos automaticamente.

[Dado que] cancelo um anúncio aberto
[Quando] confirmo a ação
[Então] o anúncio sai do marketplace e o NFT retorna ao meu inventário.

[Dado que] ocorre falha on-chain
[Quando] a compra/venda não confirma
[Então] o status fica `FAILED` e os valores são estornados segundo as regras da rede/integração.

---

[US013] — Ver preços atuais de cripto
[Dado que] estou na tela “Mercado”
[Quando] seleciono as principais moedas (ex.: BTC, ETH, USDT, etc.)
[Então] vejo preço atual, variação 24h, volume e gráfico _sparkline_.

[Dado que] escolho moeda fiduciária (BRL/USD)
[Quando] altero a preferência
[Então] todos os preços são convertidos para a moeda escolhida.

[Dado que] a atualização automática está ativa
[Quando] passam N segundos
[Então] as cotações são atualizadas sem recarregar a página; erros mostram estado de falha e opção “Tentar novamente”.

---

[US014] — Alertas de preço
[Dado que] estou em “Alertas”
[Quando] defino um gatilho (≥ ou ≤ valor) para um ativo e salvo
[Então] recebo notificação por push/e-mail quando o preço cruzar o limiar.

[Dado que] um alerta disparou
[Quando] entro na tela de Alertas
[Então] vejo o log do disparo, horário e preço; alertas repetidos só disparam novamente após eu reativá-los ou após _cooldown_.

[Dado que] edito um alerta
[Quando] aumento a sensibilidade para disparar mais cedo
[Então] as novas regras valem imediatamente; se eu o desativar, ele não dispara até ser reativado.

---

[US015] — Comprar e vender cripto
[Dado que] estou verificado (KYC aprovado)
[Quando] informo o ativo, valor e forma de pagamento/recebimento
[Então] vejo uma prévia com cotação, taxa, _slippage_ e total líquido antes de confirmar.

[Dado que] confirmo a ordem dentro do tempo de cotação
[Quando] assino/valido a operação
[Então] a ordem segue `CREATED` → `PROCESSING` → `COMPLETED/FAILED`, com atualização do saldo e recibo.

[Dado que] a cotação expirou
[Quando] tento confirmar
[Então] sou avisado para atualizar a cotação antes de prosseguir.

[Dado que] a operação viola limites/regra AML
[Quando] tento concluir
[Então] a ordem é bloqueada ou marcada para revisão, com mensagem clara do motivo.

---

[US016] — Portfólio consolidado
[Dado que] acesso “Meu Portfólio”
[Quando] a página carrega
[Então] vejo saldos por ativo, valor atual, custo médio, P/L e distribuição em gráfico.

[Dado que] filtro o período
[Quando] aplico o filtro
[Então] os cálculos de P/L e gráficos são recalculados para o intervalo.

[Dado que] desejo exportar
[Quando] clico em “Exportar CSV”
[Então] baixo um arquivo com posições e histórico do período.

[Dado que] não possuo ativos
[Quando] abro a tela
[Então] vejo estado vazio com CTA para “Depositar”, “Comprar cripto” ou “Explorar mercado”.

---

## Exemplo

[US000]
[Dado que] estou na página de um produto que tem estoque
[Quando] clico no botão “Adicionar ao carrinho”
[Então] o produto deve ser incluído na minha lista do carrinho, e o contador de itens do carrinho deve ser incrementado.
