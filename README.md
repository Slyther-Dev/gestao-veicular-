# 🚗 AutoGestor DMS — Sistema de Gestão Integrada para Revenda de Veículos

![Status](https://img.shields.io/badge/Status-Em_Operação-22c55e?style=for-the-badge)
![Versão](https://img.shields.io/badge/Versão-1.0.0-3b82f6?style=for-the-badge)
![Arquitetura](https://img.shields.io/badge/Arquitetura-Single_File_SPA-f59e0b?style=for-the-badge)
![Segurança](https://img.shields.io/badge/Segurança-RBAC_%2B_2FA_%2B_Audit_Log-ef4444?style=for-the-badge)

Sistema completo de gestão empresarial (**DMS — Dealer Management System**) desenvolvido especificamente para lojas e revendas de veículos multimarcas. O projeto une controle operacional de pátio, inteligência de precificação baseada na **Tabela FIPE**, gestão financeira, controle de acesso granular por níveis hierárquicos (**RBAC**) e monitoramento de segurança de rede.

---

## 📋 Índice

- [Visão Geral e Funcionalidades](#-visão-geral-e-funcionalidades)
- [Hierarquia de Acessos e Segurança (RBAC)](#-hierarquia-de-acessos-e-segurança-rbac)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Como Executar Localmente](#-como-executar-localmente)
- [Políticas de Segurança de Rede Recomendadas](#-políticas-de-segurança-de-rede-recomendadas)
- [Roadmap de Evolução (Multi-PC & Nuvem)](#-roadmap-de-evolução-multi-pc--nuvem)

---

## 🚀 Visão Geral e Funcionalidades

### 1. Painel Executivo & Simulador de Mercado (Tabela FIPE)
- **KPIs em Tempo Real:** Valor total imobilizado em pátio, lucro líquido projetado, ticket médio de venda e giro médio de estoque (dias parado).
- **Motor de Recálculo FIPE:** Simulador dinâmico que aplica variações percentuais de mercado sobre todo o inventário, recalculando automaticamente margens de lucro e exibindo alertas visuais para veículos precificados **acima** ou **abaixo** da tabela de referência.
- **Gráficos Analíticos:** Visualização interativa de composição de estoque, comparativo Custo vs. Valor de Venda e evolução de faturamento (via *Chart.js*).

### 2. Gestão Completa de Estoque (Inventário Automotivo)
- Cadastro detalhado com **Marca, Modelo, Ano/Modelo, Placa, Chassi, RENAVAM, Cor e Quilometragem**.
- Controle individual de custos de preparação (oficina, funilaria, higienização, documentação) somados ao valor de aquisição para cálculo exato do **Custo Real**.
- Monitoramento de **Dias em Pátio** com alertas para veículos de baixo giro.

### 3. Frente de Vendas & Fluxo de Caixa Financeiro
- Registro de vendas vinculadas ao vendedor responsável, cliente comprador e forma de pagamento (À vista, Financiamento Bancário, Troca com troco).
- Cálculo automático de **comissão de vendedores** e lucro líquido real da operação.
- Baixa automática do veículo no pátio (alterando o status para *Vendido*) e lançamento imediato no extrato financeiro da loja.

### 4. Área de Membros e Gestão de Equipe
- Cadastro e gerenciamento de usuários internos com perfis individuais.
- Capacidade do **Super Administrador** promover outros membros a administradores secundários, redefinir permissões ou revogar acessos instantaneamente.

### 5. Central de Segurança de Rede & Auditoria
- **Simulador / Validador 2FA (TOTP):** Camada extra de autenticação para operações críticas e acesso administrativo.
- **Restrição de Perímetro (IP e Horário):** Controle de acesso configurado para permitir operações da equipe comercial apenas dentro da rede corporativa da loja e durante o horário de expediente.
- **Log de Auditoria Imutável:** Registro cronológico de todas as ações sensíveis (criação/edição de veículos, alteração de preços, registro de vendas e troca de permissões) contendo **Usuário, Cargo, Data/Hora e Endereço IP**.

---

## 🔐 Hierarquia de Acessos e Segurança (RBAC)

O sistema opera sob o modelo **RBAC (Role-Based Access Control)** para proteger dados financeiros estratégicos da empresa:

| Permissão / Módulo | 👑 Super Admin (Dono) | 🛡️ Admin (Gerente) | 💼 Membro (Vendedor) |
| :--- | :---: | :---: | :---: |
| Visualizar Estoque, Fotos e Preço de Venda | ✅ | ✅ | ✅ |
| Consultar Valor de Referência (Tabela FIPE) | ✅ | ✅ | ✅ |
| Registrar Venda / Atendimento | ✅ | ✅ | ✅ |
| Visualizar **Preço de Compra (Custo)** e **Lucro Real** | ✅ | ✅ | ❌ *(Oculto)* |
| Cadastrar / Editar Veículos e Custos de Oficina | ✅ | ✅ | ❌ |
| Acessar Fluxo de Caixa Completo e Extratos | ✅ | ⚠️ *(Parcial)* | ❌ |
| Criar/Promover Novos Administradores | ✅ | ❌ | ❌ |
| Acessar Logs de Auditoria e Configurações de Rede | ✅ | ⚠️ *(Leitura)* | ❌ |

---

## 📁 Estrutura do Projeto

```text
sistema-revenda-carros/
│
├── index.html      # Aplicação completa (Interface HTML5, Estilização Tailwind/CSS e Lógica JS)
└── README.md       # Documentação técnica e operacional do sistema
```

---

## ▶️ Como Executar Localmente

O projeto é uma *Single File SPA*, ou seja, não exige instalação de dependências nem build.

1. Baixe ou clone a pasta `sistema-revenda-carros/`.
2. Abra o arquivo `index.html` diretamente no navegador (Chrome, Edge ou Firefox).
3. Opcional: sirva a pasta com um servidor local simples, por exemplo `python -m http.server 8000`, e acesse `http://localhost:8000`.

> **Obs.:** bibliotecas como *Chart.js* e *Tailwind* podem ser carregadas via CDN, portanto é necessário acesso à internet na primeira execução, a menos que sejam incluídas localmente.

---

## 🛡️ Políticas de Segurança de Rede Recomendadas

- Restringir o acesso ao sistema à rede corporativa da loja (IP fixo / VPN).
- Limitar o uso da equipe comercial ao horário de expediente.
- Exigir 2FA (TOTP) para contas Super Admin e Admin.
- Revisar periodicamente o Log de Auditoria e revogar acessos de ex-colaboradores imediatamente.
- Manter backups regulares dos dados e do arquivo `index.html`.

---

## 🗺️ Roadmap de Evolução (Multi-PC & Nuvem)

- [ ] Backend com banco de dados centralizado para uso simultâneo em múltiplos computadores.
- [ ] Autenticação real no servidor (hash de senhas, sessões e 2FA verificado no backend).
- [ ] Integração automática com a API da Tabela FIPE.
- [ ] Hospedagem em nuvem com backup automático.
- [ ] Exportação de relatórios em PDF e Excel.
- [ ] Upload e armazenamento de fotos dos veículos.
