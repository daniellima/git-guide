# Git Init

Se você chegou a esse tutorial provavelmente sabe que o Git é uma forma de versionar arquivos. Isso significa que ele te permite salvar versões antigas de um conjunto de arquivos e retornar a essas versões a qualquer momento, além de entender quais foram as mudanças entre cada versão e como elas aconteceram.

## Esse tutorial

Esse tutorial é para quem quer aprender sobre Git do zero ou para quem já sabe usar Git no dia a dia mas não se sente seguro usando a ferramenta, ou seja, não conhece muito bem os conceitos e queria entender melhor para usar todos os recursos que o Git oferece.

Lembrando que esse tutorial entra em detalhes, ensinando de forma mais completa e por isso não é adequado se você precisa usar Git o mais rápido possível. Ou seja, não é um "Quickstart". Para isso existem vários tutoriais na internet que vão te ensinar superficialmente os comandos.

A questão é que muitas vezes ficamos assim quando olhamos só superficialmente:

![XKCD sobre Git](https://imgs.xkcd.com/comics/git.png)

Não tem problema nenhum só saber superficialmente o Git, porque Git **é** uma ferramenta confusa. Claramente ser intuitivo para novos usuários não foi uma prioridade no seu desenvolvimento. Mas como Git é muito poderoso e se tornou central ao desenvolvimento de software atual, valhe a pena investir um tempo em aprender a usar todo o potencial da ferramenta.

## Terminal

Esse tutorial será dado usando como exemplos o Git via terminal. Porque não utilizar um cliente Git com interface gráfica?

* Porque interfaces gráficas geralmente simplificam funcionalidades para você ou criam suas próprias formas de funcionar. Isso significa que se você trocar de ferramenta, acabará tendo que aprender muita coisa novamente.
* Dificilmente uma interface gráfica te pferecerá acesso a todas as funcionalidades do Git.
* Procurar ajuda na internet geralmente te leva a pessoas dizendo que comandos executar, porque é muito mais fácil dizer o passo a passo via linha de comando.
* Em alguns ambientes, como servidores remotos, é impossível abrir um cliente com interface gráfica.

## Versionando arquivos

Como dito antes, o Git versiona arquivos. O nome que o Git dá para essas versões é *commit*. "Commitar" é o ato de criar uma nova versão. É importante mencionar que cada commit (versão) pode conter todas as mudanças de um conjunto de arquivos, e não de apenas um arquivo.

O conjunto de arquivos que o Git observa são os arquivos dentro de um repositorio Git. Um repositorio nada mais é do que uma pasta que foi "inicializada" pelo Git. Crie uma nova pasta e execute o comando na pasta que você criou:

```console
$ git init
Initialized empty Git repository in /caminho_para_sua_pasta/.git/
```
Se executar ``ls -a`` no seu terminal, verá que o Git criou um subdiretório chamado ``.git`` no seu diretório atual. A presença desse subdiretório é que indica que o diretório e todos os seus arquivos são parte de um mesmo repositório Git. Note que todas as informações que o Git possui sobre o repositório estão nesse subdiretório ``.git``, que funciona como um banco de dados de todas as informações que o Git precisa. 

Quando você executa qualquer comando Git, ele irá procurar no diretório atual em que você executou o comando, ou no diretório pai mais próximo, por um subdiretório ``.git`` e usará as informações contidas nele para realizar o comando. Para o diretório deixar de ser um repositório Git, basta deletar o subdiretório ``.git``.

Logo, é correto dizer que todos os seus commits ficam na pasta ``.git``, e que cada commit é uma versão dos arquivos desse repositório.

Agora vamos adicionar arquivos ao nosso repositório e criar commits. Nessa mesma pasta, crie um arquivo chamado ``main.py`` com esse conteúdo:

```python
print("Hello Word")
```
Execute-o com ``python main.py``. Se não tiver python na sua máquina, pode usar qualquer outra linguagem. O foco não é esse e será tranquilho seguir o tutorial.

Com um repositorio e um arquivo criado, já é possível executar o comando mais repetido do Git:

```console
$ git status
On branch master

Initial commit
 
Untracked files:
  (use "git add <file>..." to include in what will be committed)
 
 	main.py
 
nothing added to commit but untracked files present (use "git add" to track)
```
Esse comando mostra diversas informações sobre o repositório. Vamos focar só no fato de que ele está dizendo que o arquivo ``main.py`` está "untracked".

Mesmo que você tenha criado o repositorio, isso não significa que o Git irá versionar tudo que está dentro do repositorio. Você precisa explicitamente dizer a ele quais arquivos você quer versionar. No ``git status`` o Git sempre mostrará quais arquivos ele não está rastreando, ou seja, estão "untracked".

Para que o Git rastreie esse arquivo, execute:

```console
$ git add main.py
```
E depois o ``git status`` novamente:

```console
$ git status
On branch master
 
Initial commit
 
Changes to be committed:
   (use "git rm --cached <file>..." to unstage)
 
 	new file:   main.py
```
Agora que o Git está rastreando nosso arquivo, podemos criar um ``commit``. Como dito antes, o Git permite a você entender as alterações a serem feitas. Para isso, ele irá pedir uma descrição de que alteração foi feita e também precisará saber quem é você. 

Para dizer a ele quem deve ser considerado o criador do commit, execute o comando ``git config --global --edit`` e preencha no arquivo o seu username e email. Lembre de retirar os comentários das linhas com essas informações, removendo os ``#`` do começo das linhas.

Para passar a descrição do commit, usaremos a flag ``-m`` seguida da mensagem entre aspas:

```console
$ git commit -m "Adicionando arquivo main.py."
[master (root-commit) 324e034] Adicionando arquivo main.py.
 1 file changed, 1 insertion(+)
 create mode 100644 main.py
```
Caso você não passe o argumento ``-m``, um editor de texto feito para terminal deverá abrir para que você coloque a mensagem. Basta editar o arquivo e salvar para terminar o commit.

Executando ``git status`` novamente vemos que não existe nenhuma mudança presente.

Como dito antes, o que acabamos de fazer com apenas um arquivo também pode ser feito para multiplos arquivos. Ou seja, um commit pode conter mudanças de mais de um arquivo. Vamos criar um novo e commitar:

```console
$ echo "Repositorio para demonstração." >> readme.txt # criando o arquivo usando bash
$ git add readme.txt
$ git commit -m "Adicionando readme"
[master 5bc21f2] Adicionando readme
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
```
Agora edite ambos os arquivos. Como o nosso "Hello World!" está muito desanimado, adicione algumas exclamações e mude o texto do readme.txt. Agora execute ``git status``:

```console
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   main.py
	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
O git considera ambas as mudanças e diz que os arquivos estão "not staged for commit"(pode entender "staged" por enquanto como "marcadas"). Se realizarmos ``git commit``, podemos ver que não existem alterações para o proximo commit, embora existam arquivos modificados: 

```console
$ git commit -m "qualquer mensagem"
On branch master
Changes not staged for commit:
	modified:   main.py
	modified:   readme.txt

no changes added to commit
```
Isso acontece porque também é preciso dizer ao Git quais alterações que irão compor o novo commit, assim como no caso de um arquivo recem criado marcado como untracked. Utilizamos ``git add`` para dizer ao Git que queremos a mudança no próximo commit.

```console
$ git add main.py
$ git add readme.txt
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   main.py
	modified:   readme.txt
```
Depois do ``git add`` o Git mostra essas mudanças como "changes to be commited", que significa que elas serão sim adicionadas em um novo commit. Agora basta utilizar ``git commit``:

```console
$ git commit -m "Pequenas modificações"
[master 9d71ac2] Pequenas modificações
 2 files changed, 2 insertions(+), 2 deletions(-)
```
Depois dessa introdução ao Git, podemos resumir o fluxo básico dessa forma:
- ``git status`` para entender o que você vai estar commitando. É comum fazer mudanças que você não espera e commita-lás sem querer.
- ``git add`` para adicionar arquivos ou mudanças no proximo commit. Lembre que é possível adicionar mudanças de mais de um arquivo. Cada commit é uma versão de todos os arquivos, e não de um só.
- ``git commit`` para criar o commit propriamente dito.

Por fim, algumas dicas para agilizar esse fluxo e deixar o dia a dia mais rápido:

1. Executar ``git add .``, que adiciona de uma vez só as modificações de todos os arquivos e arquivos untracked.
2. Ainda mais prático, é executar ``git commit -am 'sua mensagem'``. A flag ``-a`` passada faz o ``git commit`` adicionar todas as mudanças e criar um novo commit de uma vez só. Mas tome cuidado porque o ``git commit -am`` **NÃO** adiciona arquivos que estão untracked.


---


Ficaram algumas pontas soltas nesse processo: 
- porque é necessário ficar executando ``git add`` toda hora? 
- Como podemos ver os commits que já criamos?

Vamos voltar ao fluxo básico mais a frente. Vários comandos úteis ainda não foram mencionados e alguns conceitos foram pulados para simplificar a introdução. Por exemplo, o que o adjetivo "staged" quer dizer. Mas na proxima parte vamos focar especificamente nos commits criados, ou seja, na história de nosso repositório. Não faz muito sentido aprender a criar uma história se não podemos acessa-la.

[Parte 2: Historia](2_historia.md)