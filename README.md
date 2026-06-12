# Mellyx

> Plataforma web de gestão de honeypots de baixa interação para detecção de **movimentação lateral intra-segmento** em redes corporativas segmentadas.

![status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![contexto](https://img.shields.io/badge/contexto-TCC%20ADS-blue)

---

## Sobre o projeto

**Mellyx** é uma plataforma centralizada, leve e *self-hosted* para **implantar, monitorar e alertar** sobre honeypots (iscas) distribuídos pelos segmentos de uma rede.

A premissa: a segmentação por firewall protege o tráfego **entre** VLANs (prevenção), mas é cega para o tráfego **dentro** de um mesmo segmento. Quando um atacante compromete um host e se move lateralmente dentro da VLAN, esse movimento não passa pelo firewall — e quase nada o detecta. O Mellyx ocupa esse ponto cego: iscas posicionadas nos segmentos certos disparam um alerta de alta confiança ao primeiro toque, já que nenhum acesso legítimo deveria ocorrer ali.

> O Mellyx **não** constrói o motor da isca (reutiliza projetos consolidados como Cowrie e OpenCanary); ele é a **camada de gestão**: orquestração, deploy, coleta, normalização, classificação e alerta.

---

## Contexto acadêmico

Este projeto é desenvolvido como **Trabalho de Conclusão de Curso (TCC)** do curso de **Análise e Desenvolvimento de Sistemas (ADS)** da **Universidade Tuiuti do Paraná (UTP)**.

O nome **Mellyx** deriva de *mel / mellis* (latim para "mel"), em referência direta ao conceito de *honeypot*.

---

## Objetivos

**Objetivo geral:** desenvolver e validar uma plataforma web que gerencie honeypots de baixa interação distribuídos por segmentos de rede, detectando interações não autorizadas que escapam ao controle da segmentação.

**Objetivos específicos:**
- Projetar uma arquitetura cliente-gestor (*phone-home*) compatível com redes segmentadas.
- Implementar o deploy containerizado das iscas com registro automático no gestor.
- Coletar e normalizar eventos de múltiplas iscas em um formato único.
- Classificar eventos entre **varredura** e **tentativa de exploração**.
- Disparar alertas integrados (ex.: webhook do Teams).
- Validar a solução em laboratório segmentado, medindo a eficácia da detecção.

---

## Arquitetura (visão geral)

- **Manager (gestor):** aplicação web central que recebe os eventos, normaliza, classifica, exibe o painel e dispara alertas. Roda em um segmento de gerência protegido.
- **Agent (isca):** container leve, posicionado por VLAN, que se passa por um host real e **inicia a conexão de saída** até o gestor (modelo *phone-home*).

O modelo *phone-home* respeita a segmentação: o gestor nunca alcança as VLANs — são as iscas que falam com ele, exigindo apenas uma regra de firewall de saída por segmento.

---

## Stack tecnológica (prevista)

- **Backend:** Python (FastAPI/Flask)
- **Frontend:** React
- **Iscas / deploy:** Docker · Cowrie / OpenCanary
- **Comunicação:** HTTPS/TLS (phone-home)

---

## Estrutura do repositório

```
mellyx/
├── docs/        # documentação, EAP, diagramas
├── manager/     # gestor web (backend + frontend)
├── agent/       # isca containerizada
└── docker-compose.yml
```

---

## Status

🚧 **Em desenvolvimento.** O projeto está em fase inicial (planejamento e fundamentação). Acompanhe o progresso pelas *Issues* e pelo *Project* do repositório.

---

## Aviso de uso

O Mellyx é uma ferramenta **defensiva**, voltada à **detecção** de atividade não autorizada. As iscas são de **baixa interação** e não expõem serviços reais. O uso em qualquer rede de produção exige autorização formal e prévia dos responsáveis. Este projeto destina-se a fins acadêmicos e de pesquisa defensiva.

---

## Licença

[MIT License](./LICENSE)
Componentes de terceiros (Cowrie, OpenCanary) e seus avisos de copyright estão em [THIRD_PARTY_LICENSES](./THIRD_PARTY_LICENSES.md).

---

## Autor

Desenvolvido por [@zLuuuck](https://github.com/zLuuuck) como parte do TCC de Análise e Desenvolvimento de Sistemas (UTP).
