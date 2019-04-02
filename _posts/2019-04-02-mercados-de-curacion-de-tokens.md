---
layout: post
title: "Mercados de curación"
date: 2019-04-02 10:26:04 -0500
categories: crypto
---

Los mercados de curación son un conjunto de diseños de protocolos que
usan criptomonedas (tokens) como señales de valor en la curación de
información. De esta manera, los mercados de curación le permiten a
la gente coordinarse de nuevas formas alrededor de objetivos e intereses
comunes y beneficiarse del valor que crean colectivamente.

En los mercados de curación, las reglas para la creación de valor y
curación se escriben en un contrato inteligente que actúa como el **punto
schelling** para un objetivo o tema específico. Este contrato inteligente
permite la creación de tokens según sea necesario utilizando un **modelo
continuo** sin intervención de terceros. El costo de crear un token
aumenta a medida que el número de tokens aumenta y disminuye a medida
que el número de tokens disminuye.

Los tokens creados no tienen que representar una utilidad
específica más allá de su habilidad para representar la **red de valor** que
una comunidad con un interés común representa y su habilidad para
facilitar la coordinación, el crecimiento y la compartición de valor.

Los mercados de curación permiten crear tokens para cosas como
sub-reddits, hashtags, personas, memes y organizaciones nuevas. Todas
las unidades teóricas de información cultural (meme) pueden tener un
**efecto de red monetizable que permita la producción de  nueva
información durante períodos de tiempo sostenidos**.

Pueden existir muchos mercados de curación que usen diferentes
experimentos monetarios y de puntos schelling para ser más efectivos,
pero el núcleo es prácticamente el mismo: **se crean tokens de acuerdo con
un algoritmo compartido, estos tokens luego son usados para respaldar
información con el fin de agregar señales de valor y realizar curación**.

# Ventajas

  * Colaboración coordinada. Los proyectos donde los participantes están
  libremente organizados son difíciles de coordinar, por ejemplo un
  proyecto de código abierto o la lucha contra el calentamiento global.

  * Uso del conocimiento colectivo y los recursos comunes de forma
  efectiva. Hay una cantidad creciente de información que necesita ser
  curada.

  * Obteneción de valor de lo que co-creamos. Frecuentemente, los
  creadores de contenido no se benefician del valor que crean. Grandes
  corporaciones motivadas sólo por el lucro y que generalmente no
  representan o se preocupan por sus usuarios, han creado redes donde
  los usuarios suben información personal y curan contenido de forma
  gratuita para construir su estatus social y reputación. La red extrae
  valor vendiendo los datos de los usuarios a terceros.

  * Las redes pueden ser propiedad de sus usuarios y ser ellos quienes
  las regulan. Los participantes comparten el éxito de la red siendo
  recompensados económicamente.

# Mecanismo

Existen tres agentes participantes: productores de contenido,
consumidores de contenido y los poseedores de tokens. Los poseedores de
tokens moderan la plataforma y alientan a los productores de contenido a
que sigan produciendo contenido valioso, ya que perderían dinero si la
gente se retira del mercado y el token baja de precio.

Una vez publicado el contrato inteligente de un mercado de curación,
Alice o Bob pueden crear tokens depositando ether, por ejemplo, en el
contrato. Según el diseño, el contrato puede mantener el ether recibido
en un depósito comunal o puede enviar una porción a un beneficiario. En
cualquier momento, Alice o Bob pueden retirarse del mercado cambiando
sus tokens y tomando como retribución una porción del depósito comunal.
Cuando esto sucede, los tokens cambiados se retiran de circulación y el
precio de crear un token se reduce porque la cantidad de tokens en
circulación se redujo. Esta reducción en el precio permite que más
gente pueda comprar tokens y entre al mercado a producir valor.

Hay un margen de precio entre crear (comprar) un nuevo token y cambiar
(vender) el token. Si Alice compra tokens y luego otros agentes se unen
a ella y compran más tokens, Alice cubrirá los gastos cuando se retire
del mercado o incluso podrá recibir ganancias.

## Ejemplo

Una vez publicado el contrato inteligente de un mercado de curación:

Alice compra un token.

Estado del mercado:
Alice: 1 token
Tokens existentes: 1 token
Depósito comunal: 1 eth

El nuevo precio del token es 2 eth.

Bob compra un token.

Estado del mercado:
Alice: 1 token
Bob: 1 token
Tokens existentes: 2 token
Depósito comunal: 3 eth

El nuevo precio del token es 3 eth.

Carol compra un token.

Estado del mercado:
Alice: 1 token
Bob: 1 token
Carol: 1 token
Tokens existentes: 3 token
Depósito comunal: 6 eth

El nuevo precio del token es 4 eth.

Alice quiere tener más peso en el mercado de curación así que compra
otro token.

Estado del mercado:
Alice: 2 token
Bob: 1 token
Carol: 1 token
Tokens existentes: 4 token
Depósito comunal: 10 eth

Al tener tokens, esta comunidad puede enviar señales de valor y empezar
a curar información. Alice y Bob creen que ellos son los mejores curando
información así que se asignan sus tokens a ellos mismos. Carol prefiere
tener su token líquido y no lo usa para curación.

Alice y Bob respaldan el tema 1.

Alice respalda el tema 2.

Bob respalda el tema 3.

Curaduría:
tema 1: 100% votos
tema 2: 66% votos
tema 3: 33% votos
tema 4: 0% votos

# Curaduría

En un mercado de curación los participantes son incentivados a realizar
más y mejor curación ya que sus tokens aumentan de valor según el
desempeño del mercado, el cual es impulsado por el desempeño de los
curadores.

Para curar información, los tokens en el mercado deben ser asignados a
curadores específicos elegidos por los poseedores de tokens. Uno puede
asignarse sus tokens a uno mismo o a otra persona (sin perder los tokens)
basado en quién uno piense que hará o que ha estado haciendo el mejor
trabajo como curador, ya que un mejor curador atraerá más atención y por
lo tanto más valor. Estos depósitos de asignación se pueden cambiar en
cualquier momento, haciendo que el proceso de elección de curadores sea
dinámico.

En un mercado hay varios curadores agrupados por temas, y cada curador
tiene asociado un porcentaje de todos los tokens en ese tema, lo cual es
un indicador indirecto de su reputación. Cada curador puede señalar como
valiosa cualquier información al respaldarla con sus tokens y también
puede retirar su respaldo en cualquier momento.

# Beneficiarios

Generalmente, los mercados de curación funcionan sin beneficiarios, esto
reduce la complejidad al utilizarse sólo para coordinar un grupo de
personas alrededor de un tema en común. Sin embargo, los mercados de
curación también pueden ser utilizados para crear "Organizaciones de
curación". En algunas circunstancias un beneficiario independiente es
importante para el desarrollo del objetivo común. Este beneficiario
puede decidir si usar sus tokens para curar o venderlos para impulsar el
desarrollo del objetivo común. En otras ocasiones el poder del
beneficiario puede disolverse con el tiempo, cuando el beneficiario sólo
es importante para arrancar un ecosistema.

Un mercado de curación puede pagar directamente al beneficiario en el
proceso de creación de tokens codificando un protocolo que reparta los
tokens creados con el beneficiario. Por ejemplo, cuando se crean los
tokens, la persona que los compra recibe 10 tokens y el beneficiario 1.

Una alternativa distinta es pagar con Ether al beneficiario. En lugar de
repartir los tokens creados, el Ether se divide entre el depósito comunal
y el beneficiario (como si el beneficiario recibiera tokens y los
vendiera inmediatamente). Un beneficiario que recibe tokens está más
alineado con el éxito del mercado de curación pero agrega más
complejidad.

# Colateral

Un riesgo de los mercados de curación es que utilizan Ether como
colateral en el depósito comunal. Esto hace que los poseedores de tokens
también deban tener en cuenta el precio del Ether en el depósito. Podría
haber escenario de volatilidad, en los que es más ventajoso transar
Ether y por lo tanto hay un mayor incentivo en mantener el token líquido
en lugar de usarlo para curación. El uso de tokens estables (DAI) como
colateral produce mercados de curación más estables.

# Curvas

Principio: Se pueden crear tokens de forma automática basándose en una
curva de precio, pero el precio sólo se reducirá una vez un token sea
usado para algo. En cualquier momento, cualquiera puede vender sus
tokens en el depósito comunal y recibir una compensación apropiada
definida por la curva de precio.

Esta configuración permite que los mercados se formen y disuelvan según
su necesidad. Si todos se retiran, todo el Ether se reembolsa y todos
los tokens dejan de existir.

Los mercados de curación se caracterizan por medio de curvas de unión
(bonding curve) que relacionan el precio de un token con la cantidad de
tokens existentes. Estas curvas son algoritmos codificados en contratos
inteligentes. Pueden existir diferentes curvas para comprar y
vender tokens, esto depende de los objetivos del proyecto.

Ejemplo de una curva cuadrática:

![image alt](https://cdn-images-1.medium.com/max/1200/1*n8acuM7F7YmIKwZxpaZT4A.jpeg "cuadratic curve")

currentPrice = tokenSupply²

Estas curvas son susceptibles a ataques "front-running". Estos se dan
cuando un adversario se da cuenta que va a realizarse una gran compra y
envía primero su orden de compra con más gas para anticiparse a la
primera orden. Una vez la orden original se hace efectiva, el atacante
vende sus tokens con una ganancia garantizada.

Una solución puede ser limitar la cantidad de gas que las personas
pueden enviar con sus transacciones e incitar a las personas a que usen
la cantidad máxima permitida. Esto evita que un adversario ejecute una
orden de compra antes de las enviadas.

# Casos de uso

## Proyecto de código abierto

Un proyecto de código abierto tiene asociado un mercado de curación
con un token utilizado para la curaduría de nuevas funcionalidades.

## Curaduría de productos

Alice compra un nuevo producto y se da cuenta que es fantástico, en
lugar de escribir una reseña, compra unos tokens de ese producto. A
medida que más personas compran el mismo producto hacen como Alice y
compran tokens para indicar lo bueno que es. Luego, Alice vende sus
tokens de este producto y obtiene ganancias.

## Curación de sub-reddits

Comunidades basadas en temas pueden coordinarse para compartir
información importante para ellos. Si se realiza una curación efectiva,
esto atrae a nuevos participantes. Por lo tanto, una comunidad basada en
un tema que cura bien, puede obtener beneficios de forma colectiva por
hacerlo.

### ethtrader token

/r/ethtrader es un sub-reddit donde la gente discute estrategias de
compra y venta de Ethereum.

Este sub-reddit quiere tener un token que represente al grupo de
personas de la comunidad, y también quieren una mejor coordinación
alrededor de buenas estrategias de compra y venta. Al usar tokens para
enviar señales de valor sobre buenos análisis técnicos, toda la
comunidad se puede beneficiar.

# Ejemplos

## ZAP Store

Curación y acceso a oráculos. Cualquiera puede crear un oráculo en la
plataforma, pero para acceder a la información que este ofrece, los
participantes deben asociar tokens Zap al oráculo. Al asociar estos
tokens, se genera un token secundario llamado Dot que equivale a una
consulta al oráculo. El éxito y la confiabilidad de cualquier oráculo es
fuertemente determinada por el número de participantes que le asocian
tokens, ya sea para realizar una consulta de datos, o para especular
sobre su popularidad en el futuro.

## Meme Factory

Un juego creado por Disctrict0x donde los usuarios pueden crear memes y
asociar tokens a cada meme de acuerdo con su propia curva de unión.
En este juego los usuarios apuestan sobre la popularidad de cada meme.

## Ocean Protocol

Se señalizan con valor conjuntos de datos. Los tokens asociados son una
señal de la popularidad de los datos en el futuro. Un usuario recibe
tokens por poner a disposición de la red conjuntos de datos. Si el
usuario ha asociado tokens al conjunto de datos cuando lo ofrece, gana
más tokens. Si el conjunto de datos no está disponible cuando alguien lo
solicite, se pierden los tokens asociados al conjunto de datos.

## 1Hive

Es una propuesta de District0x donde un beneficiario tiene acceso al
depósito comunal (usando por ejemplo, calendarios de inversión basados
en contratos inteligentes). Esto es un nuevo tipo de financiamiento
colectivo que incorpora la curación de proyectos importantes y
necesarios, pero también recompensa a inversores por respaldar
diferentes ideas y proyectos.

# Referencias

  * http://tokenengineering.net/curation-markets
  * https://medium.com/@simondlr/introducing-curation-markets-trade-popularity-of-memes-information-with-code-70bf6fed9881
  * https://docs.google.com/document/d/1VNkBjjGhcZUV9CyC0ccWYbqeOoVKT2maqX0rK3yXB20/edit
  * https://media.consensys.net/a-practical-example-of-curation-markets-an-ethtrader-token-for-curating-good-market-analysis-6f6f340c6916
  * https://hackernoon.com/fanbase-content-curation-using-blockchain-tokenomics-1b2633d9dc11
  * https://news.cryptos.com/blockchain-spotlight-curation-markets-what-they-are-and-how-theyre-advancing-a-decentralized-future/
  * https://steemit.com/cryptocurrency/@whynotdoit/what-is-token-curated-market-new-market-using-blockchain-is-coming
  * https://steemit.com/crypto/@mohamedhayibor/ethereum-killer-app-curation-markets
  * https://blog.foam.space/foam-token-curated-registries-for-geographic-points-of-interest-60d3c043f183
  * https://blockonomi.com/token-curated-registries/
  * https://dailyhodl.com/2019/01/30/token-curated-registries-tcrs-introduction-to-a-popular-crypto-economic-primitive/
  * https://medium.com/@simondlr/tokens-2-0-curved-token-bonding-in-curation-markets-1764a2e0bee5
  * https://blog.relevant.community/how-to-make-bonding-curves-for-continuous-token-models-3784653f8b17
