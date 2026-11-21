#  PsiAgenda — Banco de Dados Relacional

Modelagem e implementação do banco de dados relacional em **MySQL** para o sistema **PsiAgenda**, uma solução voltada à otimização de rotinas clínicas, gestão de pacientes, controle financeiro e organização inteligente de agenda para profissionais da Psicologia.

---

##  Objetivo do Projeto

Eliminar o uso de planilhas e anotações manuais no consultório, centralizando em uma estrutura relacional íntegra e segura.
* Cadastro de pacientes e responsáveis legais (obrigatório para menores de 18 anos).
* Agendamento inteligente com bloqueio de choque de horários.
* Reserva dedicada para atividades administrativas e elaboração de documentos clínicos (laudos, relatórios e encaminhamentos).
* Controle financeiro por sessão avulsa ou pacote mensal, com previsão de recebimentos e fluxo de caixa.
* Controle de acesso baseado em perfis de usuário e sigilo de dados clínicos

---

## Categorias de Atendimento & Perfis de Acesso

##  Modalidades Parametrizadas
1. Avaliação/anamense
2. Atendimento Infantil.
3. Devolutiva de Atendimento Infantil.
4.  Atendimento Adulto.
5.  Horário Blocado (reserva administrativa para relatórios, laudos e encaminhamentos).

## Níveis de Permissão
* **Administrador / Psicólogo:** acesso irrestrito aos cadastros, prontuários, relatórios clínicos, financeiro global e parametrização de horários.
* **Atendente / Secretária:** acesso operacional ao cadastro de pacientes/responsáveis e agendamentos, sem visualização de prontuários ou dados financeiros globais.
* **Financeiro:** acesso ao módulo de caixa, histórico de atendimentos realizados e emissão de demonstrativos.

---

##  Arquitetura e Modelagem de Dados

O banco foi projetado em **MySQL** com foco em integridade referencial (`PK` e `FK`) e suporte à futura integração com aplicação orientada a objetos em **Java**.

```mermaid
erDiagram
    PACIENTE ||--o{ AGENDAMENTO : realiza
    TIPO_ATENDIMENTO ||--o{ AGENDAMENTO : classifica
    AGENDAMENTO ||--o| LANCAMENTO_FINANCEIRO : gera
    USUARIO ||--o{ AGENDAMENTO : gerencia

    PACIENTE {
        int id_paciente PK
        varchar nome
        date data_nascimento
        varchar telefone
        varchar email
        varchar nome_responsavel
        varchar contato_responsavel
    }

    TIPO_ATENDIMENTO {
        int id_tipo PK
        varchar descricao
        int duracao_minutos
        boolean bloqueia_agenda
    }

    AGENDAMENTO {
        int id_agendamento PK
        int id_paciente FK
        int id_tipo FK
        int id_usuario FK
        datetime data_hora_inicio
        datetime data_hora_fim
        varchar status
    }

    LANCAMENTO_FINANCEIRO {
        int id_lancamento PK
        int id_agendamento FK
        decimal valor
        varchar modalidade_cobranca
        varchar status_pagamento
        date data_vencimento
    }

    USUARIO {
        int id_usuario PK
        varchar nome
        varchar login
        varchar senha_hash
        varchar perfil_acesso
    }
