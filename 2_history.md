# Usando a Historia

No começo desse tutorial eu disse que criamos versões dos arquivos de mode que:
> você possa recuperar versões antigas e entender quais foram as mudanças que aconteceram entre essas versões.

Até agora só criamos commits. Está na hora de olhar o conjunto de commits que criamos, ou seja, a história de nossos arquivos.

## Realizando mudanças

Depois que o git começa a rastrear o arquivo, qualquer alteração que o git realiza que você realizar poderá ser usada para salvar uma nova versão do arquivo. 

Como o nosso hello world está muito desanimado, vamos adicionar umas exclamações.

Agora ao executar git status vemos que o git detectou as alterações. O arquivo vai aparecer como um arquivo modificaçã || o git diz que o arquivo está "not staged for commit". Se realizarmos git commit, podemos ver que o git dirá que não existem alterações. Isso é porque também é preciso dizer ao git quais alterações que vc quer que componham a nova versão do arquivo. Para isso, utilizamos o git add assim como foi para adicionar o arquivo.

Agora o git mostra essas mudanças como "changes to be commited", que significa que elas serão sim adicionadas em um novo commit. git commit -m ".." e um novo commit será criado.

Com isso, já temos duas versões do arquivo: a original, da criação dele, e a nova versão alterada. Podemos criar novas versões repetindo os mesmos passos. Tente criar novas versões com modificações suas.

Dica: Se vc quer comittar com todas as suas alterações, é possível passar a flag -a no git commit para adicionar todas as mudanças para commit e commitar de uma vez só.

Agora que vc possui várias versões do arquivo, vamos mostrar como fazer várias operações básicas:

Esse tutorial não tem como objetivo fazer vc usar git no dia a dia sem saber o que está fazendo, o que é muito comum, como essa tirinha diz: (XKCD)

a ideia também não é ensinar toda e qualquer opção disponível. A questão é entender como funciona e saber o que está fazendo. E com sorte, como consertar quando vc fizer algo errado.


* listar todas as versões
Para listar todos os commits que você fez, é só executar git log. Ele mostrará todos os commits, junto ao indentificador único deles e a mesnagem que explica a mudança. Com muitos commits, fica bem dificil entender o que aconteceu de forma geral. Para melhor o git log, podemos usar a opção git log --oneline. Assim, temos uma versão mais compcta. Note que somente as primeiras linhas da mensagem de commit são mostradas. É por esse motivo que a primeira linha de cada commit tem que ser um resumo (boa prática) do que o commit está mudando e maiores detalhes devem ser dados nas proximas linhas da mensagem. Esse comando também mostra os hashes de commit com menos caracteres, o que facilita a digitação.

* vendo o que mudou em cada versão e as informações de quem fez a mudança
A partir do indentificador unico do commit (que vamos chamar de hash), podemos descobrir maiores informações sobre eles. Basta executar git show hash e vc terá as informações sobre as mudanças e sobre quem fez e a mesnagem de descrição.
Outra forma de ver isso é usando git diff. Basta executar git diff hash1 hash2 e o git mostrará as diferenças que existem entre o commit hash1 e o commit hash2.

* reverter para essa versão
Se for necessário voltar um ou mais commits, devemos usar o comando git reset. Usando o comando git revert e passando o nome do ultimo commit podemos ver que voltamos ao estado anterior. Se continuarmos a fazer isso, voltamos mas um passo para trás. Porém, o git não está destruindo seus commits. Ao contrário: ele está criando novos commits que contém as modificações que voce fez de forma reversa.
Caso voce queira reverter mais de um commit, basta executar git reset hash1..hash2 e o git irá reverter todos os commits, criando commits opostos em ordem reversa. Se queremos reverter os ultimos dois commits, precisamos executar git log --oneline para conseguir os hashs e depois execute git revert hash1..hash2. Note que o hash1 é o commit anterior ao ultimo que cujas alterações voce quer ver desfeitas. Outra melhora é não criar tantos commits no momento de reverter. Caso voce queira reverter 50 commits, vai ter que criar novos 50 commits reversos e isso não é bom. Para melhorar isso, podemos usar o comando git revert --no-commit hash1..hash2. Esse comando irá reverter tudo mas não criará um novo commit. Ele na verdade deixará todas as alterações prontas para serem commitadas. Basta então usar git commit -m "Sua mensagem".

Um ponto importante: o comando git revert, quando usado com o hash de um commit, não necessita que ele seja o commit imediatamente anterior. Por exemplo, podemos tentar reverter o primeiro commit. Nesse caso o git irá reverter somente as mudanças do commit escolhido. Isso as vezes não vai ser óbvio, para o git, em como fazer. Nesse caso, ele avisará de um conflito. Falaremos sobre conflitos mas a frente. Por equanto basta executar git revert --abort para cancelar as mudanças.

* navegar para uma versão anterior
Para voltar a uma versão anterior do arquivo, podemos usar o comando git checkout. Basta executar git checkout hash e vc irá ver que o arquivo irá retornar para a versão em que ele estava depois do commit hash1 ter sido realizado.
É interessante notar que se vc executar git log, somente os commits anteriores ao atual serão mostrados. Mas isso não importa muito. basta usar a flag --all para ver todos. Mas afinal, o que é essa tal de HEAD e master que o git log mostra? * começar a falar de branches aqui? *
checkout para arquivo que ainda não existe.
