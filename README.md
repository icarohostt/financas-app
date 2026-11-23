# Finanças da Casa

App de orçamento doméstico do casal. A ideia central: **antes de o ciclo começar, cada real da renda recebe um destino** (contas fixas, limite de cada categoria e metas). Durante o ciclo, cada gasto é lançado na hora e o app mostra quanto ainda resta em cada categoria.

- Funciona no navegador do celular e do computador, e pode ser instalado na tela inicial.
- Os dados ficam num repositório **privado** (`financas-dados`), no arquivo `financas.json`. Este repositório (`financas-app`) é público e contém **só o código**, sem nenhum valor real.
- Os dois aparelhos veem os mesmos dados: o app grava pela API do GitHub e confere atualizações a cada 20 segundos e sempre que é aberto.
- Sem internet, os lançamentos ficam guardados no aparelho e sobem quando a conexão volta.

## Como funciona o ciclo

O ciclo acompanha a fatura do cartão: **do dia 26 ao dia 25**. "Dezembro/2026" vai de 26/11 a 25/12, cuja fatura vence em 01/01.

- Compras parceladas: cada parcela conta no ciclo em que cai na fatura. O painel mostra quanto dos próximos ciclos já está comprometido.
- Rendas e contas fixas seguem o mês do calendário: em "Dezembro/2026" entram o salário de 15/12 e 31/12 e as contas de 10, 15 e 30/12. A fatura que vence em 01/01 é paga com o salário de 31/12.

## Telas

| Tela | Para quê |
|---|---|
| **Início** | Quanto ainda pode gastar (total e por dia), saldo de cada categoria, checklist das contas fixas, fatura projetada |
| **+** (botão verde no centro da barra) | Lançar um gasto: valor, categoria, forma de pagamento, parcelas. Avisa antes se o gasto vai estourar a categoria |
| **Extrato** | Lançamentos do ciclo, com filtro por categoria. Toque para editar ou excluir |
| **Planejar** | Orçamento do ciclo: rendas, rendas extras (13º, férias), contas fixas, limites e metas. O contador "Falta distribuir" precisa chegar a zero |
| **Metas** | Caixinhas (reserva, viagem, provisão anual), guardar/retirar e o **fechamento do ciclo** (registra os aportes e manda a sobra para uma caixinha) |
| **Ajustes** (engrenagem no topo) | Quem está usando o aparelho, valores padrão (categorias, contas fixas, rendas, caixinhas, formas de pagamento), exportação para Excel e conexão |

## Instalação (uma vez só)

### 1. Publicar o app (GitHub Pages)
No repositório `financas-app`: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `(root)` → Save**.
Em um ou dois minutos o app estará em `https://SEU-USUARIO.github.io/financas-app/`.

### 2. Criar um token de acesso para cada aparelho
No GitHub: **foto do perfil → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.

- **Token name:** ex.: `financas – celular Ícaro`
- **Expiration:** a validade que preferir (quando vencer, gere outro e cole em Ajustes)
- **Repository access:** *Only select repositories* → `financas-dados`
- **Permissions → Repository permissions → Contents:** *Read and write*

Gere um token por aparelho. Se um celular for perdido, basta revogar o token dele.

### 3. Conectar cada aparelho
Abra o endereço do app, preencha **usuário do GitHub**, repositório `financas-dados` e o **token**, e escolha quem está usando o aparelho.

Para instalar como aplicativo:
- **iPhone (Safari):** botão Compartilhar → *Adicionar à Tela de Início*
- **Android (Chrome):** menu ⋮ → *Instalar app* (ou *Adicionar à tela inicial*)

## Rotina sugerida

1. **Antes do dia 26:** abrir *Planejar* no ciclo novo, conferir rendas e contas fixas e ajustar os limites até "Falta distribuir" chegar a zero. Renda extra (13º, férias, honorários) entra em *+ Renda extra* e precisa ser distribuída também.
2. **Na hora do gasto:** botão **+**. Leva poucos segundos e mostra quanto sobra na categoria.
3. **Quando estourar:** *Remanejar entre categorias*, tirando de onde sobrou.
4. **Contas fixas:** marcar ✓ quando pagar.
5. **Dia 25 ou logo depois:** *Metas → Fechar ciclo*. Os aportes planejados são registrados nas caixinhas e a sobra (ou o estouro) vai para a caixinha escolhida.

## Detalhes técnicos

- Um único arquivo `index.html` (HTML, CSS e JavaScript puro, sem dependências), mais `sw.js` (funcionamento offline) e `manifest.webmanifest` (instalação).
- Valores guardados em **centavos** (inteiros), para evitar erro de arredondamento.
- Cada alteração é uma operação reaplicável. Se os dois gravarem ao mesmo tempo, o GitHub recusa a versão desatualizada, o app baixa a nova e reaplica as suas alterações, sem perder lançamentos.
- O token fica salvo apenas no navegador do aparelho e só é enviado para `api.github.com`.
- Há um **modo demonstração** (dados fictícios, só no aparelho) para testar sem conectar.
