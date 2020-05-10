# Sumário
* [Intrdução](#introducao)
* [Layout](#layout)
  * Design
  * [Módulos](#scripting)
* [Scripting](#scripting)
  * [Elementos dinâmicos](#elementos-dinâmicos)
* Configuração
* Restrições
* Publicando
  * Licença
  * Controle de versão
  * Considerações finais
  
## Introdução
Temas são o que são as lojas, além de sistemas que ficam por baixo dos panos, claro. 
A identidade da loja é criada através da visão e da perspectiva que o cliente tem ao visualiza-la.

Uma identidade visual é a coisa mais importante que se pode definir e que majoritariamente deve ser prioridade ao longo do tempo.
Além da sua logomarca (desenho, simbolo ou ícone) e logotipo (nome da loja) sua loja online é o ponto de encontro mais seguro
entre seu cliente e seus produtos.

Uma loja online que ofereça tudo o que um cliente pode querer: facilidade, fluidez e suporte rápido é essencial para que se construa boas relações longas e duradouras.\
Então, vamos lá, nós precisamos de: fluidez e facilidade — isso que seu tema deve conter e nós o ajudaremos a ter isso.
  
### Layout
O layout do seu tema é o fator determinante numa loja, 
um layout fluido e bem construído é essencial para que o cliente que está navegando nela consiga fazer o que ele deseja
e encontre o que ele quer com facilidade e rapidez.

Constantamente existem lojas online em que o cliente entra e percebe o layout desconfortante e nada fluido de uma loja,
inclusive há situações que o cliente se frusta por não achar o que quer e desiste da compra, procurando em outro lugar.

A localização de cada elemento na página, a funcionalidade é o que devemos ter em mente.

### Módulos
Módulos são distintos sejam eles estáticos ou que variam constantamente em um layout, como:
um menu de navegação, um rodapé, um seletor de filtro de produto (que varia ao ser selecionado pelo cliente), entre outros.

Módulos contêm informações que podem ser adicionadas ou removidas a qualquer momento que você desejar durante a construção do seu tema.
Você os pega e os coloca no seu layout, lembrando sempre do que dissemos antes: "fluidez e facilidade — isso que seu tema deve conter".

Saber onde você irá colocar o método é o que importa, por exemplo, você não pode simplesmente colocar um rodapé no topo da página
ou então, o corpo do seu layout acima do menu de navegação. Isso são coisas básicas que você perceberá que não funcionarão com o tempo.

Existem vários tipos de módulos diferentes que você pode explorar para obter o objetivo final que será o seu tema, independentemente do que for baseado ele.
Se o seu módulo conter informações extras específicas para um tipo de loja lembre-se de adicionar um aviso sobre como utilizar isso, para que nenhuma
pessoa desavisada simplesmente coloque-o e espere que funcione do jeito que ela quer.

## Scripting

### Elementos Dinâmicos
Como dissemos em [Módulos](#módulos) existem informações que **variam constantemente em um layout** são eles: os elementos dinâmicos.
Seja o horário atual, alguma informação externa, um contador de visitantes online, qualquer coisa que varia se torna um elemento deste tipo.

Agora a questão é: como eu crio um elemento dinâmico.
Para isso, nós disponibilizamos um [SDK (Kit de Desenvolvimento de Software)](https://github.com/hs-org) para que você tenha as informações necessárias para que aquele seu elemento dinâmico funcione. Vamos aos exemplos:

Nós iremos agora criar um contador de visitantes online para nossa loja,
não sabemos qual loja é, não sabemos absolutamente nada, temos somente as informações dadas pelo nosso SDK.

Vamos nomea-lo de `contador`, e dentro do nosso script importamos nosso SDK para termos com o que trabalhar.
Nosso SDK nôs disponibiliza criar uma conexão com o servidor e **pedir e aguardar informações a/de ele** informações essas
relativas a loja do contexto atual (que está sendo utilizado o tema).
```ts
import HappyShop, {DynamicThemeModule, ServerConnection, ServerResponse} from '@hs-org/hs-theme-sdk'

/*
  essa variável representa nosso elemento atual.
  é com ela que nós atualizaremos o conteúdo que será exibido.
*/
const modulo: DynamicThemeModule = HappyShop.getCurrentModule();

/*
  aqui, nós criamos nossa conexão com o servidor,
  utilizaremos isso para trocar dados da loja atual com o mesmo.
*/
const servidor: ServerConnection = HappyShop.createServerConnection();

/*
  nessa parte nós iremos ouvir as mensagens do servidor.
  toda vez que um cliente novo entra em uma loja ele envia uma mensagem para o servidor, um oi.
  e o servidor responde para ele (bem gentilmente) e avisa para todos que estão ouvindo
  um novo cliente entrou, enviando uma mensagem "STORE_CLIENT_JOIN" para todos
 
  `count` é a contagem de visitantes na loja atualmente
*/
servidor.on("STORE_CLIENT_JOIN", (response: ServerResponse<{
  count: number
}>) => {
  /*
    nós recebemos uma mensagem do servidor,
    agora atualizamos nossas informações do módulo
    
    é importante que identifiquemos as informações dinâmicas do nosso módulo com uma chave,
    essa chave será utilizada aqui para atualizar os dados.
    
    atualizamos a variável "visitantes" com a contagem dada pelo servidor
    e ele será atualizado na loja.
  */
  module.update("visitantes", response.data.count);
});
```
