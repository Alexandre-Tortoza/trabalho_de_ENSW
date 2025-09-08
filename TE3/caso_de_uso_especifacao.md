# Registrar Lançamento Financeiro

Descrição:
Permite ao usuário criar um lançamento financeiro (Despesa ou Receita), com suporte a pagamento/recebimento imediato ou futuro, recorrência mensal e parcelamento. Ao concluir, o sistema atualiza saldos e indicadores do dashboard do período selecionado.

Ator(es):
Usuário autenticado (ator primário); Serviço de Notificações (ator de apoio); Serviço de Agendamento/Recorrência (ator de apoio).

Pré-condições:

- Usuário autenticado e com pelo menos uma conta financeira ativa.
- Categorias financeiras disponíveis (cadastro mínimo).
- Período corrente definido no contexto do dashboard.

Pós-condições:

- Lançamento criado com status adequado (`PENDING`, `PAID` para despesa; `PENDING`, `RECEIVED` para receita).
- Saldo da conta impactado imediatamente quando `PAID`/`RECEIVED`; pendências impactam quando confirmadas.
- Dashboard do período recalculado (saldo, receita, despesa, resultado, % orçamento, gráficos).
- Quando aplicável, série recorrente registrada ou parcelas N/N criadas.

Regras de negócio:

- RN-01 (Valor): valor obrigatório e > 0.
- RN-02 (Categoria): categoria é obrigatória para despesas; origem/categoria obrigatória para receitas.
- RN-03 (Status & datas):

  - Despesa: `PENDING` (data futura ou sem “Pago agora”) → `PAID` (data de pagamento).
  - Receita: `PENDING` (data futura) → `RECEIVED` (data de recebimento).

- RN-04 (Recorrência): “recorrente mensal” cria instâncias futuras na mesma data até cancelamento/fim.
- RN-05 (Parcelamento): “parcelado em N vezes” cria N parcelas identificadas “i/N”.
- RN-06 (Validação): impedir submit sem valor/categoria; mensagens claras por campo.
- RN-07 (Auditoria/Timezone): registrar usuário, timestamps (UTC) e considerar fuso do usuário para exibição.

Protótipo(s) de tela:

- T001 – Nova Despesa (formulário com valor, data, categoria, conta, descrição, “Pago agora”, “Recorrente”, “Parcelado”).
- T002 – Nova Receita (valor, data, origem/categoria, conta, “Recebido agora”, “Recorrente”).
- T003 – Dashboard Financeiro (cards e gráficos recalculados após salvar).

---

## Fluxo básico

1. O usuário, a partir do dashboard, seleciona “Nova Despesa” ou “Nova Receita”.
2. O sistema exibe o formulário correspondente (T001/T002) com os campos obrigatórios e opções de “Pago/Recebido agora”, “Recorrente” e “Parcelado”.
3. O usuário preenche os dados e confirma o envio.
4. O sistema valida os campos (RN-01/02/06).
5. O sistema cria o lançamento com o status adequado (RN-03); se “Pago/Recebido agora”, grava a data de quitação/recebimento = hoje.
6. Se recorrente, o sistema agenda as próximas ocorrências (RN-04); se parcelado, cria N parcelas (RN-05).
7. O sistema atualiza saldos e recalcula o dashboard do período.
8. O sistema apresenta confirmação e (opcional) envia notificação ao usuário.

---

## Fluxos alternativos

A1 – Lançamento recorrente
A-1.1 Usuário marca “Recorrente mensal” e define fim (ou indefinido).
A-1.2 Sistema grava a série e cria/agenda ocorrências futuras na mesma data mensal (RN-04).

A2 – Lançamento parcelado
A-2.1 Usuário marca “Parcelado em N vezes” e informa N.
A-2.2 Sistema cria N parcelas identificadas “i/N”, distribuindo-as conforme a regra (ex.: meses subsequentes).

A3 – Receita com data futura
A-3.1 Usuário informa data futura e não marca “Recebido agora”.
A-3.2 Sistema cria o lançamento como `PENDING`; o saldo só será impactado quando a receita for marcada/atualizada para `RECEIVED`.

---

## Fluxos de exceção

E1 – Validação de formulário falha
E-1.1 Sistema detecta campos inválidos (valor ≤ 0, categoria ausente, data inválida).
E-1.2 Sistema bloqueia o submit e exibe mensagens de erro específicas por campo.

E2 – Falha no processamento/persistência
E-2.1 Ocorre indisponibilidade momentânea da API/BD.
E-2.2 Sistema exibe mensagem de erro e CTA “Tentar novamente” sem perder os dados preenchidos.

E3 – Conta financeira inválida/inativa
E-3.1 Usuário seleciona conta desativada ou sem permissão.
E-3.2 Sistema impede a criação e informa que é necessário escolher uma conta ativa.

E4 – Conflito de período/Timezone
E-4.1 A data do lançamento está fora do período ativo do dashboard.
E-4.2 Sistema cria o lançamento, mas alerta que os indicadores exibidos pertencem a outro período e oferece alterar o filtro.
