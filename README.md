# ⚡ Projeto Piezoelectricidade - FECART

> ⚠️ **Status do Projeto:** Fase de Aguardo de Componentes & Modelagem Conceitual.

---

## 📌 Sobre o Projeto

Este projeto está sendo desenvolvido para a **FECART** com o objetivo de estudar e aplicar a **piezoeletricidade** na conversão de energia mecânica (pressão, impacto e vibração) em energia elétrica limpa e reutilizável.

---

## 🛠️ Componentes Selecionados

A seleção de componentes já foi finalizada e o projeto aguarda a chegada do material para início da montagem:

* **Pastilhas Piezoelétricas:** Captação de pulsos elétricos a partir de pressão mecânica.
* **Diodos Retificadores (1N4007):** Montagem da ponte retificadora (conversão de AC para DC).
* **Capacitor Eletrolítico:** Armazenamento e filtragem de picos de tensão.
* **Resistores de Carga / Divisor de Tensão:** Proteção das entradas analógicas do microcontrolador.
* **Arduino (Uno/Nano):** Leitura de dados e processamento de sinal.
* **Display / LEDs Indicação:** Visualização do estado de carga e picos de tensão.
* **Protoboard & Jumpers:** Conexão e testes de circuito.

---

## 📐 Fluxo do Circuito
## 🎯 Lista de Tarefas (Checklist)

- [x] Definição do tema e escopo do projeto
- [x] Seleção e especificação de todos os componentes
- [ ] Recebimento dos componentes físicos
- [ ] Montagem do circuito na protoboard
- [ ] Upload e calibração do código no Arduino
- [ ] Testes de geração de tensão e armazenamento
- [ ] Confecção da estrutura final / Maquete para a FECART

---

## 💻 Código Fonte (Arduino)

Este é o código unificado para o Arduino. Ele realiza a leitura analógica da tensão gerada pelas pastilhas piezoelétricas, calcula o valor aproximado em Volts e aciona LEDs indicadores de acordo com a intensidade da energia gerada.

```cpp
/*
 * Projeto Piezoelectricidade - FECART
 * Código Unificado de Leitura e Sinalização
 */

// --- Configuração dos Pinos ---
const int pinoPiezo = A0;      // Entrada analógica conectada ao circuito piezo
const int pinLedVerde = 8;     // LED indicador de baixa tensão / presença de pulso
const int pinLedAmarelo = 9;   // LED indicador de média tensão
const int pinLedVermelho = 10;  // LED indicador de alta tensão / pico

// --- Variáveis de Leitura ---
int valorAnalógico = 0;
float tensaoCalculada = 0.0;
const float tensaoReferencia = 5.0; // Tensão de referência do Arduino (5V)

void setup() {
  // Inicialização da Comunicação Serial
  Serial.begin(9600);
  Serial.println("==========================================");
  Serial.println("   FECART - Sistema de Piezoeletricidade   ");
  Serial.println("==========================================");

  // Configuração dos Pinos dos LEDs
  pinMode(pinLedVerde, OUTPUT);
  pinMode(pinLedAmarelo, OUTPUT);
  pinMode(pinLedVermelho, OUTPUT);

  // Teste inicial dos LEDs
  digitalWrite(pinLedVerde, HIGH);
  digitalWrite(pinLedAmarelo, HIGH);
  digitalWrite(pinLedVermelho, HIGH);
  delay(1000);
  digitalWrite(pinLedVerde, LOW);
  digitalWrite(pinLedAmarelo, LOW);
  digitalWrite(pinLedVermelho, LOW);
}

void loop() {
  // 1. Leitura do valor bruto no pino analógico (0 a 1023)
  valorAnalógico = analogRead(pinoPiezo);

  // 2. Conversão do valor lido para Volts (0V a 5V)
  tensaoCalculada = (valorAnalógico * tensaoReferencia) / 1023.0;

  // 3. Exibição dos dados no Monitor Serial (para diagnóstico)
  if (valorAnalógico > 10) { // Filtro simples para ignorar ruídos muito baixos
    Serial.print("Leitura Bruta: ");
    Serial.print(valorAnalógico);
    Serial.print(" | Tensão Estimada: ");
    Serial.print(tensaoCalculada, 2);
    Serial.println(" V");
  }

  // 4. Lógica de acionamento dos LEDs conforme a intensidade
  if (tensaoCalculada >= 0.5 && tensaoCalculada < 1.8) {
    digitalWrite(pinLedVerde, HIGH);
    digitalWrite(pinLedAmarelo, LOW);
    digitalWrite(pinLedVermelho, LOW);
  } 
  else if (tensaoCalculada >= 1.8 && tensaoCalculada < 3.5) {
    digitalWrite(pinLedVerde, HIGH);
    digitalWrite(pinLedAmarelo, HIGH);
    digitalWrite(pinLedVermelho, LOW);
  } 
  else if (tensaoCalculada >= 3.5) {
    digitalWrite(pinLedVerde, HIGH);
    digitalWrite(pinLedAmarelo, HIGH);
    digitalWrite(pinLedVermelho, HIGH);
  } 
  else {
    // Apaga os LEDs quando não houver pulso detectado
    digitalWrite(pinLedVerde, LOW);
    digitalWrite(pinLedAmarelo, LOW);
    digitalWrite(pinLedVermelho, LOW);
  }

  delay(50); // Pequeno atraso para estabilização das leituras
}


Kenzo - @KenzoAF

Carlos Eduardo - @kadufecap-blip

Lorenzo - @LoScorza2
