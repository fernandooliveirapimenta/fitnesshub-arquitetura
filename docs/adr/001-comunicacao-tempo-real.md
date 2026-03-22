# ADR 001: Escolha da Tecnologia para Comunicação em Tempo Real (Telões)

## Status
Proposto

## Data
22 de Março de 2026

## Contexto
O FitnessHub precisa exibir rankings e métricas de alunos em telões de academias em tempo real. O sistema deve suportar altíssima escala (múltiplas redes de academias, milhares de salas simultâneas) e baixa latência. O WebSocket tradicional, embora funcional, pode apresentar gargalos de gerenciamento de estado e sobrecarga TCP (Head-of-line blocking) em cenários de instabilidade de rede ou escala massiva.

## Opções Consideradas

### 1. WebTransport (HTTP/3 + QUIC/UDP)
*   **Prós:** Sucessor do WebSocket. Elimina o "head-of-line blocking". Suporta datagramas (fire-and-forget), ideal para telemetria onde a perda de um pacote individual é aceitável em favor da instantaneidade.
*   **Contras:** Tecnologia muito recente, suporte em navegadores e bibliotecas de backend ainda em maturação comparado ao gRPC ou SSE.

### 2. Server-Sent Events (SSE) sobre HTTP/2
*   **Prós:** Unidirecional (Servidor -> Cliente), o que casa perfeitamente com o caso de uso dos telões. Mais simples de escalar com Load Balancers, reconexão nativa e utiliza multiplexação do HTTP/2.
*   **Contras:** Não suporta comunicação binária nativa (limitado a texto/UTF-8), exigindo Base64 ou formatos similares para dados complexos, o que aumenta o payload.

### 3. gRPC-Web (com Protocol Buffers)
*   **Prós:** Utiliza Protobuf (binário), sendo extremamente leve e rápido. Contratos tipados e suporte a streaming sobre HTTP/2. Excelente para ambientes de microserviços.
*   **Contras:** Requer um Proxy (como o Kong ou Envoy) para traduzir gRPC-Web para gRPC no backend.

### 4. WebSockets (Bidirecional Tradicional)
*   **Prós:** Padrão de mercado com suporte universal. Baixíssima latência pós-handshake e suporte a dados binários e texto.
*   **Contras (Limitações de Escala):**
    *   **Stateful Scaling:** Exige que o servidor mantenha uma conexão TCP aberta por cliente, consumindo memória RAM significativa.
    *   **Head-of-line Blocking:** Como roda sobre TCP, um único pacote perdido atrasa toda a fila de mensagens daquela conexão.
    *   **Gerenciamento de Conexões:** Exige "sticky sessions" e complexidade em Load Balancers para lidar com conexões de longa duração.
    *   **Zombie Connections:** Necessidade de implementar Heartbeats (Ping/Pong) manuais para detectar quedas silenciosas.

## Decisão Proposta: **WebTransport**
Para o núcleo de telemetria e rankings (Telões), a proposta é seguir com **WebTransport**.

**Justificativa:** 
O diferencial competitivo do FitnessHub é a percepção de "tempo real absoluto". O WebTransport, ao rodar sobre UDP/QUIC, garante que uma oscilação na rede da academia não "trave" o feed de dados de todos os alunos. A capacidade de enviar datagramas não confiáveis para métricas de milissegundos é o estado da arte para dashboards de alta performance.

## Consequências
*   **Positivas:** Latência mínima, melhor resiliência a redes instáveis e prontidão para escala global.
*   **Negativas:** Curva de aprendizado maior para a equipe e necessidade de validar suporte rigoroso no Ingress Controller (Kong) para HTTP/3.

## Referências
*   [WebTransport - MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebTransport_API)
*   [QUIC Protocol Benefits](https://www.chromium.org/quic/)
