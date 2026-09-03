O guia prático para nunca mais escolher o método errado em JavaScript
Você abre um projeto, encontra uma lista de usuários e precisa fazer alguma coisa com ela. Transformar os dados? Filtrar alguns itens? Encontrar uma pessoa específica? Aí aparecem três métodos muito comuns no JavaScript: map(), filter() e find(). Eles são parecidos. Mas resolvem problemas diferentes.

Antes de tudo: pense no resultado que você quer
Imagine este array:

const usuarios = [
{ id: 1, nome: "Ana", ativo: true },
{ id: 2, nome: "Carlos", ativo: false },
{ id: 3, nome: "Marina", ativo: true }
];
Agora esqueça os métodos por alguns segundos. Faça apenas uma pergunta: O que eu quero obter no final?

Se você quer transformar todos os itens, pense em map().
Se quer selecionar alguns itens, pense em filter().
Se quer encontrar apenas um item, pense em find().
Essa regra já resolve boa parte da confusão.

map(): quero transformar os dados
O map() percorre um array e cria um novo array a partir da transformação de cada item. Imagine que você recebeu objetos completos, mas precisa mostrar somente os nomes dos usuários.

const nomes = usuarios.map(usuario => {
return usuario.nome;
});

console.log(nomes);
Resultado:

["Ana", "Carlos", "Marina"]
O array original tinha três objetos. O novo array continua com três posições, mas agora cada objeto virou uma string. Essa é uma boa pista sobre o map(): ele transforma.

Podemos deixar o código ainda menor:

const nomes = usuarios.map(usuario => usuario.nome);
Outro exemplo: imagine um carrinho de compras.

const precos = [10, 20, 30];

const precosComDobro = precos.map(preco => preco * 2);

console.log(precosComDobro);
Resultado:

[20, 40, 60]
Cada valor entrou.
Cada valor foi transformado.
Cada valor produziu outro valor.
filter(): quero selecionar alguns itens
Agora o problema mudou. Você não quer transformar todos os usuários. Quer somente aqueles que estão ativos.

const usuariosAtivos = usuarios.filter(usuario => {
return usuario.ativo === true;
});

console.log(usuariosAtivos);
O resultado será:

[
{ id: 1, nome: "Ana", ativo: true },
{ id: 3, nome: "Marina", ativo: true }
]
O filter() faz uma pergunta para cada elemento: Este item deve continuar no novo array?

Se a função retornar true, ele entra.
Se retornar false, fica de fora.
Podemos simplificar:

const usuariosAtivos = usuarios.filter(
usuario => usuario.ativo
);
Essa lógica aparece o tempo inteiro em aplicações reais.

Filtrar produtos disponíveis:

const disponiveis = produtos.filter(
produto => produto.estoque > 0
);
Filtrar tarefas concluídas:

const concluidas = tarefas.filter(
tarefa => tarefa.concluida
);
Filtrar pessoas maiores de idade:

const adultos = pessoas.filter(
pessoa => pessoa.idade >= 18
);
A pergunta permanece a mesma: Quais elementos eu quero manter?

find(): quero encontrar uma coisa
Agora imagine que você recebeu um id pela URL:

/usuarios/2
Você não quer todos os usuários ativos. Também não quer transformar o array.

Você quer encontrar o usuário com id 2.

const usuario = usuarios.find(usuario => {
return usuario.id === 2;
});

console.log(usuario);
Resultado:

{
id: 2,
nome: "Carlos",
ativo: false
}
Perceba uma diferença importante.

filter() retorna um array.
find() retorna o primeiro elemento encontrado.
Compare:

const resultadoFilter = usuarios.filter(
usuario => usuario.id === 2
);

const resultadoFind = usuarios.find(
usuario => usuario.id === 2
);
Com filter(), o resultado continua sendo um array:

[
{
  id: 2,
  nome: "Carlos",
  ativo: false
}
]
Com find(), recebemos diretamente o objeto:

{
id: 2,
nome: "Carlos",
ativo: false
}
Parece uma diferença pequena. Até você tentar acessar o nome.

Com find():

console.log(resultadoFind.nome);
Com filter():

console.log(resultadoFilter[0].nome);
Se você espera apenas um resultado, find() normalmente representa melhor sua intenção. Código bom não é apenas código que funciona. É código que deixa claro o que você queria fazer.

O erro clássico: usar map() para tudo
É comum encontrar algo assim no código de quem está começando:

usuarios.map(usuario => {
if (usuario.ativo) {
  console.log(usuario.nome);
}
});
Funciona?

Sim. Mas o map() não existe simplesmente para percorrer um array. A função dele é produzir um novo array transformado. Nesse exemplo, o resultado do map() nem está sendo usado.

Se a intenção é selecionar usuários ativos, filter() expressa melhor isso:

const ativos = usuarios.filter(
usuario => usuario.ativo
);
Se a intenção é apenas executar uma ação para cada usuário, provavelmente forEach() representa melhor o que você quer:

usuarios.forEach(usuario => {
console.log(usuario.nome);
});
Os métodos até podem parecer intercambiáveis em alguns códigos. A intenção deles não é.

Dá para combinar os métodos?
Sim. E é aqui que eles começam a ficar realmente úteis. Imagine que você precisa pegar somente os usuários ativos e depois extrair seus nomes.

Primeiro filtramos:

const ativos = usuarios.filter(
usuario => usuario.ativo
);
Depois transformamos:

const nomes = ativos.map(
usuario => usuario.nome
);
Também podemos encadear as operações:

const nomes = usuarios
.filter(usuario => usuario.ativo)
.map(usuario => usuario.nome);
Leia esse código quase como uma frase:

pegue os usuários
filtre os ativos
transforme cada usuário em seu nome
Resultado:

["Ana", "Marina"]
Esse tipo de composição aparece bastante em front-end, back-end e APIs.

Você recebe dados.
Seleciona o que interessa.
Transforma no formato de que precisa.
Cuidado quando find() não encontra nada
Tem uma pegadinha importante.

Veja:

const usuario = usuarios.find(
usuario => usuario.id === 99
);
Não existe usuário com id 99.

Nesse caso, find() retorna:

undefined
Por isso, este código pode gerar um erro:

console.log(usuario.nome);
Uma opção é verificar antes:

if (usuario) {
console.log(usuario.nome);
}
Outra possibilidade é usar optional chaining quando fizer sentido:

console.log(usuario?.nome);
Esse detalhe parece pequeno. Mas é exatamente o tipo de coisa que aparece como bug quando começamos a trabalhar com dados reais.

Então qual eu uso?
Quando estiver olhando para um array, não tente lembrar primeiro da sintaxe.

Pergunte o que você quer fazer.

Quero transformar todos os itens?
→ map()

Quero selecionar vários itens?
→ filter()

Quero encontrar um único item?
→ find()
Vamos testar com produtos:

const produtos = [
{ id: 1, nome: "Teclado", preco: 200 },
{ id: 2, nome: "Mouse", preco: 80 },
{ id: 3, nome: "Monitor", preco: 1200 }
];
Quero apenas os nomes?

const nomes = produtos.map(
produto => produto.nome
);
Quero produtos acima de R$ 100?

const caros = produtos.filter(
produto => produto.preco > 100
);
Quero o produto com id 2?

const produto = produtos.find(
produto => produto.id === 2
);
Três perguntas.
Três métodos.
Três intenções diferentes.
O que vale decorar
Não decore map(), filter() e find() como três pedaços isolados de sintaxe. Decore as perguntas.

Quero transformar? map().
Quero selecionar? filter().
Quero encontrar? find().
Quando você aprende a pergunta certa, o método costuma aparecer sozinho. Esse é um salto importante para quem está começando a programar: deixar de escolher código porque “já viu alguém usando” e começar a escolher porque sabe exatamente o que precisa acontecer.

Continue aprendendo comigo
Se este artigo te ajudou, compartilhe com outro dev que ainda mistura map(), filter() e find(). E me acompanhe no LinkedIn para mais conteúdos sobre JavaScript, desenvolvimento fullstack e conceitos que fazem diferença no código do dia a dia.

#JavaScript #DesenvolvimentoWeb #Programacao

Referências
MDN Web Docs — Array.prototype.map()

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map

MDN Web Docs — Array.prototype.filter()

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

MDN Web Docs — Array.prototype.find()

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find
