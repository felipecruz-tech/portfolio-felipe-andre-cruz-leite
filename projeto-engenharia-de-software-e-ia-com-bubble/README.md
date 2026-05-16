# 💰 Sistema de Orçamentos — Laboratório No-Code

## 📝 Descrição do Projeto
Este projeto consiste em um **Sistema de Orçamentos** inteligente e estruturado, focado no gerenciamento ágil de clientes, escopos comerciais e itens de serviço. Ele foi desenvolvido como parte prática da disciplina de **Desenvolvimento No-Code / Low-Code** com o objetivo de criar uma aplicação escalável, performática e que mitigue os riscos comuns de acoplamento de plataformas visuais.

A arquitetura do sistema foi projetada seguindo regras estritas de modelagem relacional de banco de dados, governança de dados (regras de privacidade por usuário) e blindagem contra *vendor lock-in* (dependência de fornecedor).

---

## 🚀 Tecnologias e Arquitetura
* **Plataforma Core:** [Bubble.io](https://bubble.io/).
* **Modelagem de Dados:** Banco relacional nativo com mapeamento de cardinalidade $N:1$.
* **Integrações/API:** Data API habilitada com exposição de endpoints REST.
* **Padrão de Segurança:** Condicionais dinâmicas baseadas no usuário atual (*Privacy Rules*).

---

## 📊 Estrutura do Banco de Dados (Data Types)
A modelagem foi concebida focando na alta performance do sistema. Seguindo a **regra de ouro**, as chaves estrangeiras (FK) ficam estritamente no lado filho da relação, evitando listas no elemento pai que degradariam o sistema acima de 100 itens.

### Tabelas Principais (Data Types)
* **Usuário:** Gerenciamento de perfil e credenciais com hash seguro e autenticação nativa.
* **Cliente:** Cadastro de empresas e contatos vinculados diretamente ao usuário criador.
* **Orçamento:** Registro principal contendo título, descrição, status e valor total calculado.
* **ItemOrçamento:** Detalhamento individual dos serviços/produtos com cálculo dinâmico de subtotal ($\text{quantidade} \times \text{preço unitário}$).

### Option Sets (Valores Controlados)
Para evitar o uso de texto fixo (*hardcoded*), o ciclo de vida dos orçamentos é controlado dinamicamente via Option Sets:
* **StatusOrçamento:** `Pendente` | `Aprovado` | `Rejeitado` | `Em revisão` | `Expirado`.
> ⚠️ **Nota de Segurança:** Conforme boa prática, nenhuma chave de API, token ou dado sensível é armazenado dentro de Option Sets.

---

## 🔒 Regras de Privacidade & Segurança
A segurança da aplicação foi configurada na camada de dados (*Data > Privacy*) para garantir o isolamento total de informações:
* **Regra Aplicada:** `This [Data Type]'s Creator is Current User` (Tanto para Orçamentos quanto para Clientes).
* **Acesso:** Apenas o criador do registro possui permissão para visualizar campos (`View all fields`) e encontrá-los em buscas (`Find in searches`).
* **Ação de Proteção:** A regra padrão `Publicly visible` gerada de forma automática foi totalmente deletada para blindar o banco de dados contra vazamentos.

---

## 📉 Estratégia de Saída (Vendor Lock-in Mitigation)
Como o Bubble retém a posse do código-fonte, este projeto foi desenhado desde o primeiro dia com uma **Estrategia de Saída** estruturada para garantir a autonomia do negócio:

1. **Exportação de Dados via Data API:** Mapeamento e liberação das tabelas essenciais para endpoints REST seguros (`GET /api/1.1/obj/...`). Extração em JSON paginado com parâmetros `cursor` e `limit`, permitindo posterior conversão para CSV ou banco PostgreSQL de forma íntegra.
2. **Reescrita em Stack Tradicional:** Utilização do diagrama de banco de dados desenvolvido e das anotações internas da plataforma (*Notes nos Workflows*) como especificação funcional clara. Isso elimina a necessidade de engenharia reversa no futuro, guiando uma reescrita limpa em **React** (Front-end) e **Node.js com Express** (Back-end).

---
[Voltar ao início](https://github.com/seu-usuario)
