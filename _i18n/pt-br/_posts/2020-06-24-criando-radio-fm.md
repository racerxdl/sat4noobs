---
title: Criando um Radio FM
date: 2020-06-24T21:00:00-03:00
author: Lucas Teske
description: "Neste tutorial iremos aprender a fazer um simples Rádio FM!"

layout: post
image: /assets/posts/fmradio/title.jpg
---

** WORK IN PROGRESS **

Hoje vamos aprender a fazer um simples Radio FM usando o GNU Radio! Para isso você deverá ter um [RTL-SDR](/rtl-sdr) ou uma [gravação](/recordings) em seu computador!

## Configurando Projeto

Logo após criar um novo projeto no GNU Radio devemos configurar o nome do projeto nas especificações do módulo `top-level`. Este módulo está geralmente no canto superior esquerdo.

![Bloco Top-Level](/assets/posts/fmradio/project.jpg)*Bloco Top-Level*

Clique duas vezes no bloco para abrir as configurações do projeto e selecione um nome, autor e id a sua escolha.

![Configurações do Projeto](/assets/posts/fmradio/project-config.jpg)*Configurações do Projeto*

O próximo passo é adicionar o primeiro bloco que é a `fonte` de dados. Esta fonte é diferente caso você esteja usando uma [gravação](#usando-uma-gravação) ou um [RTLSDR](#usando-rtlsdr).

## Usando RTLSDR

Para o uso de um RTLSDR você deverá usar o bloco `RTL-SDR Source`:

![Fonte de dados RTL-SDR](/assets/posts/fmradio/block-rtlsdr.jpg)*Fonte de dados RTL-SDR*

Você por enquanto pode deixar as configurações padrões:

![Configuração do RTL-SDR Source](/assets/posts/fmradio/block-rtlsdr-config.jpg)*Configuração da fonte de dados RTL-SDR*

Agora você pode continuar para [Continuando o Rádio FM](#continuando-o-rádio-fm)!

## Usando uma Gravação

Para o uso de uma gravação você deverá usar dois blocos: `File Source` e `Throttle`:

![Fonte de dados de arquivo](/assets/posts/fmradio/block-file.jpg)*Fonte de dados de arquivo*
![Bloco de Controle de Fluxo](/assets/posts/fmradio/block-throttle.jpg)*Bloco de Controle de Fluxo*

O bloco de controle de fluxo (Throttle) serve para controlar quantas amostras vão ser lidas por segundo da gravação. Sem ela o GNU Radio tentará ler tudo de uma vez e poderá ter alguns problemas em seu computador! O bloco RTLSDR já contém um limitador de fluxo interno, por isso não é nescessário.

Você poderá deixar as configurações padrões em ambos, porém no bloco `File Source` você deverá escolher o arquivo ao qual deseja ler no campo `File`. Os outros valores podem ser deixados no que já estão.

![Configuração do arquivo](/assets/posts/fmradio/block-file-config.jpg)*Configuração do arquivo*

Após isso você pode ligar os dois blocos arrastando a saída do `File Source` para a entrada do `Throttle`.

![Blocos File e Throttle ligados](/assets/posts/fmradio/block-file-throttle.jpg)*Blocos File e Throttle ligados*

Agora você pode continuar para [Continuando o Rádio FM](#continuando-o-rádio-fm)!

## Continuando o Rádio FM!

Agora que temos a nossa fonte configurada corretamente, vamos testa-la usando um `Analisador de Espectro`. Para isso iremos usar o bloco `QT Frequency Sink`

![Analisador de Espectro](/assets/posts/fmradio/block-frequency-sink.jpg)*Analisador de Espectro*

As configurações padrões devem funcionar, mas para um critério de melhor visualização podemos otimizar os parâmetros `FFT Size` e `Update Period`

![Configuração do Analisador de Espectro](/assets/posts/fmradio/block-frequency-sink-config.jpg)*Configuração do Analisador de Espectro*

- O parâmetro `FFT Size` define a resolução do analisador de espectro. Iremos configurar para `4096`.
- O parâmetro `Update Period` define a taxa de atualização da imagem que iremos gerar do espectro! Vamos configurar para `0.01`
- O parârametro `Center Frequency` define qual a frequência central marcada pelo analisador. Escreveremos `center_frequency` aqui, pois iremos definir mais tarde uma variável com este nome.

Além disso deveremos configurar a variável `samp_rate` (que é criada junto com o projeto novo) para `2.56e6` (ou `2560000`). Para isso clique duas vezes em cima do bloco da varíavel na tela e configure o campo `Value` para `2.56e6`.

![Variável da Taxa de Amostragem](/assets/posts/fmradio/block-variable.jpg)*Variável da Taxa de Amostragem*

Agora criaremos um variável nova chamada `center_frequency` onde o valor será a frequência a qual o rádio estará sintonizado. No caso de uma gravação, este efeito será meramente visual (não irá alterar nada no sinal). No exemplo iremos usar `100.9e6` (ou `100900000 Hz`). Para isto o campo `id` da variável será `center_frequency`

![Variável da Frequência Central](/assets/posts/fmradio/block-variable-frequency.jpg)*Variável da Frequência Central*

Caso você esteja usando o RTLSDR, efetue a mudança do campo `Center Frequency` também no bloco `RTL-SDR Source` (dica: você pode escrever `center_frequency` no valor do campo para usar o valor da varíavel recém criada!)

Feito isso podemos ligar o bloco `QT Frequency Sink` a fonte (no caso do `RTL-SDR Source` diretamente, no caso da gravação ligado no bloco `Throttle`).

Após todos blocos ligados, podemos executar o nosso *programa* clicando no botão **play** na barra do GNU Radio

![Botão Play](/assets/posts/fmradio/gnuradio-play-button.jpg)*Botão Play*

Depois de alguns instantes o programa criado irá aparecer:

![Visualização do Analisador de Espectro](/assets/posts/fmradio/fft-preview0.jpg)*Visualização do Analisador de Espectro*

Parabéns! Se você conseguiu ver uma imagem similar a esta, você está no caminho certo! Caso esteja usando um RTL-SDR e não esteja vendo exatamente isso veja a seção [Observações para o RTL-SDR](#observações-para-o-rtl-sdr)


## Seleção de porção do espectro

Na visualização espectral do sinal de rádio, podemos observar que há várias transmissões de rádio. As transmissões podem ser identificadas por **picos** no espectro. Na imagem abaixo, cada quadrado vermelho representa uma emissora de rádio.

![Emissoras de rádio no espectro](/assets/posts/fmradio/fft-preview0-freqs.jpg)*Emissoras de rádio no espectro*

Porém desejamos apenas uma: A que está no centro. Todo resto pode ser descartado tanto para nos ajudar a consumir menos processamento quanto para ficar mais fácil de demodular o sinal de áudio sendo transmitido.

![Emissora de interesse](/assets/posts/fmradio/fft-preview0-freqs-blur.jpg)*Emissora de interesse*

Para isso iremos usar um bloco de filtro passa-baixa **Low Pass Filter**. Para pessoas com conhecimento de áudio, poderá parecer estranho usar um filtro passa-baixa ao invés de um passa-banda. Porém trabalhando com sinais de rádio em formato de números complexos, o filtro passa-baixa tem o efeito esperado. Mais detalhes disso serão explicados em outro artigo.

Um rádio FM tem uma transmissão de mais ou menos 190kHz de largura (isso pode ser conferido pelo analisador de espectro), para isso iremos fazer um filtro passa-baixa de pelo menos 95kHz. O filtro passa-baixa irá deixar passar apenas os sinais entre `frequencia central - 95kHz` e `frequencia central + 95Khz`, desta maneira totalizando os 190kHz esperados. Iremos também efetuar o processo de [decimação](/2020/06/decimation). Este processo irá tirar a média de *N* amostras para gerar apenas *uma* amostra na saída. Este processo simultaneamente reduz o nosso ruído assim como a nossa taxa de amostragem. Para este exemplo iremos usar a taxa de decimação como `4`. Isso irá reduzir nossa taxa de amostragem de `2.56e6` (ou `2560000`) para 640e3 (ou `640000`). Para isso iremos criar uma variável `decimation` com valor `4`

![Variável do Fator de Decimação](/assets/posts/fmradio/block-variable-decimation.jpg)*Variável do Fator de Decimação*

E após isso poderemos criar o nosso `Low pass filter`

![Filtro Passa Baixa](/assets/posts/fmradio/block-low-pass.jpg)*Filtro Passa Baixa*

Configuraremos os seguintes parâmetros nele:

- `Decimation` => A taxa de decimação. O valor será `decimation` (a nossa variável recém criada)
- `Sample Rate` => a taxa de amostragem **de entrada**. O valor será `samp_rate` (a variável criada no começo do projeto)
- `Cutoff Freq` => Frequência de corte. O valor será `95e3` (ou `95000`)
- `Transition Width` => Largura da faixa de transição. O valor será de `5e3` (ou `5000`)

Os outros campos poderão ficar nos valores padrões.

![Configuração do filtro Passa Baixa](/assets/posts/fmradio/block-low-pass-config.jpg)*Configuração do filtro Passa Baixa*

Iremos também alterar nosso bloco `QT Frequency Sink` para que a taxa de amostragem seja a correta (agora iremos ligar ele na saída do filtro). Para isso iremos mudar o campo `Sample Rate` do bloco `QT Frequency Sink` para `samp_rate/decimation` (sim! podemos colocar expressões entre variáveis também :smile:) e podemos ligar ele no bloco de passa baixa ao invés da fonte.

![Bloco de passa baixa e analisador de espectro](/assets/posts/fmradio/block-low-pass-frequency-sink.jpg)*Bloco de passa baixa e analisador de espectro*

Feito isso, se iniciarmos nosso programa deveremos ver um espectro completamente diferente agora.

![Analisador de espectro com seleção do canal](/assets/posts/fmradio/fft-preview1.jpg)*Analisador de espectro com seleção do canal*

Agora apenas a faixa de 190kHz central está com sinal forte! Bingo, podemos começar a demodulação!

## Demodulação de FM

Agora podemos ir para a cereja do bolo! Usaremos um bloco chamado `FM Demod` que, como o próprio nome já diz, efetua a demodulação do sinal FM.

![Bloco FM](/assets/posts/fmradio/block-fm-demod.jpg)*Bloco FM*

Para isso iremos fazer algumas configurações:

- `Channel Rate` => É a taxa de amostragem **de entrada**. Este campo será configurado para `samp_rate/decimation`
- `Audio Decimation` => É a taxa de decimação para a **saída**. A saída desse bloco é o áudio e por isso é importante escolher a taxa correta. Como a taxa de entrada é `640e3`, uma das opções oferecidas pelo bloco `Audio Sink` (adicionado mais a frente) é uma taxa de amostragem de áudio de `32e3`. Sendo assim, caso a taxa de decimação seja `20`, teremos **exatos** `32000` na saída! Logo este campo será configurado para `20`.

Os outros campos poderão ser deixados no padrão (as configurações padrões são as mesmas dos padrões brasileiros de FM)

![Configuração do bloco FM](/assets/posts/fmradio/block-fm-demod-config.jpg)*Configuração do bloco FM*

Após isso poderemos criar o bloco que irá mandar a saída de áudio para a placa de som do computador (assim poderemos ouvir o sinal de rádio). Este bloco é o `Audio Sink` e ele terá apenas uma configuração: `Audio Rate`.

![Saída de Áudio](/assets/posts/fmradio/block-audio-sink.jpg)*Saída de Áudio*

Existem algumas opções pré-definidas de `Audio Rate`. Em geral o driver do sistema operacional irá converter o som para a taxa de amostragem correta da placa de som e por isso poderemos escolher qualquer valor. Esse valor porém precisa estar conforme todos os outros blocos esperam. O valor mais alto o qual tem um divisor inteiro para nossa taxa de amostragem (`640e3`) é `32000` (640000 / 20 = 32000). Após a configuração podemos interligar todos os blocos.

![Demodulador FM](/assets/posts/fmradio/block-full-fm-demod.jpg)*Demodulador FM*

E agora quando você rodar, você irá ouvir o áudio da radio FM!

## Observações para o RTL-SDR

TODO
