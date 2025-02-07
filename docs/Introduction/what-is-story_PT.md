---
title: O que é Story
excerpt: Introdução à Story
deprecated: false
hidden: false
metadata:
  title: Story Documentation
  description: >-
    A documentação oficial do Story. Abrange a Story Network ("a Blockchain de Propriedade Intelectual do Mundo"), 
    nosso protocolo "Prova de Criatividade", 
    a Licença de Propriedade Intelectual Programável e muito mais.
  image: https://files.readme.io/2d35525-header_story.png
  robots: index
next:
  description: ''
---
<Image align="center" src="https://files.readme.io/3e11869-header_story.png" />

# Apresentando a Blockchain de Propriedade Intelectual do Mundo

O Story é um blockchain de camada 1 projetado especificamente para a gestão de propriedade intelectual. Ele tokeniza qualquer tipo de propriedade intelectual, seja uma ideia, uma imagem, um ativo do mundo real, uma música, um modelo de IA, um NFT, direitos de imagem ou qualquer outra coisa nesse espectro. Incorporando termos de uso, atribuição e acordos de royalties diretamente no blockchain, a Story oferece uma solução transparente e descentralizada para o gerenciamento de propriedade intelectual. Isso permite que os detentores de IP protejam suas criações, colaborem de forma integrada e desbloqueiem novas oportunidades de receita em uma economia impulsionada pela inteligência artificial.

> ⏩ Pule a Leitura - Resumo de 1 Minuto
>
> Quer ir direto ao resumo? Confira [Explain Like I'm Five](doc:explain-like-im-five_PT) para uma explicação super rápida e fácil de entender sobre tudo relacionado ao Story.

## O "Porquê"

Quando os proprietários de propriedade intelectual compartilham seu trabalho online, é fácil para outros usá-lo ou alterá-lo sem dar os devidos créditos, e muitas vezes eles não são pagos de forma justa se seu trabalho se tornar popular ou valioso. Isso pode ser desmotivador para quem deseja compartilhar suas ideias e criações, mas não quer perder o controle sobre elas.

Além disso, a velocidade e a superabundância de mídia gerada por IA estão ultrapassando o sistema atual de propriedade intelectual, que foi projetado para replicação física. Em muitos casos,  [ IA é treinada e está produzindo dados protegidos por direitos autorais.](https://twitter.com/BriannaWu/status/1823833723764084846).

A Story resolve isso oferecendo uma maneira para os criadores compartilharem seu trabalho com proteção embutida. Quando alguém (incluindo um modelo de IA) usa uma música, imagem ou qualquer obra criativa registrada na Story, o sistema rastreia automaticamente quem é o proprietário original e garante que ele receba os créditos. Além disso, se essa obra gerar receita—por exemplo, se alguém remixar uma música e ela gerar dinheiro—o proprietário original recebe automaticamente sua parte justa.

## O "Como"

Existem vários elementos que compõem a Story como um todo. Abaixo, abordaremos os componentes mais importantes: Story Network (a L1), o Protocolo de Prova de Criatividade (os contratos inteligentes) e a Licença de Propriedade Intelectual Programável.

![](https://files.readme.io/56f3c96-image.png)

### Story Network: "O Blockchain de Propriedade Intelectual do Mundo"

A Story Network é um blockchain de camada 1 projetado especificamente, alcançando o melhor do EVM e do Cosmos SDK. Ela é 100% compatível com o EVM, além de otimizações profundas na camada de execução para suportar estruturas de dados em gráfico, projetadas para lidar com estruturas de dados complexas como IP de forma rápida e econômica. Isso é feito por:

* Usando primitivas pré-compiladas para percorrer estruturas de dados complexas, como gráficos de IP, em questão de segundos e com custos marginais
* Uma camada de consenso baseada na stack madura do CometBFT para garantir finalidade rápida e transações baratas

### Protocolo de Prova de Criatividade

Nosso Protocolo de "Prova de Criatividade", composto por contratos inteligentes, é implantado nativamente na Story Network e permite que qualquer pessoa registre IP na Story. A maior parte da nossa documentação se concentra no protocolo.

Os criadores podem registrar sua propriedade intelectual como [🧩 Ativos de IP](doc:ip-asset_PT) em nosso protocolo. Os Ativos de IP (IPA) são os metadados programáveis de IP fundamentais no protocolo. Cada IPA é composto por:

1. um NFT na cadeia. Isso pode ser um NFT existente, como [Azuki](https://www.azuki.com/en), que em si é a propriedade intelectual, ou um novo NFT cunhado especificamente para representar uma propriedade intelectual fora da cadeia, como um ativo do mundo real.
2. sua conta de IP associada, que é uma implementação modificada do ERC-6551 (Token Bound Account).

Para interagir com os Ativos de IP dentro do protocolo, usamos "módulos" como os módulos de licenciamento, royalties e disputa. Por exemplo, o proprietário de um Ativo de IP pode definir **termos** sobre seu IP, como se obras derivadas podem ou não usar o IP comercialmente (e a que custo). Criadores de obras derivadas podem então expandir seu trabalho de forma integrada, cunhando "**Tokens de Licença**" – definidos pelos termos e também representados como NFTs – usando o [Módulo de Licenciamento](doc:licensing-module_PT), criar fluxos de receita com obras derivadas usando o [Módulo de Royalties](doc:royalty-module_PT) ou levantar uma disputa usando o [Módulo de Disputa](doc:dispute-module_PT).

### Licença de Propriedade Intelectual Programável

Embora na cadeia(on-chain), os termos de um IPA e os Tokens de Licença cunhados são aplicados por um contrato legal fora da cadeia chamado [Licença de Propriedade Intelectual Programável (PIL💊)](doc:programmable-ip-license-pil_PT), que permite que qualquer pessoa transfira a propriedade intelectual tokenizada para o sistema legal fora da cadeia e define os termos legais reais sobre como os criadores podem remixar, monetizar e criar derivados de sua propriedade intelectual. *O protocolo, ou mais especificamente os Ativos de IP e os módulos descritos acima, são o que automatizam e aplicam esses termos na cadeia(on-chain)*, criando um mapeamento entre o mundo legal (PIL) e o blockchain.

Assim como o USDC permite a conversão para moeda fiduciária, o PIL permite a conversão para propriedade intelectual.

## Um Exemplo

**Exemplo #1**: Imagine que você é um artista que cria pinturas digitais, ou um músico que faz músicas originais. Você quer compartilhar seu trabalho online, mas deseja garantir que, se outras pessoas usarem ou alterarem seu trabalho, elas te deem os devidos créditos e—se elas ganharem dinheiro com isso—você receba uma parte. É aí que a Story entra. Ela é uma plataforma que usa tecnologia para dar aos proprietários de IP, como você, controle sobre como seu trabalho é usado, rastreado e compartilhado, garantindo que ele seja protegido e recompensado de forma justa.

Pense assim: Suponha que você faça o upload de uma música na Story. Agora, qualquer pessoa pode ver que você é o criador original, e se alguém quiser fazer um remix, pode fazê-lo através da Story. O sistema então rastreia automaticamente o remix como um "derivado" da sua música e te registra como o artista original. Dessa forma, se o remix se tornar popular e gerar dinheiro, a Story pode te ajudar a ganhar uma parte desses lucros, assim como o remixer (pessoa que fez o derivado).

**Exemplo #2**: Vamos supor que um cientista faça o upload de um conjunto de imagens para ser usado por modelos de inteligência artificial (IA) em pesquisas. Através da Story, esse conjunto de dados é registrado, de forma que, se alguma empresa o usar para treinar sua IA, o cientista original receberá os créditos. Se esse conjunto de dados então contribuir para uma aplicação lucrativa de IA, a Story garante que uma parte justa seja destinada ao contribuidor original.

Com a Story, você pode compartilhar seu trabalho livremente, sabendo que, onde quer que ele vá, será rastreado e devidamente creditado a você. A ideia é criar um ambiente justo para compartilhar, construir e fazer crescer o trabalho criativo.
