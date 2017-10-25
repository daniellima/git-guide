# Usando a Historia

No começo desse tutorial foi dito que: "(o Git) te permite salvar versões antigas de um conjunto de arquivos e retornar a essas versões a qualquer momento, além de entender quais foram as mudanças entre cada versão e como elas aconteceram."

Até agora só criamos commits, ou seja, construimos a história. Está na hora de olhar a história que criamos.

## Log

Para listar todos os commits que você fez, é só executar ``git log``:

```console
$ git log
commit 6a6f7356ae179657b59a172429aff8131e821805 (HEAD -> master)
Author: Daniel Lima dos Santos <daniellima.pessoal@gmail.com>
Date:   Tue Oct 24 18:55:35 2017 -0200

    Pequenas modificações

commit f886c1d53d202cc87852d2a5c8c5fd50a6b819c2
Author: Daniel Lima dos Santos <daniellima.pessoal@gmail.com>
Date:   Tue Oct 24 18:54:43 2017 -0200

    Adicionando readme

commit 99708c9d2a1e025d5d52b694ecb66abf7941406f
Author: Daniel Lima dos Santos <daniellima.pessoal@gmail.com>
Date:   Tue Oct 24 18:54:26 2017 -0200

    Adicionando arquivo main.py
```
Ele mostrará todos os commits, junto ao indentificador único deles e a mensagem que explica a mudança. 

> Note que os indentificadores únicos em seu computador provavelmente serão diferentes, mas isso não importa. 
> O importante é ter pelo menos três commits.

Com muitos commits, fica bem dificil entender o que aconteceu de forma geral. Para melhorar o ``git log``, podemos usar a opção ``--oneline``:

```console
$ git log --oneline
6a6f735 (HEAD -> master) Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py

```
Assim, temos uma versão mais compacta. Note que somente as primeiras linhas da mensagem de commit são mostradas. É por esse motivo que é considerado uma boa prática a primeira linha de cada commit ser um resumo do que o commit está mudando e maiores detalhes serem dados nas próximas linhas da mensagem.

Podemos ainda limitar o número de commits que são mostrados, utilizando flags numericas:
```console
$ git log --oneline
6a6f735 (HEAD -> master) Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git log --oneline -2
6a6f735 (HEAD -> master) Pequenas modificações
f886c1d Adicionando readme
```
A flag ``-2`` significa "mostre somente os dois ultimos commits". Ela foi só um exemplo. Podemos usar qualquer número como ``-1`` ou ``-3``.

Um ponto interessante são os indentificadores de cada commit. O ultimo commit que o ``git log`` mostrou possui identificador ``6a6f7356ae179657b59a172429aff8131e821805``. O identificador é uma sequencia de 40 caracteres, que na prática representam um hash SHA-1. O motivo de ser um hash, e não uma sequencia numérica, faz mais sentido de ser discutido na explicação sobre como Git lida com a colaboração entre pessoas. Mas vale adiantar por curiosidade que esses identificadores **NÃO** são necessáriamente únicos em um repositório, embora a chance disso acontecer seja extremamente pequena.

No ``git log --oneline``, vemos uma versão do identificador com menos caracteres: ``6a6f735``. É só uma questão estética. Na prática, só com os primeiros caracteres já é possível indentificar unicamente todos os commits de um repositório e por isso só são mostrados tantos caracteres quanto necessários para isso. Como veremos, podemos nos referir aos commits só usando essa versão compacta ao invés de escrever todos os 40 caracteres do hash completo.

Por serem hashes, as pessoas se referem aos identificadores como tal. Por isso será assim que esse tutorial vai se referenciar a eles. Podemos dizer então que cada commit é indentificado por um hash.

As referencias a "HEAD" e "master" que aparecem no ``git log --oneline`` serão faladas mais a frente quando estivermos discutindo referencias no Git.

## Entendendo as mudanças
A partir do hash do commit, podemos descobrir maiores informações sobre ele. Basta executar ``git show hash`` e vc terá as informações sobre as mudanças ocorridas em um commit, quem foi seu autor e a mensagem que foi deixada para descrever as mudanças.

```console
$ git show 6a6f735
commit 6a6f7356ae179657b59a172429aff8131e821805 (HEAD -> master)
Author: Daniel Lima dos Santos <daniellima.pessoal@gmail.com>
Date:   Tue Oct 24 18:55:35 2017 -0200

    Pequenas modificações

diff --git a/main.py b/main.py
index f301245..184b866 100644
--- a/main.py
+++ b/main.py
@@ -1 +1 @@
-print("Hello World!")
+print("Hello World!!")
diff --git a/readme.txt b/readme.txt
index d57bec9..f4d58c6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
-Repositorio para demonstração.
+Repositorio para demonstrar o Git.

```
Outra forma de entender as mudanças que aconteceram ao longo do tempo é usando o ``git diff``. Basta executar ``git diff hash1 hash2`` e o git mostrará as diferenças que existem entre os commits indentificados por esses hashs.

```console
$ git diff 99708c9 f886c1d
diff --git a/readme.txt b/readme.txt
new file mode 100644
index 0000000..d57bec9
--- /dev/null
+++ b/readme.txt
@@ -0,0 +1 @@
+Repositorio para demonstração.

```
Nesse caso, o diff mostra que o arquivo readme.txt foi criado e que essa é a diferença entre o primeiro e o segundo commit.

## Recuperando versões anteriores de arquivos
Se for necessário voltar um ou mais commits, devemos usar o comando ``git revert``. 

Usando o comando ``git revert`` e passando o hash do ultimo commit podemos ver que voltamos ao estado anterior:

```console
$ git log --oneline
6a6f735 (HEAD -> master) Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git revert 6a6f735
[master 40e4465] Revert "Pequenas modificações"
 2 files changed, 2 insertions(+), 2 deletions(-)
$ cat main.py
print("Hello World!")
```
Ao executar esse comando, o Git abre automaticamente o seu editor de texto padrão para que você escreva uma mensagem de commit. Ele faz isso porque na verdade não está destruindo seus commits. Ao contrário: ele está criando novos commits que contém as modificações que voce fez de forma reversa. É fácil ver isso utilizando ``git log`` e ``git diff``:

```console
$ git log --oneline
40e4465 Revert "Pequenas modificações"
6a6f735 Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git diff f886c1d 6a6f735
diff --git a/main.py b/main.py
index f301245..184b866 100644
--- a/main.py
+++ b/main.py
@@ -1 +1 @@
-print("Hello World!")
+print("Hello World!!")
diff --git a/readme.txt b/readme.txt
index d57bec9..f4d58c6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
-Repositorio para demonstração.
+Repositorio para demonstrar o Git.
$ git diff 6a6f735 40e4465
diff --git a/main.py b/main.py
index 184b866..f301245 100644
--- a/main.py
+++ b/main.py
@@ -1 +1 @@
-print("Hello World!!")
+print("Hello World!")
diff --git a/readme.txt b/readme.txt
index f4d58c6..d57bec9 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
-Repositorio para demonstrar o Git.
+Repositorio para demonstração.
```

> As mensagens por default tem um "Revert " e a mensagem do commit que está sendo revertido. É interessante que se entre com mais informações explicando o porque da reversão estar sendo feita.

Se continuarmos a fazer isso, voltamos cada vez mais um passo para trás:

```console
$ git revert f886c1d
[master c1e1e91] Revert "Adicionando readme"
 1 file changed, 1 deletion(-)
 delete mode 100644 readme.txt
$ ls
main.py
```
Caso voce queira reverter mais de um commit, basta executar git reset hash1..hash2 e o git irá reverter todos os commits entre o commit hash1 e o hash2, criando commits opostos em ordem reversa. Note que o hash1 é o commit anterior ao ultimo cujas alterações queremos desfazer. 

Vamos aproveitar isso para reverter os dois ultimos commits, que foram responsáveis por reverter o repositorio para um estado igual ao primeiro commit. Com isso, o repositorio voltará ao estado que estava antes de começarmos as reversões:

```console
$ git log --oneline
c1e1e91 (HEAD -> master) Revert "Adicionando readme"
40e4465 Revert "Pequenas modificações"
6a6f735 Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git revert 6a6f735..c1e1e91
[master 39fa72d] Revert "Revert "Adicionando readme""
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
[master 2b2c6f0] Revert "Revert "Pequenas modificações""
 2 files changed, 2 insertions(+), 2 deletions(-)
$ git log --oneline
2b2c6f0 (HEAD -> master) Revert "Revert "Pequenas modificações""
39fa72d Revert "Revert "Adicionando readme""
c1e1e91 Revert "Adicionando readme"
40e4465 Revert "Pequenas modificações"
6a6f735 Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ cat main.py
print("Hello World!!")
```
Utilizamos o ``git log`` para conseguir os hashes e depois executamos ``git revert``. 

Um problema que começa a ficar evidente é que caso voce reverta 50 commits, serão criados 50 novos commits revertendo as alterações e isso pode deixar a história confusa de ler, fora que será necessário entrar com uma mensagem nova para cada um deles. Para melhorar isso, podemos usar a flag ``--no-commit`` no ``git revert``:

```console
$ git log --oneline
2b2c6f0 (HEAD -> master) Revert "Revert "Pequenas modificações""
39fa72d Revert "Revert "Adicionando readme""
c1e1e91 Revert "Adicionando readme"
40e4465 Revert "Pequenas modificações"
6a6f735 Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git revert --no-commit 99708c9..2b2c6f0
$ git status
On branch master
You are currently reverting commit f886c1d.
  (all conflicts fixed: run "git revert --continue")
  (use "git revert --abort" to cancel the revert operation)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   main.py
	deleted:    readme.txt

$ cat main.py 
print("Hello World!")
```
Como podemos ver, esse comando irá reverter tudo mas não criará um novo commit. Ele na verdade deixará todas as alterações necessárias para reverter o código prontas para serem commitadas. Basta então executar ``git commit`` para que somente um commit seja criado: 

```console
$ git commit -m "Revertendo repositorio para o início"
[master f30f051] Revertendo repositorio para o início
 2 files changed, 1 insertion(+), 2 deletions(-)
 delete mode 100644 readme.txt
```
Um ponto importante: o comando ``git revert``, quando usado com o hash de um commit, não necessita que ele seja o commit imediatamente anterior. Por exemplo, podemos tentar reverter o primeiro commit mesmo estando no ultimo. Nesse caso o git irá reverter somente as mudanças do commit escolhido. Isso as vezes gerará um conflito, pois o Git não saberá como reverter corretamente. Imagine reverter uma mudança em um arquivo que não existe mais:

```console
$ git log --oneline
f30f051 (HEAD -> master) Revertendo repositorio para o início
2b2c6f0 Revert "Revert "Pequenas modificações""
39fa72d Revert "Revert "Adicionando readme""
c1e1e91 Revert "Adicionando readme"
40e4465 Revert "Pequenas modificações"
6a6f735 Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git revert 6a6f735
error: could not revert 6a6f735... Pequenas modificações
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
$ git status
On branch master
You are currently reverting commit 6a6f735.
  (fix conflicts and run "git revert --continue")
  (use "git revert --abort" to cancel the revert operation)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add/rm <file>..." as appropriate to mark resolution)

	deleted by us:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
Conflitos aparecem em outros momentos, mas a cara deles é basicamente essa. Eles surgem quando mandamos o Git fazer algo que ele não consegue fazer sozinho. O conflito é então a forma do Git pedir intervenção manual. Falaremos mais sobre conflitos mas a frente. Por enquanto, caso se encontre nessa situação depois de um ``git revert``, basta executar ``git revert --abort`` para cancelar a reversão. 

```console
$ git revert --abort
$ git status
On branch master
nothing to commit, working tree clean
```
É possível usar ``git show`` e ``git diff`` para tentar fazer a reversão manualmente em um novo commit.

## Voltando no tempo
Para voltar a uma versão anterior do arquivo, podemos usar o comando ``git checkout``. Basta executar ``git checkout hash`` e os arquivos irão retornar para a versão em que eles estavam depois do commit hash1 ter sido realizado. Para voltar ao normal, é só executar ``git checkout master``. 

```console
$ git log --oneline
f30f051 (HEAD -> master) Revertendo repositorio para o início
2b2c6f0 Revert "Revert "Pequenas modificações""
39fa72d Revert "Revert "Adicionando readme""
c1e1e91 Revert "Adicionando readme"
40e4465 Revert "Pequenas modificações"
6a6f735 Pequenas modificações
f886c1d Adicionando readme
99708c9 Adicionando arquivo main.py
$ git checkout f886c1d
Note: checking out 'f886c1d'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at f886c1d... Adicionando readme
$ ls
main.py  readme.txt
$ git checkout master
Previous HEAD position was f886c1d... Adicionando readme
Switched to branch 'master'
$ ls
main.py
```
O primeiro checkout mostra uma mensagem sobre 'branchs', 'detached HEAD' e a flag ``-b`` do ``git checkout``. Isso acontece porque o comando ``git checkout`` não é só usado com esse motivo. Na verdade, ele está mais relacionado à refencias. Após saber sobre referencias a mensagem que o ``git checkout`` mostrou fará bem mais sentido.


---


Mas como dito na parte anterior do tutorial, primeiro vamos voltar ao fluxo básico do ``git status``, ``git add`` e ``git commit`` para explorá-lo com mais calma e conhecer alguns conceitos essenciais ao Git.

[Parte 3: O Indice e a Area de Staging](3_staging_e_indice.md)
