# Esquema de Conexão - Módulo de Automação para Ventiladores

## Componentes Necessários
- NodeMCU (ESP8266)
- 4 Relés 5V (para controlar as bobinas do motor)
- 1 Relé 5V (para controlar a alimentação geral)
- Fonte 5V para alimentar NodeMCU e relés
- Transformador 220V → 24V (ou fonte 220V com regulador)
- 4 Botões (push button)
- Resistores pull-up 10kΩ (para botões)
- Capacitores 100nF (proteção nos relés)
- Diodos 1N4007 (proteção nos relés)

## Esquema de Ligação do NodeMCU com Relés

```
NODMCU (ESP8266)
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  GND ───────────────────────────────────────────────────────→ GND
│  3V3 ───────────────────────────────────────────────────────→ VCC
│                                                                 │
│  D1 (GPIO5)   ──→ Relé 1 (Bobina Velocidade 1)               │
│  D2 (GPIO4)   ──→ Relé 2 (Bobina Velocidade 2)               │
│  D3 (GPIO0)   ──→ Relé 3 (Bobina Velocidade 3)               │
│  D4 (GPIO2)   ──→ Relé 4 (Desliga Motor)                     │
│                                                                 │
│  D5 (GPIO14)  ←─ Botão Liga/Desliga (com pull-up 10kΩ)       │
│  D6 (GPIO12)  ←─ Botão Velocidade 1 (com pull-up 10kΩ)       │
│  D7 (GPIO13)  ←─ Botão Velocidade 2 (com pull-up 10kΩ)       │
│  D8 (GPIO15)  ←─ Botão Velocidade 3 (com pull-up 10kΩ)       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Esquema de Ligação Relés com Bobinas do Motor

```
ENTRADA 220V AC
      ↓
   ─┬─┬─
    │ │
    R S T (3 fases ou Fase/Neutro em monofásico)

RELÉ 1 - VELOCIDADE 1
   Comum (COM) ──→ Entrada de 220V (Fase)
   Normalmente Aberto (NO) ──→ Bobina de Velocidade 1
   
RELÉ 2 - VELOCIDADE 2
   Comum (COM) ──→ Entrada de 220V (Fase)
   Normalmente Aberto (NO) ──→ Bobina de Velocidade 2
   
RELÉ 3 - VELOCIDADE 3
   Comum (COM) ──→ Entrada de 220V (Fase)
   Normalmente Aberto (NO) ──→ Bobina de Velocidade 3
   
RELÉ 4 - DESLIGA (Segurança)
   Comum (COM) ──→ Saída das bobinas
   Normalmente Fechado (NC) ──→ Neutro de 220V

Motor do Ventilador
   Terminal 1 (Neutro) ──→ Ligado diretamente ao Neutro de 220V
   Terminal 2 ──→ Saída do Relé 4
```

## Diagrama Lógico de Controle (Interbloqueio)

```
Estado do Motor:
├─ DESLIGADO
│  └─ Relés 1, 2, 3, 4 = DESLIGADOS
│
├─ VELOCIDADE 1
│  ├─ Relé 1 = LIGADO
│  ├─ Relés 2, 3 = DESLIGADOS
│  └─ Relé 4 = LIGADO (permite passagem de corrente)
│
├─ VELOCIDADE 2
│  ├─ Relé 2 = LIGADO
│  ├─ Relés 1, 3 = DESLIGADOS
│  └─ Relé 4 = LIGADO
│
└─ VELOCIDADE 3
   ├─ Relé 3 = LIGADO
   ├─ Relés 1, 2 = DESLIGADOS
   └─ Relé 4 = LIGADO
```

## Matriz de Proteção (Interbloqueio via Software)

| Estado | Relé 1 | Relé 2 | Relé 3 | Relé 4 |
|--------|--------|--------|--------|--------|
| Desligado | OFF | OFF | OFF | OFF |
| Vel 1 | ON | OFF | OFF | ON |
| Vel 2 | OFF | ON | OFF | ON |
| Vel 3 | OFF | OFF | ON | ON |

**Regra Crítica**: Nunca ativar simultaneamente Relés 1, 2 ou 3. O código garante apenas um ligado por vez.

## Conexão de Proteção nos Relés

```
Para cada bobina de relé:
    (+)VCC ─┬─ Diodo 1N4007 (cátodo para bobina)
            │
          Bobina do Relé
            │
            └─ GND do NodeMCU (com diodo cátodo virado para bobina)
            
Resistor/Capacitor de proteção entre COM e NO de cada relé:
    COM ─┬─ 100nF capacitor ─┬─ NO
         │                   │
         └───────────────────┘
```
