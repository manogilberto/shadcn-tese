# Escopo do Projeto

## Visão Geral

Juntos, iremos desenvolver uma aplicação web completa voltada para gestão pós-venda de clientes de uma indústria fabricante de máquinas. O objetivo é fornecer uma solução robusta, escalável e modular, facilitando futuras expansões.

## Tecnologias

- **Frontend:** React ou Next.js, TanStack Query, Shadcn/UI, Tailwind CSS  
- **Backend:** Node.js com Express.js, PostgreSQL com Drizzle ORM
- **Integração de Email:** Nodemailer (IMAP/SMTP)

## Requisitos

- Node.js versão 18 ou superior
- PostgreSQL versão 13 ou superior
- Conexão com internet para funcionalidades específicas do cliente de email

## Estrutura Recomendada de Pastas

```
project/
├── client/        # Frontend (React)
├── server/        # Backend (Express.js)
├── shared/        # Código compartilhado
├── migrations/    # Scripts de migração do banco
└── uploads/       # Arquivos enviados por usuários
```
## FrontEnd
O frontend, possui um template pré-configurado na pasta `client/`. Para instalar as depencias, acessa a página e rode os seguintes comandos.
```pnpm install```
e:
```pnpm run dev```

## Banco de Dados

Utilizaremos PostgreSQL com scripts de migração gerenciados pelo Drizzle ORM. Cada nova tabela criada deverá ter scripts correspondentes em `migrations/` para facilitar futuras implantações sem necessidade de ajustes adicionais.

## Arquitetura Modular

A aplicação será organizada de maneira modular por domínios. Cada módulo terá sua própria estrutura MVC no frontend e backend. Recomendamos:

- Sistema de plugins para fácil adição e manutenção de módulos.
- Comunicação intermodular baseada em eventos.
- Serviço centralizado de envio e recebimento de emails.
- Estrutura de módulos claramente separada, permitindo escalabilidade e manutenção.

## Segurança e Usuários

- Sistema sem cadastro público. Apenas usuários autorizados poderão criar novos usuários.
- Perfis personalizados com permissões detalhadas para acesso às views e ao banco de dados.
- Informações adicionais ao cadastro de usuarios: Telefone fixo, ramal, telefone celular.


## Módulos Principais

### Clientes
- Cadastro básico e código ERP
- Cadastro de países com DDI e bandeira (via Excel)
- Segmentação por indústria
- Tags personalizáveis
- Múltiplos contatos por cliente (emails, telefones)

### Ativos/Produtos
- Número de série automático com prefixo por categoria
- Estrutura hierárquica (ativos podem conter subcomponentes)
- Famílias de ativos identificadas por código alfanumérico
- Rastreamento de garantia
- Documentação técnica com níveis de acesso e versionamento

### Chamados
- Tipos configuráveis (falha, dúvida, melhoria, peças)
- Origem diversa (telefone, email, formulário, operador)
- Prioridade ajustável (baixa a crítica)
- Status configuráveis no banco de dados
- Visualizações Kanban e lista agrupada
- Interações internas e externas com controle de privacidade

### Ordens de Serviço
- Diferentes tipos de manutenção
- Controle detalhado de materiais e peças
- Controle e rastreamento das horas técnicas
- Precificação flexível
- Fluxo de aprovação, especialmente para garantias
- Aprovação de orçamentos diretamente pelos clientes

### Agendamento
- Gerenciamento de visitas técnicas e manutenções
- Organização de disponibilidade técnica
- Controle de especialidades técnicas e zonas geográficas
- Cálculo automático de distâncias e valores por quilômetro
- Notificações automáticas por email e calendário

## Sistema Geral

- Suporte inicial ao português, com futura expansão para múltiplos idiomas
- Configuração de moedas com formatos customizados
- Escalabilidade planejada para gerenciar mais de 20 mil ativos em cinco anos


## Auditorias e Segurança

- Registro de auditorias detalhadas de login e modificações importantes
- Uso de soft deletes para proteção e recuperação de dados

## API REST - Exemplo de Estrutura

- `GET /api/clients` – Listar clientes
- `POST /api/clients` – Criar cliente
- `GET /api/clients/:id` – Detalhes do cliente
- `PUT /api/clients/:id` – Atualizar cliente
- `GET /api/clients/:id/contacts` – Contatos do cliente
- `GET /api/clients/:id/assets` – Ativos do cliente
- `GET /api/tickets` – Listar chamados
- `POST /api/tickets` – Criar chamado
- `GET /api/tickets/:id` – Detalhes do chamado
- `PUT /api/tickets/:id` – Atualizar chamado
- `POST /api/tickets/:id/comments` – Adicionar comentário
- `POST /api/tickets/:id/assign` – Atribuir chamado

## Observação Final

Este projeto deverá priorizar a criação de uma estrutura sólida e modular, simplificando a implementação futura de novos módulos e garantindo facilidade na manutenção e escalabilidade da aplicação.

# Perguntas e respostas

## 1. Módulo de Clientes

### 1. Cadastro de Clientes

**Além das informações básicas (nome, CNPJ/CPF, endereço), quais campos específicos são essenciais para seus clientes industriais?**

- Dados completos de endereço (rua, complemento, bairro, cidade, estado, país, CEP).
- Campo para código de cliente no ERP.
- Cadastro separado para países, com possibilidade de importação (planilha Excel com colunas: País, DDI, URL da imagem da bandeira).

**Você precisa rastrear filiais de um mesmo cliente?**

- Não há necessidade de rastrear filiais.

### 2. Segmentação de Clientes

**Quais critérios de segmentação são importantes para seu negócio?**

- Segmentação por tipo de indústria (cadastro separado).

**Você precisa de tags customizáveis para classificar clientes?**

- Sim, tags personalizáveis cadastráveis em tabela.

### 3. Contatos do Cliente

**Além de nome e dados básicos, quais informações específicas são importantes?**

- Nome, emails, ramal, telefones (múltiplos celulares e fixos), cargo e departamento.

**Você precisa diferenciar níveis de contatos?**

- Não é necessário.

## 2. Módulo de Ativos/Produtos

### 1. Cadastro de Ativos

**Informações específicas para rastreamento:**

- Número de série gerado automaticamente (ano, mês, hora do cadastro, categorização inicial por 2 letras).
- Código do produto no ERP, descrição original e descrição do ativo.
- Data de garantia e vinculação ao cliente.
- Hierarquia de ativos: ativos podem ter outros ativos como componentes.

**Identificação do ativo:**

- Número de série gerado automaticamente.
- Futuramente implementação de etiqueta com QR Code para chamado público.

### 2. Categorização de Ativos

- Famílias de ativos com nome e simbologia (2 letras).
- Hierarquia: máquinas e componentes (nem todos os componentes precisam ser gerenciados).

### 3. Ciclo de Vida

- Estados possíveis: em operação, em manutenção, obsoleto.
- Rastreamento de garantia por data de expiração.

### 4. Documentação Técnica

- Tipos: Manuais, esquemas elétricos, certificados, desenhos técnicos, software PLC.
- Documentos com controle de acesso e versionamento sugerido para documentos críticos (ex.: desenhos técnicos, software).

## 3. Classificação de Chamados

### Tipos de chamados atendidos:

- Falha técnica, dúvida técnica, pedido de melhoria, solicitação de peças de reposição.
- Origem: telefone, email, formulário público, operador.
- Prioridades: baixa, média, alta, crítica (definidas pelo operador).

### Fluxo de Trabalho:

- Estados configuráveis no banco de dados.
- Visualização inicial em Kanban e/ou lista agrupada.
- Possibilidade de pular etapas não aplicáveis a determinados tipos de chamados.

### Interações e Comentários:

- Todos os envolvidos podem interagir; interações internas privadas.
- Transferência de responsabilidade do chamado.
- Status alterado somente pelo responsável.
- Inclusão de novos membros somente pelo responsável.

### SLAs (Acordos de Nível de Serviço):

- Não há implementação atual de SLAs.

## 4. Módulo de Ordens de Serviço

### Tipos de Ordens de Serviço:

- Manutenção corretiva externa (garantia e fora de garantia).
- Manutenção preventiva externa.
- Manutenção corretiva interna (garantia e fora de garantia).
- Instalação de equipamentos.

- Idealmente vinculadas a chamados, com possibilidade de criação independente (ainda sob avaliação).

### Recursos e Materiais:

- Rastreamento detalhado de materiais e peças utilizadas, com controle de saída e retorno.
- Fluxo de aprovação para controle de estoque e faturamento.
- Registro detalhado de horas técnicas (variação por horário e técnico).

### Precificação e Faturamento:

- Flexível por hora técnica, pacote fechado, diária.
- Inclusão de despesas adicionais (viagem, hospedagem, alimentação).

### Aprovações:

- Fluxo de aprovação, especialmente em casos de garantia.
- Orçamentos precisam ser aprovados pelo cliente.

## 5. Módulo de Agendamento

### Tipos de Agendamentos:

- Visitas técnicas, manutenções preventivas e corretivas externas.
- Duração variável de acordo com o tipo de serviço.

### Recursos:

- Gestão de disponibilidade dos técnicos.
- Especializações consideradas: eletrotécnico, técnico geral, programador CLP.

### Zonas Geográficas:

- Organização por zonas específicas.
- Cálculo automático de distâncias e cobrança por km rodado.

### Notificações:

- Notificação por email a todos envolvidos.
- Calendário interno para técnicos.

## 6. Sistema de Campos Personalizáveis

- Será abordado futuramente.

## 7. Perguntas Gerais sobre o Sistema

### Idioma e Localização:

- Português inicialmente, futura expansão possível.
- Formatos personalizados para data, hora e moedas (GMT -3, cadastro específico para moedas).

### Integrações Externas:

- Integração futura com ERPs e sistemas contábeis.

### Políticas de Retenção de Dados:

- Avaliação futura.

### Requisitos de Segurança:

- Não há requisitos específicos atualmente.

### Volume de Dados:

- Estimativa de 20.000 ativos nos próximos 5 anos.

