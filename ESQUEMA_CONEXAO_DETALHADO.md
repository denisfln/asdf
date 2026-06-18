# Esquema Detalhado de Ligação - Módulo de Automação para Ventiladores com 3 Bobinas

## Componentes Necessários
- NodeMCU (ESP8266)
- 4 Relés 5V com 2 contatos (DPDT ou SPDT com bobina) - 5 terminais cada
- Fonte 5V para alimentar NodeMCU e bobinas dos relés
- Fonte 220V com transformador ou regulador para alimentar bobinas do motor
- 4 Botões (push button)
- 4 Resistores pull-up 10kΩ (para os botões)
- 8 Diodos 1N4007 (proteção nas bobinas dos relés)
- 4 Capacitores 100nF (proteção nos contatos)

---

## 1. RELÉ 1 - CONTROLA BOBINA DE VELOCIDADE 1

### Terminais do Relé 1:
```
┌─────────────────────────────┐
│     RELÉ 1 (DPDT 5V)        │
├─────────────────────────────┤
│                             │
│  BOBINA+ ───────→ VCC 5V    │
│  BOBINA- ───────→ GPIO5     │
│  (COM 1) ───────→ FASE 220V │
│  (NO 1)  ───────→ BOBINA 1  │
│  (NC 1)  ───────→ TERRA     │
│                             │
└─────────────────────────────┘
```

### Conexão Detalhada Relé 1:
```
NodeMCU D1 (GPIO5) 
    ↓
[Resistor 1kΩ]
    ↓
Relé 1 - BOBINA-
    ↓
GND NodeMCU

NodeMCU VCC 5V
    ↓
[Diodo 1N4007 - cátodo para bobina]
    ↓
Relé 1 - BOBINA+
    ↓
GND

ENTRADA 220V - FASE
    ↓
Relé 1 - COM (Comum)

Relé 1 - NO (Normalmente Aberto)
    ↓
BOBINA 1 DO MOTOR
    ↓
TERRA (Neutro)

Relé 1 - NC (Normalmente Fechado)
    ↓
[opcional - não usar neste caso]
```

---

## 2. RELÉ 2 - CONTROLA BOBINA DE VELOCIDADE 2

### Terminais do Relé 2:
```
┌─────────────────────────────┐
│     RELÉ 2 (DPDT 5V)        │
├─────────────────────────────┤
│                             │
│  BOBINA+ ───────→ VCC 5V    │
│  BOBINA- ───────→ GPIO4     │
│  (COM 2) ───────→ FASE 220V │
│  (NO 2)  ───────→ BOBINA 2  │
│  (NC 2)  ───────→ TERRA     │
│                             │
└─────────────────────────────┘
```

### Conexão Detalhada Relé 2:
```
NodeMCU D2 (GPIO4)
    ↓
[Resistor 1kΩ]
    ↓
Relé 2 - BOBINA-
    ↓
GND NodeMCU

NodeMCU VCC 5V
    ↓
[Diodo 1N4007 - cátodo para bobina]
    ↓
Relé 2 - BOBINA+
    ↓
GND

ENTRADA 220V - FASE
    ↓
Relé 2 - COM (Comum)

Relé 2 - NO (Normalmente Aberto)
    ↓
BOBINA 2 DO MOTOR
    ↓
TERRA (Neutro)

Relé 2 - NC (Normalmente Fechado)
    ↓
[opcional - não usar neste caso]
```

---

## 3. RELÉ 3 - CONTROLA BOBINA DE VELOCIDADE 3

### Terminais do Relé 3:
```
┌─────────────────────────────┐
│     RELÉ 3 (DPDT 5V)        │
├─────────────────────────────┤
│                             │
│  BOBINA+ ───────→ VCC 5V    │
│  BOBINA- ───────→ GPIO0     │
│  (COM 3) ───────→ FASE 220V │
│  (NO 3)  ───────→ BOBINA 3  │
│  (NC 3)  ───────→ TERRA     │
│                             │
└─────────────────────────────┘
```

### Conexão Detalhada Relé 3:
```
NodeMCU D3 (GPIO0)
    ↓
[Resistor 1kΩ]
    ↓
Relé 3 - BOBINA-
    ↓
GND NodeMCU

NodeMCU VCC 5V
    ↓
[Diodo 1N4007 - cátodo para bobina]
    ↓
Relé 3 - BOBINA+
    ↓
GND

ENTRADA 220V - FASE
    ↓
Relé 3 - COM (Comum)

Relé 3 - NO (Normalmente Aberto)
    ↓
BOBINA 3 DO MOTOR
    ↓
TERRA (Neutro)

Relé 3 - NC (Normalmente Fechado)
    ↓
[opcional - não usar neste caso]
```

---

## 4. RELÉ 4 - SEGURANÇA (Desliga o motor)

### Terminais do Relé 4:
```
┌─────────────────────────────┐
│     RELÉ 4 (DPDT 5V)        │
├─────────────────────────────┤
│                             │
│  BOBINA+ ───────→ VCC 5V    │
│  BOBINA- ───────→ GPIO2     │
│  (COM)   ───────→ Entrada   │
│  (NO)    ───────→ Saída     │
│  (NC)    ───────→ [não usar]│
│                             │
└─────────────────────────────┘
```

### Conexão Detalhada Relé 4 (Segurança):
```
NodeMCU D4 (GPIO2)
    ↓
[Resistor 1kΩ]
    ↓
Relé 4 - BOBINA-
    ↓
GND NodeMCU

NodeMCU VCC 5V
    ↓
[Diodo 1N4007 - cátodo para bobina]
    ↓
Relé 4 - BOBINA+
    ↓
GND

Relé 1 NO + Relé 2 NO + Relé 3 NO (saída das 3 bobinas)
    ↓
[Capacitor 100nF]
    ↓
Relé 4 - COM (Comum)

Relé 4 - NO (Normalmente Aberto)
    ↓
BOBINA 1 + BOBINA 2 + BOBINA 3 DO MOTOR
    ↓
NEUTRO 220V

[O Relé 4 funciona em série para garantir que SEMPRE há uma bobina ligada
 antes de permitir a passagem de corrente para o motor]
```

---

## MATRIZ DE CONEXÃO - RESUMO COMPLETO

### NodeMCU - Pinos de Saída (GPIO para Relés)

| GPIO | Pino NodeMCU | Função | Destino |
|------|--------------|--------|---------|
| GPIO5 | D1 | Saída Relé 1 | Relé Velocidade 1 (BOBINA-) |
| GPIO4 | D2 | Saída Relé 2 | Relé Velocidade 2 (BOBINA-) |
| GPIO0 | D3 | Saída Relé 3 | Relé Velocidade 3 (BOBINA-) |
| GPIO2 | D4 | Saída Relé 4 | Relé Segurança (BOBINA-) |
| 3V3 | 3V3 | VCC 5V | + Bobinas Relés (via fonte 5V) |
| GND | GND | TERRA | - Bobinas Relés |

### NodeMCU - Pinos de Entrada (Botões)

| GPIO | Pino NodeMCU | Função | Botão |
|------|--------------|--------|-------|
| GPIO14 | D5 | Entrada Botão | Liga/Desliga |
| GPIO12 | D6 | Entrada Botão | Velocidade 1 |
| GPIO13 | D7 | Entrada Botão | Velocidade 2 |
| GPIO15 | D8 | Entrada Botão | Velocidade 3 |
| 3V3 | 3V3 | VCC Botões | + Botões (via pull-up 10kΩ) |
| GND | GND | TERRA | - Botões |

---

## DIAGRAMA COMPLETO - CONEXÃO 220V

```
ENTRADA PRINCIPAL 220V AC
│
├─ FASE (L) ──┬─ Relé 1 COM
│             ├─ Relé 2 COM
│             └─ Relé 3 COM
│
└─ NEUTRO (N) ──── Relé 4 NO ──────┬─ BOBINA 1 DO MOTOR
                                  ├─ BOBINA 2 DO MOTOR
                                  └─ BOBINA 3 DO MOTOR

    Relé 1 NO (Vel 1) ───┐
    Relé 2 NO (Vel 2) ───┼─── Relé 4 COM ───→ [Capacitor 100nF] ───→ Saída
    Relé 3 NO (Vel 3) ───┘
```

---

## PINAGEM DO MOTOR - 3 BOBINAS

```
MOTOR DO VENTILADOR (Conexão Interna)
┌──────────────────────────────────┐
│                                  │
│  Terminal Neutro ────────────────┼──→ NEUTRO 220V
│                                  │
│  Terminal Bobina 1 ──────────────┼──→ Relé 1 NO → FASE 220V
│                                  │
│  Terminal Bobina 2 ──────────────┼──→ Relé 2 NO → FASE 220V
│                                  │
│  Terminal Bobina 3 ──────────────┼──→ Relé 3 NO → FASE 220V
│                                  │
└──────────────────────────────────┘

IMPORTANTE: Cada bobina deve receber 220V APENAS de uma por vez
            através do respectivo relé (interbloqueio via software)
```

---

## BOTÕES - ESQUEMA DE CONEXÃO

```
Botão Liga/Desliga (D5 - GPIO14):
   VCC 5V
      │
    [10kΩ Resistor Pull-up]
      │
      ├─────→ GPIO14 (D5)
      │
   [Botão Push]
      │
     GND

Botão Velocidade 1 (D6 - GPIO12):
   VCC 5V
      │
    [10kΩ Resistor Pull-up]
      │
      ├─────→ GPIO12 (D6)
      │
   [Botão Push]
      │
     GND

Botão Velocidade 2 (D7 - GPIO13):
   VCC 5V
      │
    [10kΩ Resistor Pull-up]
      │
      ├─────→ GPIO13 (D7)
      │
   [Botão Push]
      │
     GND

Botão Velocidade 3 (D8 - GPIO15):
   VCC 5V
      │
    [10kΩ Resistor Pull-up]
      │
      ├─────→ GPIO15 (D8)
      │
   [Botão Push]
      │
     GND
```

---

## PROTEÇÃO CONTRA SOBRECARGA

### Diodos de Proteção (1N4007):

```
Cada bobina de relé recebe proteção:

    VCC 5V
      │
   [Resistor 1kΩ]
      │
   ╔══════════════════╗
   ║ Bobina do Relé   ║
   ║                  ║
   ║  [Diodo 1N4007]  ║  ← Cátodo virado para a bobina
   ║                  ║
   ╚══════════════════╝
      │
      ├─────→ GPIO (Saída do NodeMCU)
      │
     GND
```

### Capacitores de Proteção:

```
Para cada contato NO/NC do relé:

    COM ──┬─ [Capacitor 100nF] ─┬─ NO/NC
          │                     │
          └─────────────────────┘
          
Protege contra picos de tensão ao comutar.
```

---

## TABELA DE INTERBLOQUEIO - ESTADOS DOS RELÉS

| Estado do Motor | Relé 1 | Relé 2 | Relé 3 | Relé 4 | Bobina Ativa |
|-----------------|--------|--------|--------|--------|--------------|
| Desligado | DESATIVADO | DESATIVADO | DESATIVADO | DESATIVADO | Nenhuma |
| Velocidade 1 | ATIVADO | DESATIVADO | DESATIVADO | ATIVADO | Bobina 1 |
| Velocidade 2 | DESATIVADO | ATIVADO | DESATIVADO | ATIVADO | Bobina 2 |
| Velocidade 3 | DESATIVADO | DESATIVADO | ATIVADO | ATIVADO | Bobina 3 |

---

## CHECKLIST DE CONEXÃO

- [ ] GPIO5 (D1) → Relé 1 BOBINA-
- [ ] GPIO4 (D2) → Relé 2 BOBINA-
- [ ] GPIO0 (D3) → Relé 3 BOBINA-
- [ ] GPIO2 (D4) → Relé 4 BOBINA-
- [ ] GPIO14 (D5) ← Botão Liga/Desliga (com pull-up 10kΩ)
- [ ] GPIO12 (D6) ← Botão Velocidade 1 (com pull-up 10kΩ)
- [ ] GPIO13 (D7) ← Botão Velocidade 2 (com pull-up 10kΩ)
- [ ] GPIO15 (D8) ← Botão Velocidade 3 (com pull-up 10kΩ)
- [ ] 3V3 NodeMCU → VCC 5V (via fonte)
- [ ] GND NodeMCU → GND 5V (via fonte)
- [ ] FASE 220V → COM Relé 1, Relé 2, Relé 3
- [ ] NEUTRO 220V → Relé 4 NO → Bobinas do Motor
- [ ] Bobina 1 Motor → Relé 1 NO
- [ ] Bobina 2 Motor → Relé 2 NO
- [ ] Bobina 3 Motor → Relé 3 NO
- [ ] Todos os diodos 1N4007 instalados (cátodo para bobinas)
- [ ] Capacitores 100nF nos contatos dos relés
- [ ] Resistores 1kΩ em série com GPIO (proteção)
