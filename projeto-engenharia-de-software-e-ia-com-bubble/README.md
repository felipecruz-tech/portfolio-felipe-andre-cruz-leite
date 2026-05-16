# 🌐 Sistema de Orçamentos — Ambiente de Homologação (Bubble.io)

## 📝 Descrição do Projeto
Este repositório documenta e estrutura o **Sistema de Orçamentos**, uma aplicação robusta desenvolvida na plataforma No-Code **Bubble.io**. O sistema foi projetado como parte prática do laboratório da disciplina de **Desenvolvimento No-Code / Low-Code**, funcionando como um ecossistema ágil para o gerenciamento de clientes, controle de escopos comerciais e geração dinâmica de propostas financeiras.

Este README reflete a arquitetura da aplicação implantada e acessível para validação no ambiente de testes:
👉 **URL do App (Para visualização do teste):** https://felipewilfred-57047.bubbleapps.io/version-test?debug_mode=true

---

## 🚀 Funcionalidades Chave
* **Autenticação Segura:** Módulo nativo de gerenciamento de sessões com criptografia e hash de senhas gerenciado internamente pela plataforma.
* **Painel de Clientes:** Cadastro e listagem estruturada de empresas, contatos, e-mails e telefones com DDD.
* **Emissor de Orçamentos:** Criação de escopos comerciais com definição de títulos, detalhamento descritivo e datas de validade.
* **Cálculo Dinâmico de Itens:** Módulo filho conectado ao orçamento para inserção de múltiplos itens com multiplicação automática de subtotal ($\text{quantidade} \times \text{preço unitário}$).

---

## 📊 Arquitetura do Banco de Dados (Data Types)
A modelagem relacional segue rigorosamente os padrões de engenharia de software para garantir a máxima performance e evitar a degradação do sistema.

### Tabelas Principais
1. **Usuário (`User`):** Armazena `id`, `nome`, `email`, `senha` e a data de criação.
2. **Cliente (`Cliente`):** Vinculado ao usuário que o cadastrou (`criador`), contendo dados cadastrais corporativos.
3. **Orçamento (`Orcamento`):** Armazena o cabeçalho da proposta, o `valor_total` calculado e o vínculo com a tabela Cliente.
4. **ItemOrçamento (`ItemOrcamento`):** Armazena as linhas de serviço com chaves estrangeiras apontando para o orçamento pai.

### Regra de Ouro da Modelagem
* Conforme o rascunho estrutural do banco de dados, **as chaves estrangeiras (FK) residem estritamente no lado filho da relação** (ex: `ItemOrcamento` aponta para `Orcamento`). 
* Listas de registros dentro da tabela pai foram completamente evitadas para mitigar gargalos de performance caso o volume ultrapasse 100 itens.

### Option Sets (Controle de Estados)
Para evitar o uso de textos fixos (*hardcoded*) e garantir a consistência das regras de negócio, o ciclo de vida das propostas é regido pelo Option Set **`StatusOrcamento`**:
* `Pendente` | `Aprovado` | `Rejeitado` | `Em revisão` | `Expirado`

---

## 🔒 Governança e Regras de Privacidade (`Data > Privacy`)
Para assegurar a conformidade e a segurança multi-inquilino (*multi-tenant*), a aplicação foi blindada diretamente na camada de dados:
* **Isolamento de Dados:** Configuração de regras estritas onde `This [Data Type]'s Creator is Current User`. Usuários autenticados possuem acesso completo de leitura e busca (`View all fields + Find in searches`) apenas aos registros criados por eles mesmos.
* **Segurança Padrão:** A política permissiva `Publicly visible` gerada automaticamente pela IA do Bubble foi permanentemente removida antes da publicação do ambiente.

---

## 📉 Mitigação de Dependência de Fornecedor (*Vendor Lock-in*)
Visando a sustentabilidade do negócio e antecipando cenários de migração para infraestruturas tradicionais, o projeto conta com uma política ativa de **Estratégia de Saída**:

1. **Exposição via Data API:** A Data API REST do Bubble está habilitada nas configurações internas (`Settings > API`), expondo os endpoints para consumo externo em formato JSON estruturado.
2. **Paginação Segura:** A extração pode ser feita via requisições HTTP autenticadas utilizando paginação com os parâmetros `cursor` e `limit`, permitindo a replicação integral dos dados em bancos externos como o **PostgreSQL**.
3. **Especificação Funcional para Reescrita:** Toda a lógica contida nos Workflows (regras de privacidade e enums) e as notas internas servem como documentação técnica para guiar uma eventual reconstrução completa utilizando pilhas convencionais como **React** e **Node.js (Express)**.

---
[Voltar ao início](https://github.com/seu-usuario)
