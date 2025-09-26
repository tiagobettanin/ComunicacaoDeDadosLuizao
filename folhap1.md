# Resumo Completo - Comunicação de Dados

## 1. Fundamentos de Sinais
- **Sinal Analógico vs. Digital:** Analógico é contínuo; Digital é discreto.
- **Onda Senoidal:** `s(t) = A * sin(2πft + Φ)`
  - `A`: Amplitude (força do sinal)
  - `f`: Frequência (Hz)
  - `t`: Tempo (s)
  - `Φ`: Fase (deslocamento em radianos ou graus)
- **Comprimento de Onda (λ):** Distância de um ciclo.
  - **Fórmula:** `λ = v / f`
    - `v`: Velocidade de propagação (no vácuo, `~3 x 10⁸ m/s`)
    - `f`: Frequência (Hz)
- **Fase:** Um ciclo completo = 360° = 2π radianos. Um deslocamento de 1/6 de ciclo = `360 / 6 = 60°`.

## 2. Composição de Sinais e Largura de Banda
- **Análise de Fourier:** Sinais periódicos complexos são a soma de uma **frequência fundamental (1ª harmônica)** com suas **harmônicas** (múltiplos inteiros da fundamental).
- **Onda Quadrada Simétrica:** Composta apenas por harmônicas **ímpares**.
- **Espectro de Frequência:**
  - **Sinal Periódico:** Espectro com frequências **discretas** (linhas finas).
  - **Sinal Não Periódico:** Espectro com frequências **contínuas** (área preenchida).
- **Largura de Banda (B):**
  - **De um Sinal:** `B = F_max - F_min`
  - **Necessária para Modulação (Simplificado):**
    - `B ≈ S` (Baud Rate) para ASK, PSK.
    - `B ≈ S + 2Δf` para FSK.

## 3. Imperfeições e Desempenho
- **Atenuação:** Perda de energia do sinal. Frequências mais altas atenuam mais em cabos.
  - **Fórmula (Decibel - dB):** `G_dB = 10 * log₁₀(P₂ / P₁)`
    - `G_dB`: Ganho/Perda em dB.
    - `P₁`: Potência inicial.
    - `P₂`: Potência final.
- **Ruído:** Sinal indesejado. Tipos:
  - **Térmico:** Inevitável, agitação de elétrons (chiado branco).
  - **Intermodulação:** Mistura de sinais, criando novas frequências (soma/diferença).
  - **Crosstalk (Diafonia):** Interferência entre canais/cabos vizinhos.
  - **Impulso:** Picos de energia externos (raios, motores). Causa principal de erros em dados digitais.
- **Relação Sinal-Ruído (SNR):**
  - **Razão Direta (sem unidade):** `SNR = Potência_Sinal / Potência_Ruído`
  - **Em Decibéis:** `SNR_dB = 10 * log₁₀(SNR)`
  - **Conversão Inversa:** `SNR = 10^(SNR_dB / 10)`

## 4. Taxas e Capacidade de Canal
- **Taxa de Amostragem (Nyquist):** Mínimo para evitar **aliasing** (sinal fantasma de baixa frequência).
  - **Fórmula:** `f_s(mínima) = 2 * f_max`
- **Taxa de Sinalização (S):** Velocidade dos símbolos. Relação com a largura de banda.
  - **Fórmula:** `S_max (baud) = 2 * B (Hz)`
- **Taxa de Bits (N) vs. Taxa de Sinalização (S):**
  - **Fórmula:** `N (bps) = S (baud) * r (bits/sinal)`
    - `r = log₂(L)` (`L` = número de níveis/estados de sinal)
- **Taxa de Bits Máxima (Nyquist - Canal sem Ruído):**
  - **Fórmula Completa:** `N = 2 * B * r`
  - **Fórmula Simplificada (usada em exercícios de modulação):** `N = B * r`
- **Capacidade de Shannon (Canal com Ruído):**
  - **Fórmula:** `C (bps) = B (Hz) * log₂(1 + SNR)`

## 5. Meios de Transmissão
- **Guiados (Cabos):** O meio físico define as limitações.
  - **Par Trançado:** Fios trançados para cancelar ruído/crosstalk. UTP (sem blindagem), STP (com blindagem).
  - **Coaxial:** Núcleo central blindado. Melhor contra ruído que par trançado.
  - **Fibra Óptica:** Pulses de luz guiados por reflexão. Imune a ruído eletromagnético.
    - **Multimodo:** Núcleo largo, múltiplos caminhos de luz. Para distâncias menores.
    - **Monomodo:** Núcleo muito fino, um único caminho de luz. Para longas distâncias e altíssima velocidade.
- **Não Guiados (Sem Fio):** O sinal e a antena definem as limitações.
  - **Propagação:** Ionosférica (reflete na ionosfera, 2-30 MHz) vs. Linha de Visada (ondas viajam em linha reta, >30 MHz).
  - **Ondas:** Omnidirecionais (todas as direções, ex: rádio) vs. Unidirecionais (focadas, ex: micro-ondas).

## 6. Conceitos Chave e Modulação
- **Amplificador vs. Repetidor:**
  - **Amplificador (Analógico):** Aumenta a força do sinal E do ruído.
  - **Repetidor (Digital):** Interpreta os bits e **regera** um sinal novo e limpo, eliminando o ruído.
- **Modulação AM vs. FM:**
  - **AM (Amplitude):** Informação na amplitude. Mais suscetível a ruído e atenuação.
  - **FM (Frequência):** Informação na frequência. Mais robusta contra ruídos que afetam a amplitude.
- **Técnicas de Modulação Digital e Bits por Símbolo (r):**
  - **ASK (Amplitude):** `r = 1` (forma básica)
  - **FSK (Frequência):** `r = 1` (forma básica)
  - **BPSK (Fase):** `r = 1`
  - **QPSK (Fase):** `r = 2`
  - **16-QAM (Amplitude+Fase):** `r = 4` (pois `log₂(16) = 4`)
  - **64-QAM (Amplitude+Fase):** `r = 6` (pois `log₂(64) = 6`)