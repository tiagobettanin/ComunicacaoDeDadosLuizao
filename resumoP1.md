# Resumo Abrangente de Comunicação de Dados

Este documento consolida os principais conceitos sobre sinais, meios de transmissão, imperfeições e técnicas de modulação, com base nos materiais de aula e anotações.

---

## 1. Introdução à Comunicação de Dados

[cite_start]A **comunicação de dados** é o processo de troca de informações entre dispositivos através de um meio de transmissão[cite: 441]. [cite_start]Para contextualizar o estudo, utilizamos o **modelo OSI**, que organiza a comunicação em sete camadas[cite: 1]. Nossa disciplina foca no cerne da transmissão, abrangendo principalmente:
* [cite_start]**Camada Física:** Lida com o nível de bit, características do sinal, modulação e multiplexação[cite: 1].
* [cite_start]**Camada de Enlace:** Responsável por dividir os dados em frames, controlar o fluxo e os erros[cite: 1].

[cite_start]Todo sistema de comunicação é composto por um **transmissor (TX)**, um **canal de comunicação** e um **receptor (RX)**[cite: 451, 448, 453]. [cite_start]Durante a transmissão, o sinal está sujeito a degradações, como o **ruído**, que podem corromper a informação[cite: 454].

---

## 2. Fundamentos de Sinais

[cite_start]Para que a comunicação ocorra, a informação (dados) precisa ser convertida em sinais elétricos ou eletromagnéticos[cite: 471]. [cite_start]A propagação física desses sinais é chamada de **sinalização**[cite: 472].

### Tipos de Dados e Sinais

* [cite_start]**Dados Analógicos vs. Digitais:** Dados analógicos são contínuos (ex: voz humana), enquanto dados digitais são discretos (ex: dados de computador)[cite: 439, 440].
* [cite_start]**Sinal Analógico vs. Digital:** Sinais analógicos possuem infinitos níveis de valor, enquanto sinais digitais têm um número limitado de níveis discretos[cite: 479].
* [cite_start]**Sinais Periódicos vs. Aperiódicos:** Sinais periódicos repetem um padrão ao longo do tempo, e os aperiódicos não[cite: 488, 489].

### A Onda Senoidal: O Bloco de Construção

[cite_start]A forma mais fundamental de um sinal analógico periódico é a **onda senoidal**[cite: 558], descrita por:

* [cite_start]**Amplitude (A):** A intensidade do sinal[cite: 494].
* [cite_start]**Frequência (f):** O número de ciclos por segundo (Hz)[cite: 494].
* [cite_start]**Período (T):** O tempo para completar um ciclo ($T = 1/f$)[cite: 496].
* [cite_start]**Fase ($\Phi$):** A posição da onda em relação ao tempo zero[cite: 495].
* [cite_start]**Comprimento de Onda ($\lambda$):** A distância de um ciclo no meio de transmissão ($\lambda = v/f$, onde $v$ é a velocidade)[cite: 497, 499].
    * **Exemplo:** Um sinal de 10 kHz ($T = 10^{-4}$ s) no vácuo (velocidade da luz, $c \approx 3 \times 10^8$ m/s) tem um comprimento de onda de $\lambda = 3 \times 10^8 \times 10^{-4} = 30$ km.

### Composição de Sinais e Largura de Banda

[cite_start]De acordo com a **análise de Fourier**, qualquer sinal pode ser decomposto em uma soma de ondas senoidais simples[cite: 636].
* [cite_start]Um sinal digital periódico, como uma onda quadrada, é composto pela superposição de uma onda senoidal (frequência fundamental) e infinitas **harmônicas ímpares**[cite: 638]. A utilização apenas de harmônicas ímpares é uma propriedade matemática da decomposição em Série de Fourier para uma onda quadrada simétrica.
* [cite_start]No **domínio da frequência**, o gráfico de um sinal periódico consiste em linhas verticais discretas, enquanto o de um sinal não periódico é uma curva contínua, pois é composto por infinitas frequências[cite: 639, 718].

[cite_start]A **largura de banda (Bandwidth)** é a diferença entre a frequência mais alta e a mais baixa de um sinal[cite: 741]. Ela influencia diretamente a taxa de transmissão e a qualidade do sinal. Se um sinal for transmitido com poucas harmônicas (largura de banda limitada), ele pode se distorcer a ponto de o receptor não conseguir interpretá-lo corretamente.

---

## 3. Imperfeições na Transmissão

[cite_start]Os meios de transmissão não são perfeitos e causam degradação do sinal[cite: 1250, 1251]. As três principais causas são:

### Atenuação
[cite_start]É a perda de energia do sinal ao se propagar pelo meio[cite: 1258, 1259].
* Frequências mais altas se atenuam mais rapidamente que as mais baixas.
* Pode-se compensar aumentando a potência do transmissor, mas isso eleva o consumo de energia.
* [cite_start]É medida em **decibéis (dB)**[cite: 1278]. [cite_start]Uma perda de 3 dB significa que a potência do sinal caiu pela metade[cite: 1284].
    * **Exemplo:** Se um sinal de 5W sofre uma atenuação de 10 dB, a potência final (P2) é:
        ```
        -10 dB = 10 * log10(P2 / 5)
        -1 = log10(P2 / 5)
        10^-1 = P2 / 5  =>  P2 = 0.5 W
        ```

### Distorção
[cite_start]É a alteração da forma do sinal [cite: 1392][cite_start], que ocorre porque diferentes componentes de frequência viajam a velocidades distintas, chegando defasadas ao destino[cite: 1393, 1394].

### Ruído
[cite_start]É o principal fator que limita o desempenho[cite: 1406]. Tipos comuns incluem:
* **Ruído Térmico (Branco):** Gerado pela agitação de elétrons; [cite_start]é inevitável[cite: 1421, 1423, 1424].
* [cite_start]**Crosstalk (Diafonia):** Interferência entre canais ou cabos vizinhos[cite: 1438].
* [cite_start]**Ruído por Impulso:** Picos de energia de fontes externas (ex: raios), muito prejudiciais a dados digitais[cite: 1443, 1444, 1448].

[cite_start]A **Relação Sinal-Ruído (SNR)** mede a potência do sinal em relação à do ruído[cite: 1487]. [cite_start]Um SNR alto indica um sinal de melhor qualidade[cite: 1493].

---

## 4. Limites na Taxa de Dados

[cite_start]A velocidade máxima de transmissão é limitada pela largura de banda, níveis de sinal e ruído[cite: 1519, 1520, 1521, 1522].

### Taxa de Nyquist (Canal sem Ruído)
[cite_start]Para um canal ideal, a taxa de bits máxima é: `Taxa = 2 × B × log₂ L`, onde `B` é a largura de banda e `L` é o número de níveis de sinal discretos[cite: 1530, 1531].
* [cite_start]**Exemplo (Amostragem de Áudio):** O ouvido humano capta até 22 kHz[cite: 1]. Pelo Teorema de Nyquist, a taxa de amostragem deve ser o dobro da frequência máxima: `fs = 2 * 22 kHz = 44 kHz`. É por isso que CDs usam 44.1 kHz. Se cada amostra usar 8 bits, teremos `L = 2^8 = 256` níveis de sinal.

### Capacidade de Shannon (Canal com Ruído)
[cite_start]Para um canal real, a capacidade máxima teórica (em bps) é: `Capacidade = B × log₂(1 + SNR)`[cite: 1573].
* **Exemplo:** Um canal de 20 kHz com SNR de 40 dB.
    1.  **Converter SNR de dB:**
        `40 dB = 10 * log10(SNR)  =>  SNR = 10^4 = 10.000`
    2.  **Calcular Capacidade:**
        `C = 20.000 * log₂(1 + 10.000) ≈ 265.000 bps` ou `265 kbps`.

---

## 5. Meios de Transmissão

[cite_start]Os meios de transmissão são os caminhos físicos que transportam o sinal e se dividem em **guiados** (com fios) e **não guiados** (sem fios)[cite: 78].

### Meios Guiados

* [cite_start]**Cabo de Par Trançado:** Pares de fios de cobre trançados para reduzir ruídos[cite: 89, 92].
    * [cite_start]**UTP (Unshielded):** Sem blindagem[cite: 96].
    * [cite_start]**STP (Shielded):** Possui uma blindagem metálica que funciona de forma análoga a uma **Gaiola de Faraday**, protegendo o sinal[cite: 96].
    * [cite_start]**Padrões:** A especificação `10BASE-T` indica um padrão Ethernet que opera a **10 Mbps** sobre par trançado[cite: 105].

* [cite_start]**Cabo Coaxial:** Núcleo condutor central com uma blindagem externa[cite: 153, 155]. [cite_start]Oferece mais largura de banda que o par trançado[cite: 157].

* [cite_start]**Fibra Óptica:** Transmite dados como pulsos de luz, sendo imune a ruídos eletromagnéticos e com baixíssima atenuação[cite: 194, 220].
    * A atenuação na fibra não é linear; existem picos em certos comprimentos de onda (ex: próximo a 1,4 micrômetros) onde o material do vidro absorve parte da energia do sinal.

### Meios Não Guiados

[cite_start]Transportam ondas eletromagnéticas pelo espaço livre[cite: 319].

* [cite_start]**Antenas:** Dispositivos que irradiam ou captam as ondas[cite: 352]. O tamanho da antena está relacionado ao comprimento de onda do sinal.
    * **Exemplo (Antena Dipolo):** Para uma rádio FM em **102.9 MHz**:
        1.  `T = 1 / (102.9 * 10^6) ≈ 9.718 * 10^-9 s`
        2.  `λ = c * T = 3 * 10^8 * 9.718 * 10^-9 ≈ 2.915 metros`
        3.  Uma antena dipolo de meia onda ideal teria `λ / 2 ≈ 1.457 metros`.

* [cite_start]**Satélites:** Atuam como repetidores de micro-ondas no espaço, usando uma frequência para receber (**uplink**) e outra para retransmitir (**downlink**)[cite: 382, 383].

---

## 6. Técnicas de Modulação

[cite_start]**Modulação** é o processo de alterar uma onda portadora de alta frequência com base em um sinal de informação[cite: 869].

### Modulação de Dados Analógicos
* [cite_start]**AM (Modulação em Amplitude):** A amplitude da portadora varia[cite: 893]. [cite_start]É simples, mas suscetível a ruído[cite: 910].
* [cite_start]**FM (Modulação em Frequência):** A frequência da portadora varia[cite: 900]. [cite_start]É mais resistente a ruído, mas exige maior largura de banda[cite: 922].

### Modulação de Dados Digitais (Shift Keying)
* [cite_start]**ASK (Amplitude-Shift Keying):** A amplitude assume valores discretos para representar os bits[cite: 997].
* [cite_start]**FSK (Frequency-Shift Keying):** A frequência assume valores discretos[cite: 1001].
* [cite_start]**PSK (Phase-Shift Keying):** A fase assume valores discretos[cite: 1044]. [cite_start]É mais robusta a ruídos que a ASK[cite: 1048].
* [cite_start]**QAM (Quadrature Amplitude Modulation):** Combina ASK e PSK, alterando amplitude e fase para transmitir mais bits por elemento de sinal (baud)[cite: 1147, 1148].
    * [cite_start]**4-QAM (ou QPSK):** Transmite **2 bits por sinal**[cite: 1082, 1221].
    * [cite_start]**8-QAM:** Transmite **3 bits por sinal**[cite: 1164, 1165].