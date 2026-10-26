<link rel="stylesheet" href="https://unpkg.com/sakura.css/css/sakura.css" type="text/css">

# Trabalho prático: Elastic Optical Networks 
**Disciplina:** Redes de Computadores II
**Grupo 15:** Carlos Rodrigues, Eduardo Martins e Eduardo Freire

---

## 1. Introdução às Redes Ópticas Elásticas
Redes ópticas elásticas (EONs) são uma evolução recente das redes de fibra óptica, proposta por M. Jinno em 2008, e podem ser caracterizadas como sucessoras da tecnologia DWDM (dense wavelength division multiplexing). Para a compreensão das vantagens das EONs, vamos esclarecer as características da fibra óptica e de suas diferentes versões.

A tecnologia de fibra óptica é pautada em transmitir dados na forma de luz, o que permite taxas de transmissão muito altas e grandes distâncias. Uma fibra óptica é um filamento de espessura comparável à de um fio de cabelo, normalmente construído com sílica, um tipo de vidro. Ela é composta por um núcleo, por uma casca, cujo índice de refração é menor que o do núcleo, e por um revestimento protetor. Com essa configuração, a luz sofre reflexão interna total e permanece no núcleo, assim sendo transmitida pela fibra.

Durante a instalação, que pode ser um processo caro, unem-se várias fibras ópticas em cada cabo. Embora fibra óptica seja um tipo de enlace, muitas redes são projetadas visando ao uso predominante de fibra óptica, e por isso são convenientemente denominadas redes de fibra óptica.

Uma técnica essencial utilizada em enlaces ópticos modernos é a multiplexação por divisão do comprimento de onda (WDM), uma especialização óptica da multiplexação por divisão de frequência (FDM). Ela permite que diferentes canais sejam transmitidos simultaneamente em uma única fibra, cada um em uma faixa de comprimento de onda. Podemos citar 2 categorias de WDM. A multiplexação esparsa (CWDM), em que cada canal ocupa 20nm, transmite até 18 canais entre 1270nm e 1610nm. A multiplexação densa (DWDM) reserva 0,4nm a 0,8nm a cada canal, permitindo a transmissão de mais de 100 canais em uma fibra óptica, acarretando taxa de transmissão total ainda maior.

Transmitir um sinal a uma taxa muito alta em uma pequena faixa de comprimento de onda, como faz o DWDM, implica problemas complexos. A altas taxas, cada símbolo (que representa 1 ou mais bits) tem duração muito pequena. Conforme o sinal viaja, ocorre o fenômeno de dispersão, que pode levar à interferência entre símbolos consecutivos (ISI), e à corrupção dos dados. Para que isso não aconteça, redes DWDM precisam de intervalos de guarda entre símbolos, em que nada é transmitido. Esse fenômeno atua como limitante da taxa de transmissão.

Nesse contexto, são propostas as redes ópticas elásticas, baseadas na tecnologia OFDM (multiplexação por divisão de frequência ortogonal). Enlaces EON são bem menos vulneráveis à dispersão, o que permite taxas de transmissão ainda maiores, o que é essencial frente à demanda imposta por tendências modernas como aplicações de streaming. Além disso, uma fibra óptica EON pode ser dividida entre conexões de forma mais granular e eficiente. No capítulo seguinte, discutiremos como isso é possível.

## 2. Arquiteturas e Tecnologias
Primeiramente, vamos esclarecer do que se trata a OFDM. Em um enlace FDM tradicional, as faixas de frequência que estão transmitindo não podem se sobrepor. A OFDM, porém, divide cada canal em subportadoras cujas faixas de frequência admitem certa sobreposição, desde que a sobreposição seja ortogonal. Nessas condições, o pico de uma subportadora coincide com o cruzamento de zero de outra, e é possível a recuperação dos sinais originais. Esse arranjo permite a transmissão de várias subportadoras em uma pequena faixa de frequência, característica que denominamos eficiência espectral.

![](./assets/fdm_ofdm.png)

Além disso, em um enlace OFDM, cada subportadora transmite a uma taxa pequena. Como resultado, os símbolos são transmitidos com duração maior, e tornam-se bem menos vulneráveis à dispersão e à interferência entre símbolos. Por isso, os intervalos de guarda podem ser menores. Além disso, a grande quantidade de subportadoras em paralelo implica que a taxa de transmissão total por canal é substancialmente maior do que seria em um enlace FDM.

Além de poder transmitir a uma taxa maior, uma EON é, em vários aspectos, mais flexível do que uma rede DWDM tradicional. Em um enlace DWDM, cada canal tem uma faixa de comprimento de onda fixa (digamos, de 0.4nm), que possibilita certa taxa de transmissão. Contudo, frequentemente, é necessário alocar um canal a uma conexão que não demanda essa quantidade de recursos, o que implica desperdício. Em um enlace OFDM, contudo, cada canal pode receber apenas a quantidade de subportadoras necessárias para suprir a demanda da conexão.

Outra funcionalidade de enlaces OFDM é a modulação adaptativa (AMC). Cada canal pode ser seguir um algoritmo de modulação diferente, dependendo das condições de transmissão. Por exemplo, um canal com caminho óptico curto pode usar QAM (modulação de amplitude em quadratura) com 16 símbolos diferentes e 4 bits por símbolo. Ao mesmo tempo, um canal com caminho óptico mais comprido pode usar QPSK (chaveamento por deslocamento de fase em quadratura), que carrega apenas 2 bits por símbolo, mas é mais resistente a corrupção de dados.

Essas funcionalidades são suportadas pelo transponder de largura de banda variável (BVT). Em geral no contexto de fibra óptica, um transponder é um dispositivo que recebe um sinal óptico em uma faixa de comprimento de onda e retransmite em outra. Visando à flexibilidade das EONs, um BVT é capaz de ajustar seu comportamente em função da taxa de dados que precisa ser transmitida de forma eficiente. Esse objetivo é atingido por meio do controle do número de subportadoras necessário e da modulação adaptativa, mencionados acima.

## 3. O problema de Roteamento e Alocação de Espectro

O problema de Roteamento e Alocação de Espectro (Routing and Spectre Allocation) é o problema de otimização central de otimização e controle em redes ópticas elásticas. Substitui e deriva do clássico problema de Roteamento e Atribuição de Comprimento de Onda (Routing and Wavelength Attribution) das redes WDM tradicionais ao substituir a grade fixa de faixas (tipicamente 50 GHz ou 100 GHz) por uma grade flexível (flex-grid, estandardizada pelo ITU-T G.694.1) composta por unidades granulares denominadas Slots de Frequência (Frequency Slots), normalmente de 12,5 GHz (ou múltiplos de 6,25 GHz). 

Para atender a uma requisição de conexão óptica (lightpath) entre um nó de origem "s" e um nó de destino "d" com taxa de transmissão "B", o objetivo do RSA consiste em determinar, simultaneamente, uma rota física no grafo da rede e um conjunto de slots de frequência capazes de transportar o sinal sem violar as restrições físicas da camada óptica.

<img width="399" height="250" alt="images" src="https://github.com/user-attachments/assets/4dcf1141-e9ab-49e2-a7ac-6596df22b78b" />

A figura acima contrasta uma grade fixa WDM e uma grade flexível EON. Enquanto a rede WDM (A) subutiliza um canal inteiro de 50 GHz para uma demanda de 10G e exige múltiplos canais independentes para 400G, a rede óptica elástica (B) aloca exatamente a largura espectral necessária para demandas heterogêneas (10G, 60G, 300G) agrupando slots adjacentes em supercanais.

Para formular o problema, primeiramente modela-se a rede óptica como um grafo G = (V, E), onde V é o conjunto de nós (comutadores ópticos BV-OXCs / ROADMs baseados em Bandwidth-Variable Wavelength Selective Switches) e E é o conjunto de enlaces de fibra óptica. Cada enlace e pertencente a E dispõe de um conjunto ordenado de slots de frequência F = {1, 2, ..., F}.

Para que uma requisição r = {s, d, n}, a qual demanda n slots de frequência, seja estabelecida por caminho físico P(s,d) contido em E, o RSA exige que sejam estritamente cumpridas três restrições. A primeira delas é a restrição de contiguidade do espectro, na qual n slots alocados para uma mesma requisição em um enlace e devem ser estritamente adjacentes no domínio da frequência. Se o índice do slot inicial é "x", o bloco espectral ocupado deve ser o intervalo contínuo S(r) = {x, x+1, x+2, ..., x+n-1}.

A segunda restrição é a restrição de continuidade do espectro, isto é, na ausência de conversores de espectro nos nós intermediários, o lightpath deve ocupar exatamente os mesmos índices de slots S(r) em todos os enlaces "e" pertencentes a P(s,d) que compõem a rota selecionada do nó de origem ao destino.

A terceira restrição é a restrição de não-sobreposição, que consiste no fato de dois lightpaths distintos que dividam um ou mais enlaces físicos (P₁ ∩ P₂ ≠ ∅) não poderem usufruir do mesmo slot de frequência f nesses enlaces compartilhados simultaneamente.

Todavia, na prática, o número de slots n requisitado por uma demanda de taxa de bits B (em gigabits por segundo) depende do formato de modulação $m$ escolhido nos transponders de largura de banda variável (Bandwidth-Variable Transponders) e da distância física da rota P(s,d), dando origem ao problema RMLSA (Routing, Modulation Level, and Spectrum Allocation), cuja principal equação é esta:

n = ⌈B / (ηₘ · Δfₛₗₒₜ)⌉ + n_GB

Em que Δfₛₗₒₜ é a largura de banda de um slot de frequência, ηₘ é a eficiência espectral do formato de modulação em bits/s/Hz, que varia conforme o nível de dupla polarização, e n_GB é o número de slots de banda de guarda (Guard Band) inseridos entre conexões adjacentes para evitar interferência de canal adjacente (crosstalk) devido às limitações de filtragem dos WSSs.

O problema RSA é comprovadamente NP-difícil (reduz-se ao problema de coloração de grafos de intervalos somado ao fluxo de múltiplos produtos). A escolha da abordagem de solução depende do cenário de operação da rede. Por exemplo, no cenário do RSA Estático (ou seja, offline), a matriz de tráfego é conhecida anteriormente (por meio de planejamento de capacidade). Nesse cenário, o objetivo é minimizar o índice máximo de slot utilizado (Fmax​) ou minimizar o número de transponders e/ou fibras. Abordagens comuns para resolução são Programação Linear Inteira Mista (Integral Linear Programming / Mixed Integral Linear Programming), decomposição em colunas ou até mesmo a utilização de meta-heurísticas (e.g. algoritmos genéticos e Simulated Annealing).

Já no cenário de RSA Dinâmico (online), as requisições chegam e partem estocasticamente em tempo real. Nessa situação, o objetivo é minimizar a Probabilidade de Bloqueio (Block Probability) ou a Taxa de Bloqueio de Banda (Band Block Rate) com baixa latência de decisão. Abordagens comuns para resolução são decomposição em K-Caminhos Mínimos (KSP) [aliada a políticas de alocação (First-Fit, Exact-Fit)] e aprendizado por reforço profundo (Deep Reinforcement Learning). Nesse caso específico, o problema é resolvido em duas etapas sequenciais (ou conjuntas em lista de candidatos), que como a própria sigla RSA já indica, são os sub-problemas do roteamento e da alocação de espectro.

No subproblema de roteamento, uma técnica denominada Roteamento Fixo Alternativo é utilizada para calcular os K menores caminhos físicos (via algoritmo de Yen) entre (s, d) e testa a disponibilidade espectral em ordem crescente de distância (favorecendo modulações de maior ordem no RMLSA). Outra técnica, denominada Least Congested Path (LCP), seleciona o caminho com maior disponibilidade de slots livres contíguos ou menor taxa de ocupação média nos enlaces.

No subproblema da alocação de espectro, existem três técnicas principais para resolução.

First-Fit (FF): Percorre a grade de frequência a partir do índice f = 1 e aloca o primeiro bloco contíguo e contínuo de n slots livres. Compacta o tráfego na parte inferior do espectro, deixando blocos maiores livres no topo.

Exact-Fit (EF): Busca uma lacuna espectral livre cujo tamanho seja exatamente igual a n slots. Caso não exista, recorre ao First-Fit. Preserva blocos contíguos maiores para futuras demandas de alta capacidade.

Last-Fit / Random-Fit / First-Last-Fit: Particionam o espectro em zonas (ex.: requisições pequenas em índices baixos via First-Fit e requisições grandes em índices altos via Last-Fit) para reduzir a colisão entre perfis de tráfego distintos.

Todavia, a combinação das restrições de contiguidade e continuidade sob tráfego dinâmico (chegadas e partidas de demandas com larguras variadas n) provoca a fragmentação espectral, caracterizada pelo fato de o espectro disponível nos enlaces ficar pulverizado em pequenos blocos isolados de slots livres que não são adjacentes no mesmo enlace (fragmentação vertical/intralink) ou não estão alinhados nos mesmos índices ao longo dos enlaces de uma rota (fragmentação horizontal/interlink). Uma forma de mitigar o problema é ativamente reconfigurar lightpaths em operação (Hitless Defragmentation, como Push-Pull ou Make-Before-Break) para compactar conexões existentes sem interromper o tráfego, ou roteamento ciente de fragmentação (Fragmentation-Aware RSA).


## 4. Controle, Design e Gerenciamento da Rede

É necessário, no gerenciamento de Redes Ópticas Elásticas, mecanismos que sejam capazes de distribuir os recursos da rede de maneira eficiente, atendendo às diferentes demandas de transmissão.Nesse sentido, o sistema de controle deve selecionar rotas adequadas, determinar a quantidade de slots de frequência necessária para cada conexão e configurar os equipamentos ópticos, considerando os princípios físicos da fibra ótica, como explicado na seção anterior.

É importante notar que essas decisões não se limitam a disponibilidade de espectro, mas também consideram as limitações físicas do enlace e a qualidade do sinal. Além disso, estratégias de alocação e desfragmentação espectral podem ser utilizadas para reduzir o desperdício de recursos e permitir o estabelecimento de novas conexões.

Uma abordagem notável para esse gerenciamento é a Rede Definida por Software (SDN: Software-Defined Networking), que permite centralizar parte das decisões de controle em um controlador programável. Dessa forma, é possível acompanhar o estado dos enlaces, monitorar a utilização do espectro e automatizar a configuração dos equipamentos. O monitoramento contínuo também auxilia na identificação de falhas e na adaptação da rede a eventuais mudanças de tráfego.

Essa combinação de alocação eficiente, controle automatizado e posterior monitoramento da performance da rede são cruciais para melhorar a eficiência e confiabilidade das Redes Ópticas Elásticas.

## 5. Tópicos Avançados e Tendências

A evolução das Redes Ópticas Elásticas (EONs) concentra-se em superar o limite não-linear de Shannon das fibras monomodo padrão (SSMF) operando exclusivamente na Banda C (~4,8 THz). À medida que o tráfego de datacenters, 5G/6G e treinamentos distribuídos de IA exige enlaces de 800 Gbps, 1,6 Tbps e além, o problema clássico de RSA expande-se em quatro frentes principais: novas dimensões físicas (espectro multi-banda e espaço), modelagem exata da camada física (QoT e formatação de constelação), controle autônomo via aprendizado de máquina (DRL e GNNs) e desagregação aberta / coexistência quântica.

I. Redes ópticas multi-banda (MB-EONs) e o problema RMBSA

Enquanto a implantação de novos cabos de fibra óptica exige alto investimento de capital (CAPEX), as fibras ITU-T G.652.D já instaladas no mundo possuem uma janela de baixa atenuação que vai muito além da tradicional C-Band (1530–1565 nm). As MB-EONs (Multi-Band EONs) exploram toda a janela espectral da sílica, estendendo a transmissão para as bandas L (1565–1625 nm), S (1460–1530 nm), E (1360–1460 nm) e O (1260–1360 nm), multiplicando a largura de banda utilizável de ~4,8 THz para mais de 50 THz.

<img width="600" height="286" alt="images" src="https://github.com/user-attachments/assets/49a5da17-d9dd-490a-8ca3-e258ade8413c" />

A imagem acima demonstra o perfil de bandas espectrais O, E, S, C e L em fibras ITU-T G.652. Como ilustrado no perfil de atenuação (Attenuation [dB/km] vs. Wavelength [nm]) acima, a eliminação do pico de absorção de hidroxila (Water peak na E-Band, em torno de 1383 nm) nas fibras modernas ITU-T G.652 Fiber (linha tracejada azul) viabiliza o uso contínuo desde a O-Band até a L-Band. Contudo, o problema de alocação evolui de RSA para RMBSA (Routing, Modulation, Band, and Spectrum Allocation), introduzindo desafios físicos severos. Dentre esses desafios, está o espalhamento Raman estimulado intercanal (ISRS — Inter-channel Stimulated Raman Scattering). Consiste no fato de que, em transmissões de banda ultralarga (e. g. S-Band + C-Band + L-Band), o efeito não linear de Raman transfere potência óptica das frequências mais altas (comprimentos de onda menores, como S-Band e C-Band) para as frequências mais baixas (L-Band), com pico de ganho em deslocamentos de ~13,2 THz. Isso cria uma inclinação espectral (spectral tilt) dinâmica: alocar um novo lightpath na Banda S altera instantaneamente a relação sinal-ruído (SNR) de conexões já ativas nas Bandas C e L.

Outro entrave físico é o fato de amplificadores a fibra dopada com Érbio (EDFAs) operarem apenas nas bandas C e L. A expansão multi-banda exige arquiteturas híbridas com amplificadores dopados com Túlio (TDFAs para a S-Band), Bismuto (BDFAs para E-Band e O-Band), amplificadores ópticos a semicondutor (SOAs) ou amplificação Raman distribuída, cada qual com figura de ruído (NF) e ganho distintos.

II. Multiplexação por divisão espacial (SDM-EONs) e o problema RMSCA

Para escalar a capacidade em uma ou duas ordens de grandeza por fibra, as SDM-EONs (Space-Division Multiplexing EONs) adicionam a dimensão espacial ao domínio tempo-frequência por meio de três tecnologias de fibra, que são as fibras de múltiplos núcleos fracamente acoplados (Weakly-Coupled Multi-Core Fibers — WC-MCF), que consistem em múltiplos núcleos monomodo independentes (tipicamente 4, 7, 12 ou 19 núcleos) compartilhando a mesma casca (cladding); fibras de poucos modos (Few-Mode Fibers — FMF) e MCFs fortemente acopladas, as quais realizam transmissão simultânea em múltiplos modos de propagação espacial (LP01, LP11A, LP11B... ), exigindo processamento digital de sinais MIMO (Multiple-Input Multiple-Output) no receptor coerente para desacoplar os modos; e feixes de fibras monomodo (SMF Bundles), as quais consistem em múltiplas fibras paralelas com chaveamento conjunto nos ROADMs.

Nas SDM-EONs, o RSA transforma-se no problema RMSCA (Routing, Modulation, Spectrum, and Core/Space Allocation). Além da contiguidade e continuidade espectral (e continuidade de núcleo, caso não haja comutadores espaciais de conversão de núcleo), surge um novo fenômeno limitante nas WC-MCFs, que é a diafonia entre núcleos (Inter-Core Crosstalk — IC-XT). Quando dois núcleos fisicamente adjacentes dentro da mesma fibra utilizam os mesmos slots de frequência simultaneamente no mesmo enlace, o acoplamento de campo evanescente vaza potência entre os núcleos. A principal consequência disso é que, em vez de alocar n slots contíguos em um único núcleo (Supercanal Espectral), um Supercanal Espacial aloca o mesmo índice de slot f(i) através de K núcleos simultaneamente. Isso reduz drasticamente o custo e a complexidade dos nós ópticos (BV-OXCs), pois um único filtro WSS pode comutar todos os núcleos de uma só vez (Joint Switching), ao custo de impor alinhamento espectral entre os núcleos. 

III. Controle e Segurança

A arquitetura de controle e segurança das EONs também passa por três transformações estruturais, dentre elas:

Desagregação óptica e controle SDN aberto: 

Consiste na substituição de sistemas monolíticos proprietários (vendor lock-in) por redes ópticas abertas e desagregadas (iniciativas OpenROADM, Telecom Infra Project — OOPT e ONF). Controladores SDN (como ONOS e TeraFlowSDN) configuram transponders e ROADMs multimarcas via modelos de dados padronizados YANG / NETCONF / RESTCONF e coletam telemetria contínua via gNMI/gRPC, utilizando motores físicos abertos como o GNPy para alimentar um Gêmeo Digital Óptico (Optical Digital Twin).

Coexistência de distribuição quântica de chaves (QKD) em EONs: 

Para proteger a infraestrutura crítica contra computadores quânticos, canais quânticos (DV-QKD ou CV-QKD, transmitindo fótons únicos) precisam compartilhar a mesma fibra óptica com supercanais clássicos de alta potência. O algoritmo de RSA precisa alocar o espectro clássico de modo a minimizar o ruído de Espalhamento Raman Espontâneo (SpRS) e Mistura de Quatro Ondas (FWM) que cai sobre a janela espectral do canal quântico (frequentemente alocando o canal quântico na O-Band e os dados clássicos na C-Band, ou isolando-os em núcleos dedicados em fibras MCF).

Sobrevivência avançada e "degradação graciosa" (Squeezed Protection): 

Em cenários de desastres ou cortes múltiplos de fibra, o uso de BVTs elásticos com PCS permite que rotas de proteção compartilhada (SBPP — Shared Backup Path Protection) operem com largura espectral ou taxa de bits temporariamente comprimida (spectrum squeezing), garantindo conectividade crítica mesmo sob escassez severa de slots.



## 6. Conclusões

Em conclusão, as Redes Ópticas Elásticas representam uma evolução importante das redes de comunicação por fibra óptica, pois permitem adaptar a utilização do espectro às necessidades de cada conexão. Por meio de tecnologias como OFDM e transponders de largura de banda variável, essas redes oferecem maior flexibilidade na distribuição dos recursos e possibilitam um aproveitamento mais eficiente da capacidade disponível em comparação com as arquiteturas tradicionais DWDM.

No entanto, é evidente que essa flexibilidade também introduz desafios de planejamento e operação que vêm exigindo soluções cada vez mais sofisticadas. De fato, a otimização do uso da rede e o aprimoramento de sua confiabilidade são aspectos cruciais para essa tecnologia frente às crescentes demandas de comunicação por parte da sociedade.

De forma concreta, vale destacar que o problema de roteamento e alocação de espectro exige a escolha de caminhos adequados e a reserva de slots que respeitem as restrições da rede, enquanto a fragmentação espectral pode dificultar o estabelecimento de novas conexões. Nesse contexto, algoritmos de otimização, mecanismos de controle automatizado e técnicas de monitoramento são fundamentais para melhorar o desempenho e a utilização dos recursos.

As tendências apresentadas na seção 5 demonstram que o desenvolvimento das EONs vai além da melhoria da eficiência espectral. A expansão para múltiplas bandas e dimensões espaciais, a aplicação de aprendizado de máquina e o uso de arquiteturas de controle abertas proporcionam novas possibilidades para ampliar a capacidade e a adaptabilidade das redes. 

Portanto, as Redes Ópticas Elásticas constituem uma tecnologia promissora para atender à crescente demanda por comunicação de alta capacidade. Seu futuro e evolução depende da solução de desafios como a complexidade de controle, as limitações físicas e a confiabilidade da infraestrutura.

---

## 7. Referências
