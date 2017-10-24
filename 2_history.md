# Usando a Historia

No começo desse tutorial foi dito que: "(o Git) te permite salvar versões antigas de um conjunto de arquivos e retornar a essas versões a qualquer momento, além de entender quais foram as mudanças entre cada versão e como elas aconteceram."

Até agora só criamos commits, ou seja, construimos a história. Está na hora de olhar a história que criamos.

## Log

Para listar todos os commits que você fez, é só executar ``git log``:

```
```
Ele mostrará todos os commits, junto ao indentificador único deles e a mensagem que explica a mudança. Com muitos commits, fica bem dificil entender o que aconteceu de forma geral. Para melhorar o ``git log``, podemos usar a opção ``--oneline``:

```
```
Assim, temos uma versão mais compacta. Note que somente as primeiras linhas da mensagem de commit são mostradas. É por esse motivo que é considerado uma boa prática a primeira linha de cada commit ser um resumo do que o commit está mudando e maiores detalhes serem dados nas próximas linhas da mensagem.

Um ponto interessante são os indentificadores únicos(id) de cada commit. O ultimo commit que o ``git log`` mostrou possui id ``xxx``. O id é uma sequencia de 40 caracteres, que na prática representam um hash SHA-1. O motivo de ser um hash, e não uma sequencia numérica, faz mais sentido de ser discutido na explicação sobre como Git lida com a colaboração entre pessoas. Mas vale adiantar por curiosidade que esses ids **NÃO** são necessáriamente únicos em um repositório, embora a chance que isso aconteça é extremamente pequena.

No ``git log --oneline``, vemos uma versão do id com menos caracteres: ``x``. É só uma questão estética. Na prática, só com os primeiros caracteres já é possível indentificar unicamente todos os commits de um repositório e por isso só são mostrados tantos caracteres quanto necessários para isso.

Por serem hashes, as pessoas se referem aos ids como tal, ao invés de "ids". Por isso será assim que esse tutorial vai se referenciar a eles. Logo, podemos dizer que cada commit é indentificado por um hash.

As referencias a "HEAD" e "master" que aparecem no ``git log --oneline`` serão faladas mais a frente quando estivermos discutindo referencias no Git.

## Entendendo as mudanças
A partir do hash do commit, podemos descobrir maiores informações sobre ele. Basta executar ``git show hash`` e vc terá as informações sobre as mudanças ocorridas em um commit, quem foi seu autor e a mensagem que foi deixada para descrever as mudanças.

```
```
Outra forma de entender as mudanças que aconteceram ao longo do tempo é usando o ``git diff``. Basta executar ``git diff hash1 hash2`` e o git mostrará as diferenças que existem entre os commits indentificados por esses hashs.

## Recuperando versões anteriores de arquivos
Se for necessário voltar um ou mais commits, devemos usar o comando ``git revert``. Usando o comando ``git revert`` e passando o nome do ultimo commit podemos ver que voltamos ao estado anterior:

```
```
Se continuarmos a fazer isso, voltamos cada vez mais um passo para trás. Porém, o git não está destruindo seus commits. Ao contrário: ele está criando novos commits que contém as modificações que voce fez de forma reversa:

```
```
Caso voce queira reverter mais de um commit, basta executar git reset hash1..hash2 e o git irá reverter todos os commits entre o commit hash1 e o hash2, criando commits opostos em ordem reversa. Se queremos reverter os ultimos dois commits, podemos executar os seguintes comandos:

```
```
Utilizamos o ``git log`` para conseguir os hashes e depois executamos ``git revert``. Note que o hash1 é o commit anterior ao ultimo cujas alterações queremos desfazer. 

Um problema é que, caso voce reverta 50 commits, serão criados 50 novos commits revertendo as alterações e isso não é bom. Para melhorar isso, podemos usar a flag ``--no-commit`` no ``git revert``:
```
```
Como podemos ver, esse comando irá reverter tudo mas não criará um novo commit. Ele na verdade deixará todas as alterações necessárias para reverter o código prontas para serem commitadas. Basta então usar: 

```
$ git commit -m "Sua mensagem"
```
Um ponto importante: o comando ``git revert``, quando usado com o hash de um commit, não necessita que ele seja o commit imediatamente anterior. Por exemplo, podemos tentar reverter o primeiro commit mesmo estando no ultimo. Nesse caso o git irá reverter somente as mudanças do commit escolhido. Isso as vezes gerará um conflito, pois o Git não saberá como reverter corretamente. Imagine reverter uma mudança em um arquivo que não existe mais. Falaremos sobre conflitos mas a frente. Caso se encontre em um caso assim, basta executar ``git revert --abort`` para cancelar a reversão. Depois, é possível usar ``git show`` e ``git diff`` para tentar fazer a reversão na mão.

## Voltando no tempo
Para voltar a uma versão anterior do arquivo, podemos usar o comando ``git checkout``. Basta executar ``git checkout hash`` e os arquivos irão retornar para a versão em que eles estavam depois do commit hash1 ter sido realizado. Para voltar ao normal, é só executar ``git checkout master``. 

O comando ``git checkout`` não é só usado com esse motivo. Na verdade, ele está mais relacionado à refencias do Git. Vamos falar delas mais adiante.
