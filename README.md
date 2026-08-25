# ArchFlow

Sistema de gestão para escritórios de arquitetura. Centraliza clientes, projetos, prazos, briefing, especificação de obra, propostas e financeiro — substituindo a colcha de retalhos de planilhas, Trello e blocos de anotação que a maioria dos escritórios usa hoje.

**[Acessar o app →](https://archflow-wine.vercel.app/)**

---

## O problema

Um escritório de arquitetura pequeno opera espalhado entre WhatsApp, Google Drive, Excel, Trello, agenda e e-mail. As informações se perdem, os prazos escapam e a cobrança atrasa porque ninguém lembrou de emitir.

O ArchFlow não tenta substituir AutoCAD ou Revit — arquivos pesados continuam onde funcionam bem. Ele substitui a *gestão*: o que precisa ser feito, por quem, até quando, por quanto e o que já foi pago.

## Funcionalidades

**Projetos e execução**
- Projetos com etapas, prazos individuais por etapa e checklist de tarefas
- Timeline automática — cada ação relevante vira histórico, sem entrada manual
- Anexos por etapa e documentos por projeto
- Comentários por projeto

**Comercial**
- Tabela de honorários do escritório (por ambiente, por m² ou valor fixo)
- Calculadora de proposta com desconto e exportação em PDF
- Geração de parcelas a partir da proposta fechada

**Especificação de obra**
- Itens agrupados por ambiente definido pelo arquiteto
- Biblioteca de itens reutilizáveis entre projetos
- Exportação em PDF — versão completa ou lista de compras para o cliente

**Briefing**
- Questionário estruturado com modelos reutilizáveis
- Anexo do modelo próprio do escritório
- Anotações livres

**Financeiro**
- Contas a receber com lembrete automático de cobrança
- Despesas recorrentes do escritório com materialização mensal
- Visão consolidada por período e por projeto

**Escritório**
- Múltiplos espaços de trabalho por usuário
- Convites por link, com papel e projetos pré-atribuídos
- Permissões em duas camadas: papel no escritório e vínculo por projeto
- Agenda compartilhada e notificações in-app
- Tema claro/escuro

## Stack

- **Next.js 15** (App Router) · **React 19** · **TypeScript**
- **Tailwind CSS 4** · **shadcn/ui**
- **Drizzle ORM** · **Neon PostgreSQL**
- Autenticação própria com **argon2id** e sessões em banco
- **Cloudflare R2** para arquivos (upload direto via presigned URL)
- **React PDF** para documentos gerados
- **Resend** para e-mails transacionais
- Deploy na **Vercel**

## Decisões de arquitetura

**Permissões em duas camadas.** O papel no escritório (gerente ou membro) define o alcance; o vínculo por projeto define o escopo. Membro só enxerga projetos onde foi incluído, e dados financeiros do escritório nunca saem do servidor para quem não é gerente — a checagem é server-side em toda query, não apenas na interface.

**Snapshot em vez de referência.** Itens de biblioteca, perguntas de briefing e linhas de proposta são copiados no momento do uso. Reajustar a tabela de honorários não altera propostas já enviadas ao cliente.

**Histórico como subproduto.** A timeline não tem entrada manual: cada mutação relevante grava seu evento na mesma transação. O histórico é confiável porque não depende de alguém lembrar de registrar.

**Arquivos pesados ficam fora.** DWG, RVT e SKP continuam no Drive do escritório — o sistema guarda o link. Só documentos leves (contrato, recibo, proposta) sobem para o R2, direto do navegador via presigned URL.

## Status

Em uso e validação com escritórios reais. O roadmap é guiado por feedback de campo — prazos por etapa, calculadora de honorários e o redesenho de arquivos nasceram todos de sessões de teste com arquitetos.

---
