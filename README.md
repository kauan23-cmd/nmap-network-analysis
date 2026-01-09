# nmap-network-analysis
Network Reconnaissance & Vulnerability Analysis Study using Nmap.

## 1. Introdução
Este projeto documenta os estudos práticos de reconhecimento de rede utilizando a ferramenta **Nmap**. O objetivo foi identificar dispositivos ativos, portas abertas e serviços em execução em um ambiente controlado, comparando a visibilidade interna versus externa.

## 2. Metodologia
* **Ferramenta:** Nmap 7.98
* **Ambiente:** Rede Local (LAN) e IP Público (WAN)
* **Objetivo:** Enumeração de portas e identificação de serviços (Fingerprinting).

## 3. Desenvolvimento e Resultados

### Etapa 1: Reconhecimento Interno (LAN)
Foi executado um scan agressivo no gateway da rede local para identificar serviços ativos no roteador.

**Comando utilizado:**
```bash
nmap -p 1-65535 -T4 -A -v 192.168.x.x
```
### Resultados obtidos:

| Porta | Estado | Serviço | Versão / Detalhes |
| :--- | :--- | :--- | :--- |
| 80/tcp | Open | HTTP | Interface de Gerenciamento Web |
| 443/tcp | Open | HTTPS | Interface Segura (SSL/TLS) |
| 1900/tcp | Open | UPnP | TP-LINK router upnpd |

Análise Técnica: A presença da porta 1900 (UPnP) indica que o roteador permite a configuração automática de redirecionamento de portas por dispositivos internos. Em um cenário de segurança, o UPnP deve ser monitorado, pois pode ser explorado por malwares para abrir brechas no firewall.

### Etapa 2: Reconhecimento Externo (WAN)
Nesta fase, o alvo foi o IP Público da conexão, simulando a visão que um atacante teria através da internet para testar o perímetro de segurança.

**Comando utilizado:**
```bash
nmap -Pn -F [IP_PUBLICO_OCULTO]
```
### Resultado obtido: 

100 filtered tcp ports (no-response).

Análise: O estado Filtered indica que o firewall do roteador ou do provedor (ISP) está descartando pacotes (DROP), tornando a rede "invisível" para varreduras externas. Isso demonstra uma configuração de segurança eficaz (Modo Stealth).

## 4. Conclusões e Recomendações
Através deste estudo, foi possível observar:

### Diferença de Perímetro:
Serviços essenciais para administração (HTTP/HTTPS) estão acessíveis apenas internamente.
### Segurança por Omissão:
O bloqueio de respostas ICMP (Ping) e o estado filtrado das portas externas dificultam significativamente o reconhecimento por parte de agentes maliciosos.
### Ponto de Atenção:
Como recomendação de hardening, sugere-se desativar o protocolo UPnP caso não seja estritamente necessário.
