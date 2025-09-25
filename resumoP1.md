# Resumo Abrangente de Comunicação de Dados

Este documento consolida os principais conceitos sobre sinais, meios de transmissão, imperfeições e técnicas de modulação, com base nos materiais de aula e anotações.

---

## 1. Introdução à Comunicação de Dados

A **comunicação de dados** é o processo de troca de informações entre dispositivos através de um meio de transmissão. Para contextualizar o estudo, utilizamos o **modelo OSI**, que organiza a comunicação em sete camadas. Nossa disciplina foca no cerne da transmissão, abrangendo principalmente:
* **Camada Física:** Lida com o nível de bit, características do sinal, modulação e multiplexação.
* **Camada de Enlace:** Responsável por dividir os dados em frames, controlar o fluxo e os erros.

Todo sistema de comunicação é composto por um **transmissor (TX)**, um **canal de comunicação** e um **receptor (RX)**. Durante a transmissão, o sinal está sujeito a degradações, como o **ruído**, que podem corromper a informação.

---

## 2. Fundamentos de Sinais

Para que a comunicação ocorra, a informação (dados) precisa ser convertida em sinais elétricos ou eletromagnéticos. A propagação física desses sinais é chamada de **sinalização**.

### Tipos de Dados e Sinais

* **Dados Analógicos vs. Digitais:** Dados analógicos são contínuos (ex: voz humana), enquanto dados digitais são discretos (ex: dados de computador).
* **Sinal Analógico vs. Digital:** Sinais analógicos possuem infinitos níveis de valor, enquanto sinais digitais têm um número limitado de níveis discretos.
* **Sinais Periódicos vs. Aperiódicos:** Sinais periódicos repetem um padrão ao longo do tempo, e os aperiódicos não.

### A Onda Senoidal: O Bloco de Construção

A forma mais fundamental de um sinal analógico periódico é a **onda senoidal**, descrita por:

* **Amplitude (A):** A intensidade do sinal.
* **Frequência (f):** O número de ciclos por segundo (Hz).
* **Período (T):** O tempo para completar um ciclo ($T = 1/f$).
* **Fase ($\Phi$):** A posição da onda em relação ao tempo zero.
* **Comprimento de Onda ($\lambda$):** A distância de um ciclo no meio de transmissão ($\lambda = v/f$, onde $v$ é a velocidade).
    * **Exemplo:** Um sinal de 10 kHz ($T = 10^{-4}$ s) no vácuo (velocidade da luz, $c \approx 3 \times 10^8$ m/s) tem um comprimento de onda de $\lambda = 3 \times 10^8 \times 10^{-4} = 30$ km.

### Composição de Sinais e Largura de Banda

De acordo com a **análise de Fourier**, qualquer sinal pode ser decomposto em uma soma de ondas senoidais simples.
* Um sinal digital periódico, como uma onda quadrada, é composto pela superposição de uma onda senoidal (frequência fundamental) e infinitas **harmônicas ímpares**. A utilização apenas de harmônicas ímpares é uma propriedade matemática da decomposição em Série de Fourier para uma onda quadrada simétrica.
* No **domínio da frequência**, o gráfico de um sinal periódico consiste em linhas verticais discretas, enquanto o de um sinal não periódico é uma curva contínua, pois é composto por infinitas frequências.

A **largura de banda (Bandwidth)** é a diferença entre a frequência mais alta e a mais baixa de um sinal. Ela influencia diretamente a taxa de transmissão e a qualidade do sinal. Se um sinal for transmitido com poucas harmônicas (largura de banda limitada), ele pode se distorcer a ponto de o receptor não conseguir interpretá-lo corretamente.

---

## 3. Imperfeições na Transmissão

Os meios de transmissão não são perfeitos e causam degradação do sinal. As três principais causas são:

### Atenuação
É a perda de energia do sinal ao se propagar pelo meio.
* Frequências mais altas se atenuam mais rapidamente que as mais baixas.
* Pode-se compensar aumentando a potência do transmissor, mas isso eleva o consumo de energia.
* É medida em **decibéis (dB)**. Uma perda de 3 dB significa que a potência do sinal caiu pela metade.
    * **Exemplo:** Se um sinal de 5W sofre uma atenuação de 10 dB, a potência final (P2) é:
        ```
        -10 dB = 10 * log10(P2 / 5)
        -1 = log10(P2 / 5)
        10^-1 = P2 / 5  =>  P2 = 0.5 W
        ```

### Distorção
É a alteração da forma do sinal, que ocorre porque diferentes componentes de frequência viajam a velocidades distintas, chegando defasadas ao destino.

### Ruído
É o principal fator que limita o desempenho. Tipos comuns incluem:
* **Ruído Térmico (Branco):** Gerado pela agitação de elétrons; é inevitável.
* **Crosstalk (Diafonia):** Interferência entre canais ou cabos vizinhos.
* **Ruído por Impulso:** Picos de energia de fontes externas (ex: raios), muito prejudiciais a dados digitais.

A **Relação Sinal-Ruído (SNR)** mede a potência do sinal em relação à do ruído. Um SNR alto indica um sinal de melhor qualidade.

---

## 4. Limites na Taxa de Dados

A velocidade máxima de transmissão é limitada pela largura de banda, níveis de sinal e ruído.

### Taxa de Nyquist (Canal sem Ruído)
Para um canal ideal, a taxa de bits máxima é: `Taxa = 2 × B × log₂ L`, onde `B` é a largura de banda e `L` é o número de níveis de sinal discretos.
* **Exemplo (Amostragem de Áudio):** O ouvido humano capta até 22 kHz. Pelo Teorema de Nyquist, a taxa de amostragem deve ser o dobro da frequência máxima: `fs = 2 * 22 kHz = 44 kHz`. É por isso que CDs usam 44.1 kHz. Se cada amostra usar 8 bits, teremos `L = 2^8 = 256` níveis de sinal.

### Capacidade de Shannon (Canal com Ruído)
Para um canal real, a capacidade máxima teórica (em bps) é: `Capacidade = B × log₂(1 + SNR)`.
* **Exemplo:** Um canal de 20 kHz com SNR de 40 dB.
    1.  **Converter SNR de dB:**
        `40 dB = 10 * log10(SNR)  =>  SNR = 10^4 = 10.000`
    2.  **Calcular Capacidade:**
        `C = 20.000 * log₂(1 + 10.000) ≈ 265.000 bps` ou `265 kbps`.

---

## 5. Meios de Transmissão

Os meios de transmissão são os caminhos físicos que transportam o sinal e se dividem em **guiados** (com fios) e **não guiados** (sem fios).

### Meios Guiados

* **Cabo de Par Trançado:** Pares de fios de cobre trançados para reduzir ruídos.
    * **UTP (Unshielded):** Sem blindagem.
    * **STP (Shielded):** Possui uma blindagem metálica que funciona de forma análoga a uma **Gaiola de Faraday**, protegendo o sinal.
    * **Padrões:** A especificação `10BASE-T` indica um padrão Ethernet que opera a **10 Mbps** sobre par trançado.

* **Cabo Coaxial:** Núcleo condutor central com uma blindagem externa. Oferece mais largura de banda que o par trançado.

* **Fibra Óptica:** Transmite dados como pulsos de luz, sendo imune a ruídos eletromagnéticos e com baixíssima atenuação.
    * A atenuação na fibra não é linear; existem picos em certos comprimentos de onda (ex: próximo a 1,4 micrômetros) onde o material do vidro absorve parte da energia do sinal.

### Meios Não Guiados

Transportam ondas eletromagnéticas pelo espaço livre.

* **Antenas:** Dispositivos que irradiam ou captam as ondas. O tamanho da antena está relacionado ao comprimento de onda do sinal.
    * **Exemplo (Antena Dipolo):** Para uma rádio FM em **102.9 MHz**:
        1.  `T = 1 / (102.9 * 10^6) ≈ 9.718 * 10^-9 s`
        2.  `λ = c * T = 3 * 10^8 * 9.718 * 10^-9 ≈ 2.915 metros`
        3.  Uma antena dipolo de meia onda ideal teria `λ / 2 ≈ 1.457 metros`.

* **Satélites:** Atuam como repetidores de micro-ondas no espaço, usando uma frequência para receber (**uplink**) e outra para retransmitir (**downlink**).

---

## 6. Técnicas de Modulação

**Modulação** é o processo de alterar uma onda portadora de alta frequência com base em um sinal de informação.

### Modulação de Dados Analógicos
* **AM (Modulação em Amplitude):** A amplitude da portadora varia. É simples, mas suscetível a ruído.
* **FM (Modulação em Frequência):** A frequência da portadora varia. É mais resistente a ruído, mas exige maior largura de banda.

### Modulação de Dados Digitais (Shift Keying)
* **ASK (Amplitude-Shift Keying):** A amplitude assume valores discretos para representar os bits.
* **FSK (Frequency-Shift Keying):** A frequência assume valores discretos.
* **PSK (Phase-Shift Keying):** A fase assume valores discretos. É mais robusta a ruídos que a ASK.
* **QAM (Quadrature Amplitude Modulation):** Combina ASK e PSK, alterando amplitude e fase para transmitir mais bits por elemento de sinal (baud).
    * **4-QAM (ou QPSK):** Transmite **2 bits por sinal**.
    * **8-QAM:** Transmite **3 bits por sinal**.