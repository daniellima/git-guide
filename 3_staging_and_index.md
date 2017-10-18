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
![ilustração dos estados de um arquivo e como navegar entre eles](https://raw.githubusercontent.com/stone-payments/git-workshop/master/images/git_file_states.png?token=AC8p7Gb-tZarTWHY5Czcjb6nuLnKBAFIks5Z6lHbwA%3D%3D)
> Lembrando que é necessário executar ``git commit`` depois do ``git rm --cached`` para que o arquivo seja realmente deletado. Também lembre-se que mudanças individuais no arquivo e não necessariamente o arquivo todo podem ser adicionadas na área de staging.
