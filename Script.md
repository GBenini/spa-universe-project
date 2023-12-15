// Tirar o padrao de quando clicar no botao a pagina redirecionar, criamos uma funcao chamada de route(), ou seja ela vai rotear, trabalhar com a rota, essa funcao vai receber o evento (event), o evento é o click, entao colocamos onclick em todos os "a" do meu nav.

// E dentro do onclick do html colocamos qual a funcao queremos que ele rode, que é a route(), nao preciso passar argumentos para ela, automaticamente ja existe argumentos sendo passados ali, o argumento sendo passado é o event.

//O que queremos que ele faca nesse evento é muito simples, primeiro verificamos se foi passado um evento, se nao passou pega o evento que esta na janela

// Nesse evento, podemos utilizar uma das funcionalidades, que é o preventDefault(), que vai fazer com que ela perca o seu padrao, e o nosso padrao do click em qualquer link a que tenha href, o padrao dele é redirecionar. Isso serve para todos os eventos que existem na DOM.



function route(event) {
    event = event || window.event
    event.preventDefault()

    window.history.pushState({}, "", event.target.href)

    handle()
}

// Agora vamos fazer o mapeamento da rota, para achar a pagina que queremos, para isso vamos criar uma constante chamada de routes, ela é um objeto, e nesse objeto eu vou colocar como o nome da propriedade, o nome da rota, e como valor para ela colocamos a rota até nosso arquivo.

// Colocamos o nome das propriedades com "" porque nao podemos criar o nome da propriedade com / ex: /universe, entao colocamos "/universe"

//Se quisermos acessar essa propriedade depois, nao conseguimos acessar com console.log(routes.universe), precisamos acessar ela com console.log(routes["/universe"])

const routes = {
    "/": "/pages/home.html",
    "/universe": "/pages/universe.html",
    "/exploration": "/pages/exploration.html",
    404: "/pages/notfound.html"
}

//vamos criar outra funcao, chamada handle(), essa funcao onde handle significar manipular, ela vai funcionar da seguinte forma, vamos criar uma const pathname que vai puxar o nome que vem depois da / no link.

// atraves do location.pathname eu vou ter acesso ao meu /universe, se eu tiver mapeado o /universe, eu vou conseguir descobrir qual é a pagina dele

// quando eu clicar em rout() em algum momento temos que fazer o handle(), entao ja vou deixar a funcao handle() dentro do route(), porque sempre que clicarmos, ele vai executar o route(), entrou no route(), fez todo o preventDefault, ai vai fazer o handle().

// Mas ainda falta fazermos algo para ele entender que nossa rota esta mudando, em route(), vamos entrar no window e acessar o history dele, que muda toda vez que mudamos de pagina ou clicamos em um href, entao pegamos o pushState() do historico *pushState é state de estado e push de empurre/coloque, ou seja coloque um estado no historico* , a primeira parte é o tipo de dado que queremos adicionar, como nao temos nenhum dado deixamos {}, unused tambem nao temos nada para colocar, entao colocamos como vazio porque ele é uma string "", e o terceiro é a url que quero colocar no meu historico que esta no meu event.target.href *o nosso evento que é o click tem muitas funcionalidades, uma delas é quem disparou esse evento, que seria o target ou seja nosso alvo, e do target queremos pagar nosso href, porque o target que disparou o evento é nosso a, e o a, tem a propriedade href. Entao quero pegar o href e colocar no meu historico.

//Primeiro tinhamos desabilitado a mudanca automatica da pagina, mas tambem nao estavamos mais avisando para onde estamovamos indo, e todo link ficava na mesma pagina, e atraves do pushState, estamos avisando para ele colocar no historico que estamos sim mudando de pagina, mas fique por enquanto apenas no historico, nao mudamos nada ainda.

// Porem temos outra maneira de pegar o pathname sem ser essa: const pathname = window.location.pathname, podemos desestruturar isso: const { pathname } = window.location, isso é muito simples, dentro do location, temos varias propriedades e funcionalidades, uma delas é o pathname, oque estamos dizendo nessa instrucao dentro das chaves é  para ir dentro do location, encontrar o pathname e coloque ele fora como uma constante, o nome disso é DESESTRUTURAR, estamos tirando de dentro do objeto, ou seja estamos desestruturando o location, para pegar apenas o pathname dele

// Esse novo modo de pegar o pathname é bom para pegar junto outras propriedades como href, host, port. Se tivermos que tirar por exemplo 20 itens, desestruturar melhora muito o codigo.

function handle() {
    const { pathname } = window.location
    const route = this.routes[pathname] || this.routes[404]

    fetch(route)
    .then(data => data.text())
    .then(html => {
        document.querySelector('#app').innerHTML = html
    })

}


// Agora vamos fazer a continuacao do handle, ja temos o mapeamento de rota e de pagina, que esta dentro do objeto routes, do routes podemos pegar o nome que queremos, ai colocamos a rota do pathname, ai se tivermos aquela rota queremos que ele mostre OU se nao tiver caso o usuario coloque / qualquer coisa no link, ai queremos o 404.

// Agora temos que pegar o HTML de cada pages, para quando clicar conseguir ver o arquivo, como pegamos aquele pages algum arquivo e jogar ele em um variavel: Conseguimos usar a ideia do assincronismo no JS a ideia de promessas, essa ideia de promises funciona assim, para eu acessar qualquer arquivo de fora desse nosso arquivo, qualquer informacao, seja css, js, isso sempre leva um tempo, porem um tempo curto, mas o JS nao pode bloquear minha aplicacao esperando esse tempo, porem nao pode bloquear a tela do usuario, entao o JS responde para nos o seguinte, ao clicar na nova page, ele te promete que vai trazer o arquivo que voce pediu, porem quando ele terminar a leitura dele, voce vai receber ele, antes de terminar a leitura dele, voce pode fazer qualquer coisa na aplicacao, nao vai travar a janela, porem isso quebra o padrao do JS de ler linha a linha.

// O que for assincrono ele vai ler a linha mas tirar do meio do caminho e depois de ler o arquivo "voltar ele pro jogo"

// com isso temos o fetch(), é uma API do html, pois é um conjunto de regras e funcionalidades, se pegarmos por ex: fetch('https://google.com') o fetch sempre retorna uma promise, ou seja Promise é algo assincrono, significa que ele vai "sair do jogo com essa linha" e a promessa é queremos que ele pegue a rota (route). 

//Porem que momento que a Promise "volta pro jogo?", o momento ele continua com o .then(), esse then do ingles "então" é assim, fetch no ingles é "vai buscar", entao estamos falando: prometo pra voce que vou buscar essa rota, Entao Prometo pra voce que quando eu buscar vou executar essa funcao aqui. then(() => {})

//.then() tambem é uma funcao callback, que é uma funcao passada como parametro para outra funcao que vai executar depois.

//Assim que eu prometer, vou voltar com os dados dessa funcao, "data" = dados, e nessa parte do then podemos retornar na funcao direto o data.text(), essa funcionalidade text, em cima do nosso tipo de dado, vai transformar os nossos dados em texto, porem estmaos retornando isso para onde?? se nao entender essa estrutura: .then(data => data.text) é a mesma coisa que fazer .then(function (data) { return data.text()}) so que de uma maneira curta. Para onde esse then esta retornando afinal? Ela continua em uma cadeia de .then(), no proximo then podemos chamar de html.

//Ate agora em nosso console ja conseguimos pegar outra pagina do nosso html atraves do fetch, ate o momento esse é o principio de pegar dados quando nao estao no front-end, nesse caso estamos pegando do front-end mas em uma SPA geralmente ele vai pegar os dados devolvidos como JSON, la do servidor, para o front-end, essa comunicacao geralmente é assim, assincrona e usando algo tipo o fetch, mesmo usando em exemplos simples ate agora, é ele o poder de linkar nosso front end com o beckend o nosso servidor.

// Voltando, conseguimos pegar nosso html e agora precisamos continuar essa aplicacao dentro desse ultimo then,  
