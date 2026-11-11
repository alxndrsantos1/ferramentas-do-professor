# Ferramentas para Professores (página central)

Página estática que reúne as ferramentas do Alexandre Santos e leva a cada uma. Publicada em `https://www.ferramentasdoprofessor.com.br`.

## Arquivos
- `index.html`: página principal com os cartões. Usa Tailwind pela CDN (`https://cdn.tailwindcss.com/3.4.17`, versão fixa) e Plus Jakarta Sans.
- `contato/index.html`: página `/contato/` com WhatsApp e e-mail. Os dados de contato ficam só nessa página.
- Não há build: o que está no repositório é o que vai ao ar.

## Publicação (Render)
- Serviço estático `ferramentas-do-professor` (publish path `./`), deploy automático da `main`.
- Domínio no Registro.br: registro A `216.24.57.1` e CNAME `www` para o endereço `onrender.com` do serviço.
- As ferramentas aparecem no mesmo domínio por regras de **Rewrite** do Render (configuradas no painel, não neste repositório): `/mapao`, `/fechamento` e `/bussola`, cada uma com a versão sem e com `/*`, apontando para os sites `onrender.com`.
- Nunca use **Redirect** para `/x` → `/x/`: o Render trata as duas formas como a mesma origem e entra em laço (`ERR_TOO_MANY_REDIRECTS`). Use só Rewrite.
- Por isso os sites rodando em subcaminho precisam de caminhos **relativos** aos arquivos.

## Cartões
- Notas Mapão: `/mapao/` (âmbar). Notas Edu Pro: `/fechamento/` (azul-céu, etiqueta AVA). Bússola de Cursos: `/bussola/` (roxo).
- Vagas CIEE (esmeralda) abre o endereço próprio `https://painel-vagas-ciee.onrender.com/` em outra aba, porque é um app com servidor (`/api/...`) que não funciona em subcaminho. Tem o aviso de que o primeiro acesso pode demorar cerca de 1 minuto (plano gratuito do Render).
- Ao criar um novo cartão, copie a estrutura de um existente e escolha uma cor pastel nova.

## Idioma e público
- Tudo em português do Brasil (interface, textos, commits e respostas).
- Público: professores da rede estadual de SP (usam no computador e no celular, em redes de escola). Mantenha a interface simples e legível.

## Identidade visual (igual em todas as Ferramentas do Professor)
- Paleta índigo: botão principal `#4f46e5` (hover `#4338ca`), fundos suaves `#eef2ff`, texto `#0f172a` e `#475569`, bordas `#e2e8f0`.
- Fonte Plus Jakarta Sans (Google Fonts), fundo em degradê `#eef2ff` → `#fff` → `#faf5ff` com manchas de cor desfocadas.
- Cartões com cantos bem arredondados (24 a 28 px), botões e abas em formato pill (totalmente arredondados), ícones SVG no estilo Lucide.
- Só tema claro (sem modo escuro).
- Barra de topo com o logo do site e o botão "Ferramentas do Professor" que volta para `https://www.ferramentasdoprofessor.com.br/`.
- Rodapé: "Desenvolvido por Prof. Alexandre Santos" + link "Contato" para `https://www.ferramentasdoprofessor.com.br/contato/`. Não escreva "Especialista" no nome.
- Cores semânticas das etiquetas: âmbar = notas e faltas, azul-céu = AVA/fechamento, roxo = orientação, esmeralda = estágio.

## Privacidade
- Planilhas e relatórios têm dados de alunos (menores). Nunca coloque dados reais de alunos no repositório, em exemplos, testes ou capturas de tela. Use dados inventados.
- Nunca grave telefone, e-mail ou chaves de API em arquivos do repositório (os repositórios, exceto o vagas-ciee, são públicos).

## Fluxo de trabalho com Git
- Faça as mudanças numa branch `claude/<assunto>` a partir da `main` atualizada (`git fetch origin main`).
- Mudanças de visual, lógica ou novas funções: envie a branch e peça aprovação ao Alexandre antes do merge na `main`.
- Ajustes de texto pedidos de forma explícita podem ir direto para a `main`.
- O merge é um avanço direto (fast-forward): `git push origin <branch>:main`. Só faça se `main` for ancestral da branch.
- A `main` publica sozinha no Render. Não crie pull request se não for pedido.
- Não faça commit de `package-lock.json`, `node_modules/` nem `dist/`.
- Mensagens de commit em português, no imperativo, com o corpo explicando o porquê.
- O aviso do hook sobre "commit sem branch remota" pode aparecer atrasado: confirme com `git ls-remote --heads origin <branch>` antes de reenviar.

## Como testar
- Use o Playwright (já instalado: `require('playwright')`) e confira sempre em 1280 px e em 390 px de largura. Verifique que não há rolagem horizontal (`scrollWidth <= innerWidth`) e que não há erros de página.
- A rede da sessão bloqueia vários endereços (CDNs, Google Fonts, GoatCounter, onrender.com). Isso não é defeito do código: as fontes caem para a fonte do sistema e o contador não carrega nos testes locais.
- Capturas de tela com `fullPage` mostram a barra fixa no meio da página: é efeito da captura, não bug.

## Teste local da página com Tailwind
A CDN não carrega na rede da sessão. Para pré-visualizar, compile o CSS com `tailwindcss@3.4.17` (CLI) e injete numa **cópia** do HTML. Nunca altere o arquivo real para isso.
