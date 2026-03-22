# Projeto FitnessHub - Contexto e Arquitetura

Este documento consolida o estado atual do projeto FitnessHub, servindo como guia de referência para o desenvolvimento e tomada de decisão.

## 📌 Visão Geral
Plataforma SaaS para redes de academias exibirem métricas de treino em tempo real em telões interativos, com gamificação e gestão multi-unidade.

### Objetivos Principais
1. **Tempo Real:** Captura de métricas (velocidade, calorias, distância, tempo) de equipamentos IoT e wearables.
2. **Gamificação:** Rankings automáticos com regras de destaque configuráveis.
3. **Multi-Ambiente:** Filtragem de dados por salas específicas (Spinning, Funcional, etc.) com telões dedicados.
4. **Conectividade Híbrida:** Integração via IoT (API/Bluetooth) e Apps de Saúde (Apple Health, Google Fit).
5. **Multi-Tenant:** Estrutura hierárquica: Rede > Unidade > Sala.

## 🎨 Identidade Visual (UX/UI)
- **Cores:** Preto (#000000) e Laranja Vibrante (#FF6B35).
- **Estilo:** Motion design sutil, microinterações recompensatórias e modo escuro obrigatório.
- **Tipografia:** Moderna e legível à distância (otimizada para telões).

## 🛠️ Stack Tecnológica

### Backend (Kubernetes + Microserviços)
- **Infraestrutura:** Kubernetes (K8s).
- **Componentes:**
  - `app-autorizacao-sts`: Serviço de Token e Segurança (STS).
  - `app-fitnesshub-backend`: Core business e processamento de métricas.
  - `app-fitness-portal-academia`: Backend administrativo para unidades.
- **Gateway:** Kong API Gateway (gerenciamento de tráfego e políticas).
- **Mensageria:** Nats.io (comunicação assíncrona e eventos em tempo real).

### Mobile (Flutter)
- **Framework:** Flutter (compilação nativa para iOS e Android).
- **Integração de Saúde:** [Health Connector](https://github.com/fam-tung-lam/health_connector) para comunicação com Apple Health e Google Fit.

## 🏗️ Arquitetura (Miro)
O design do sistema está sendo documentado no Miro:
- **Board Principal:** [FitnessHub Miro Board](https://miro.com/app/board/uXjVGuXmKuM=/)
- **Diagrama de Classes Atualizado:** [Versão PT-BR (Aluno)](https://miro.com/app/board/uXjVGuXmKuM=/?moveToWidget=3458764664711412376)

### Entidades Principais
- **Rede:** Gestão central (CNPJ, Admin).
- **Unidade:** Academia física com fuso horário e configurações de painel.
- **Sala:** Espaço físico onde ocorrem os treinos e rankings.
- **Aluno:** Perfil do usuário com preferências de privacidade e histórico.
- **CheckIn:** Sessão ativa que vincula o Aluno a uma Sala.
- **ConjuntoMetricas:** Objeto que centraliza os dados de performance.

## 📱 App Móvel (Requisitos de Negócio)
- **Autenticação:** Manual + Google SSO.
- **Check-in:** Baseado em geolocalização (raio de 100m) + Bluetooth Beacon.
- **Privacidade:** Modo Fantasma (oculta do telão mas mantém sync) e conformidade LGPD.
- **Integrações:** Apple HealthKit e Google Fit/Health Connect.

---
*Documento atualizado em: 22 de Março de 2026*
