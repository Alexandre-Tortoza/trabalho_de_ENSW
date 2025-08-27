# Critérios de aceite

[US001]
[Dado que] estou na página principal do aplicativo, autenticado e com o mês atual
[Quando] - 
[Então] devem aparecer Saldo Atual, Despesa do Mês, Receita do Mês, Resultado do Mês, % do orçamento e gráficos de linha, pizza por categoria, barras por mês do período

[Dado que] altero o filtro de período (mês/ano ou intervalo customizado)
[Quando] aplico o filtro
[Então] todos os dados e gráficos são recalculados para o novo período

[Dado que] não existem lançamentos no período selecionado
[Quando] carrego a página
[Então] devo ver estados vazios com CTA para “Adicionar receita” 

[Dado que] a API está respondendo
[Quando] os dados ainda estão sendo buscados
[Então] devo ver skeletons/loader; se a busca falhar, devo ver mensagem de erro e botão “Tentar novamente”

---

[US002]
[Dado que] estou no formulário de “Nova Despesa”
[Quando] informo valor > 0, data, categoria (obrigatória), conta (opcional), descrição (opcional) e salvo
[Então] o lançamento é criado como 'PENDING' e aparece no extrato e no dashboard do período

[Dado que] marco “Pago agora”
[Quando] salvo a despesa
[Então] o status fica 'PAYED' com data de pagamento = hoje e o saldo da conta selecionada é atualizado imediatamente

[Dado que] escolho “Recorrente mensal”
[Quando] salvo a despesa
[Então] é criada uma série mensal com próxima ocorrência no mesmo dia dos meses seguintes até o fim definido (ou indefinido)

[Dado que] escolho “Parcelado em N vezes”
[Quando] salvo a despesa
[Então] são criadas N parcelas identificadas por “1/N … N/N”

[Dado que] tento salvar sem valor ou com valor ≤ 0 ou sem categoria
[Quando] envio o formulário
[Então] o submit é bloqueado e são exibidas mensagens de validação nos campos


---

[US003]
[Dado que] estou no formulário de “Nova Receita”
[Quando] informo valor > 0, data, origem/categoria, conta e salvo
[Então] o lançamento é criado como 'INCOMING' se a data ≤ hoje, ou 'PENDING' se futuro, e o saldo é atualizado quando 'RECEIPT'

[Dado que] marco “Recorrente mensal” (ex.: salário)
[Quando] salvo
[Então] é criada uma série mensal no mesmo dia até cancelamento

---

[US004]
[Dado que] seleciono “Transferir entre
[Quando] informo conta de origem ≠ conta de destino, valor > 0 e data
[Então] são criados dois lançamentos, saída na origem e entrada no destino

[Dado que] seleciono “Transferência externa (Pix/TED)”
[Quando] informo destinatário (salvo ou novo), dados bancários válidos (Pix/Agência-Conta, CPF/CNPJ), valor e confirmo 2FA
[Então] a ordem é criada com status CREATED → PROCESSING → COMPLETED or FAILED, e recebo notificação do resultado

[Dado que] meus limites diário/mensal foram excedidos
[Quando] tento confirmar a transferência
[Então] a operação é bloqueada, a menos que libere, com mensagem do motivo e a data/valor de liberação

[Dado que] informo dados bancários inválidos
[Quando] tento avançar
[Então] vejo mensagens de validação específicas (formato de chave Pix, agência/conta, CPF/CNPJ)

---

## Exemplo

[US000]
[Dado que] estou na página de um produto que tem estoque
[Quando] clico no botão “Adicionar ao carrinho”
[Então] o produto deve ser incluído na minha lista do carrinho, e o contador de itens do carrinho deve ser incrementado.
