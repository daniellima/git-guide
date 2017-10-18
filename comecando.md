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
Edite ambos os arquivos e veja:

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
O git considera ambas as mudanças. Para commitá-las, basta seguir os mesmos passos de antes: ``git add`` e ``git commit``.

```console
$ git add main.py
$ git add readme.txt 
$ git commit -m "Pequenas modificações"
[master 9d71ac2] Pequenas modificações
 2 files changed, 2 insertions(+), 2 deletions(-)
```
> Existem duas coisas que você pode fazer para tornar esse processo mais rápido. Uma é executar ``git add .``, que adiciona as modificações de todos os arquivos e arquivos untracked. 
> Outra, ainda mais prática, é executar ``git commit -am 'sua mensagem'``. Preste atenção na flag ``-a`` passada. Ele adiciona as mudanças e cria um novo commit de uma vez só. Mas tome cuidado porque o ``git commit -am`` **NÃO** adiciona arquivos que estão untracked.
> Sempre execute ``git status`` para entender o que você vai estar commitando. É comum fazer mudanças que você não espera e commita-lás sem querer.

# Usando a Historia

No começo desse tutorial eu disse que criamos versões dos arquivos de mode que:
> você possa recuperar versões antigas e entender quais foram as mudanças que aconteceram entre essas versões.

Até agora só criamos commits. Está na hora de olhar o conjunto de commits que criamos, ou seja, a história de nossos arquivos.

(TODO)

# O Indice e a Area de Staging
Um ponto interessante que você pode estar se perguntando é: porque eu preciso sempre confirmar todas as mudanças que estou fazendo com ``git add``? A resposta é que o Git permite que você commite apenas *parte* das mudanças que você realizou. Edite ambos os arquivos e execute ``git add`` em apenas um deles:
```console
$ git add main.py
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   main.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```
Se você executar ``git commit``, somente as mudanças em main.py serão commitadas e as mudanças em readme.txt *NÃO* serão. Ou seja, existe uma clara diferença para o Git entre arquivos modificados que ele detectou e quais das mudanças efetivamente devem ser adicionadas no próximo commit.
```console
$ git commit -m "Alterando main.py sem alterar readme.txt"
[master 0247eab] test
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
Esse lugar, onde as mudanças que vão ser commitadas ficam, é chamado de "staging area". Uma mudança "staged" é uma que está pronta para ser commitada. Perceba que eu disse mudança, não necessariamente arquivo. É possível enviar para a area de staging apenas parte das alterações feitas. Altere o arquivo ``main.py`` da seguinte forma:
```python
# Escreve um hello world:

print("Hello World")

# Fim do programa
```
E agora execute ``git add --patch``:
```console
$ git add --patch
diff --git a/main.py b/main.py
index 184b866..e70ab09 100644
--- a/main.py
+++ b/main.py
@@ -1 +1,4 @@
+# Escreve um hello world:
 print("Hello World!!")
+
+# Fim do programa
Stage this hunk [y,n,q,a,d,/,s,e,?]? ?
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
@@ -1 +1,4 @@
+# Escreve um hello world:
 print("Hello World!!")
+
+# Fim do programa
Stage this hunk [y,n,q,a,d,/,s,e,?]? s
Split into 2 hunks.
@@ -1 +1,2 @@
+# Escreve um hello world:
 print("Hello World!!")
Stage this hunk [y,n,q,a,d,/,j,J,g,e,?]? y
@@ -1 +2,3 @@
 print("Hello World!!")
+
+# Fim do programa
Stage this hunk [y,n,q,a,d,/,K,g,e,?]? n
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   main.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   main.py
```
O comando ``git add --patch`` é interativo, e escrevendo ``?`` você consegue ver a lista de comandos. Nesses comandos anteriores, conseguimos commitar apenas a primeira linha mas não a segunda. O arquivo então aparece tanto como modificado, quanto como tendo modificações na area de staging.

Para ver essas modificações é possível usar o ``git diff``:
```console
$ git diff --staged
diff --git a/main.py b/main.py
index 184b866..e174620 100644
--- a/main.py
+++ b/main.py
@@ -1 +1,2 @@
+# Escreve um hello world:
 print("Hello World!!")
$ git diff
diff --git a/main.py b/main.py
index e174620..e70ab09 100644
--- a/main.py
+++ b/main.py
@@ -1,2 +1,4 @@
 # Escreve um hello world:
 print("Hello World!!")
+
+# Fim do programa
```
Usando ``git diff --staged`` você consegue ver as mudanças entre o que está modificado e o que está na area de staging.

Mas e se voce enviar algum coisa para a area de staging e quiser retirar depois? Usamos o comando ``git reset``:
```console
$ git reset --patch HEAD main.py
diff --git a/main.py b/main.py
index 184b866..e174620 100644
--- a/main.py
+++ b/main.py
@@ -1 +1,2 @@
+# Escreve um hello world:
 print("Hello World!!")
Unstage this hunk [y,n,q,a,d,/,e,?]? y

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   main.py

no changes added to commit (use "git add" and/or "git commit -a")
$ git diff
diff --git a/main.py b/main.py
index 184b866..e70ab09 100644
--- a/main.py
+++ b/main.py
@@ -1 +1,4 @@
+# Escreve um hello world:
 print("Hello World!!")
+
+# Fim do programa
```
O ``git reset``, nesse caso, funciona como o oposto do ``git add``, e até suporta a opção ``--path``. 
> Tome cuidado: ``git reset`` faz mais coisa do que simplesmente desfazer o ``git add``. 
> Aquele parametro ``HEAD``, por exemplo, deve ser usado exatamente assim. No futuro revisitaremos o ``git reset`` e vamos entender o que ``HEAD`` significa.

Só existe mais duas coisas para saber sobre arquivos e a área de staging: como desfazer mudanças em um arquivo e como retirar o arquivo do Git.

Para desfazer mudanças basta usar o ``git checkout``:
``
$ git checkout -- main.py
$ git status
On branch master
nothing to commit, working tree clean
$ cat main.py
print("Hello World!!")
``
Assim como o ``git reset``, esse comando faz mais do que simplesmente desfazer mudanças, e revisitaremos ele mais tarde.

Para retirar um arquivo do Git, deixando ele untracked, utilizamos o ``git rm``:
```console
$ git rm --cached readme.txt
rm 'readme.txt'
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	readme.txt
```
A opção ``--cached`` aqui na verdade quer dizer "staged". Ela diz ao Git para somente registrar a mudança da remoção na area de staging mas não efetivamente remover o arquivo. Como o ``git status`` mostra, o arquivo ainda existe no sistema de arquivos mas está untracked. Usar o ``git rm`` sem a opção ``--cached`` é equivalente a remover o arquivo manualmente e depois usar ``git add`` para registrar a mudança na area de staging.
```console
$ git rm readme.txt
$ git add readme.txt 
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    readme.txt
```

É importante notar que todos os comandos que recebem um nome de arquivo também aceitam padrões para trabalhar com mais de um arquivo de uma vez só, como por exemplo ``git add *.txt`` para adicionar todos os arquivos que terminam em ".txt" no diretorio atual.

Para concluir, vamos falar de alguns conceitos que são mencionados na documentação do Git e são importantes de conhecer.

Em textos relacionados a Git muitas vezes você ouvirá falar sobre "working tree" ou "working directory". Quando ler isso, o texto está se referenciando ao seu diretório, onde pode haver modificações ou não. É o lugar onde você efetivamente trabalha. Uma working tree é considerada "suja" quando existem modificações nela ou "limpa" caso contrário.

Outro 'lugar' frequentemente mencionado é o indice("index" no inglês). É no indice que o Git mantém as versões de um arquivo. É através da comparação do que está no indice com a sua working tree que ele consegue descobrir que existe alguma modificação em um ou mais arquivos.

Podemos então dizer que um arquivo, para o Git, pode estar em um desses quatro possíveis 'estados':
- **untracked**, quando o Git não sabe o que fazer com esse arquivo. 
- **indexado**, quando o Git já possui esse arquivo no indice e ele está igual a versão na working tree.
- **modificado**, quando o arquivo na working tree não está igual a ultima versão do indice.
- **staged**, quando modificações no arquivo foram postas na area de staging para serem commitadas.

Essa imagem resume os estados de um arquivo e como ele pode transitar entre esses estados:
![ilustração dos estados de um arquivo e como navegar entre eles](https://raw.githubusercontent.com/stone-payments/git-workshop/master/git_file_states.png?token=AC8p7Gb-tZarTWHY5Czcjb6nuLnKBAFIks5Z6lHbwA%3D%3D)
> Lembrando que é necessário executar ``git commit`` depois do ``git rm --cached`` para que o arquivo seja realmente deletado. Também lembre-se que mudanças individuais no arquivo e não necessariamente o arquivo todo podem ser adicionadas na área de staging.




