# Git é uma forma de versionar seus arquivos

Git é uma ferramenta incrível, e seu uso mais básico é o de versionar arquivos. Versionar significa criar multiplas versões de um arquivo, de modo que você possa recuperar versões antigas e entender quais foram as mudanças que aconteceram entre essas versões.

## Porque esse tutorial?

Você provavelmente já sabe usar alguns comandos no git, como muitos sabem. A questão é que Git é uma ferramenta confusa. Claramente ser intuitivo para novos usuários não foi uma prioridade no seu desenvolvimento. Porém, o Git é muito poderoso e por isso se tornou central ao desenvolvimento de software atual. Com isso, temos usuário que usam diariamente algo que eles não entendem e acabam não conseguindo aproveitar totalmente.
![XKCD sobre Git](https://imgs.xkcd.com/comics/git.png)
O humilde objetivo desse tutorial é dar um conhecimento mais amplo de git para que voce possa entender  o que está fazendo quando executa aquele comando maluco que achou no stack overflow. Ou melhor, ser voce mesmo a pessoa que escreve aqueles comandos.

## Terminal

Esse tutorial será dado usando como exemplos o git via terminal. Porque não utilizar um cliente Git com interface gráfica?

* Porque interfaces gráficas geralmente simplificam funcionalidades para você ou criam suas próprias formas de funcionar. Isso significa que se você trocar de ferramenta, acabará tendo que aprender muita coisa novamente.
* Dificilmente uma interface gráfica de dara acesso a todas as funcionalidades do Git.
* Procurar ajuda na internet geralmente te leva a pessoas dizendo que comandos executar, porque é muito mais fácil dizer o passo a passo via linha de comando.
* Em alguns ambientes, como servidores remotos, é impossvel abrir um cliente com interface gráfica.

Dito isso, existem casos em que realmente é melhor utilizar um cliente gráfico para Git, que serão comentados ao longo do tutorial.

## Versionando arquivos

Crie uma pasta no sistema e crie um arquivo chamado ``main.py`` com esse conteúdo:

```
print("Hello Word")
```
Executeo com ``python main.py``.. Se não tiver python na sua máquina, pode usar qualquer outra linguagem. O foco não é esse e será tranquilho seguir o tutorial.

O Git trabalha com repositorios. Um repositorio nada mais é do que uma pasta que foi "inicializada" pelo Git. Para isso, execute o comando na pasta que você criou:
```
git init
# output
# Initialized empty Git repository in /caminho_para_sua_pasta/.git/
```
Se executar ``ls -a`` no seu terminal, verá que o Git criou uma pasta chamada ``.git`` no seu diretótio atual. A presença dessa pasta é que indica que a pasta e todos os arquivos e subdiretórios são partes de um mesmo repositório Git. Note que todas as informações que o Git possui sobre o repositorio estão nessa pasta ``.git``. Remova ela e essa pasta voltará a ser uma pasta normal novamente.

Com um repositorio criado, já é possível executar o comando mais repetido do Git:
```
git status
# output:
# On branch master
#
# Initial commit
# 
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
# 
# 	main.py
# 
# nothing added to commit but untracked files present (use "git add" to track)
```
Esse comando mostra diversas informações sobre o repositório. Vamos focar só no fato de que ele está dizendo que o arquivo ``main.py`` está "untracked".

Mesmo que você tenha criado o repositorio, isso não significa que o Git irá versionar tudo que está dentro do repositorio. Você precisa explicitamente dizer a ele quais arquivos você quer versionar. No ``git status`` o Git sempre mostraŕa quais arquivos ele não está rastreando.

Para que o Git rastreie esse arquivo, execute:
```
git add main.py
# output
# (vazio)
```
E depois o ``git status`` novamente:
```
git status
# output
# On branch master
# 
# Initial commit
# 
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
# 
# 	new file:   main.py
```
Agora que o Git reconhece nossa arquivo, podemos criar um ``commit``, que efetivamente diz ao Git para salvar nossas alterações no repositorio. Como dito antes, o Git permite a você entender as alterações a serem feitas. Para isso, ele irá pedir uma descrição de que alteração foi feita. Vamos simplesmente usar "Adicionando arquivo main.py." como descrição.
> Atenço: O Git precisa de seu nome e email para registrar quem fez as mudanças.
> Para registrar essas informações, execute ``git config --global --edit``e coloque seu nome e email.
```
git commit -m "Adicionando arquivo main.py."
# output
# [master (root-commit) 324e034] Adicionando arquivo main.py.
#  1 file changed, 1 insertion(+)
#  create mode 100644 main.py
```
Caso você não passe o argumento ``-m``, um editor de texto feito para terminal deverá abrir para que você coloque a mensagem. Basta editar o arquivo e salvar para terminar o commit.

Executando git status novamente vemos que não existe nenhuma mudança presente.

O que acabamos de fazer com apenas um arquivo também pode ser feito para multiplos arquivos. Vamos criar um novo e commitar:
```
$ echo "Repositorio para demonstração." >> readme.txt # criando o arquivo usando bash
$ git add readme.txt
$ git commit -m "Adicionando readme"
[master 5bc21f2] Adicionando readme
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
```
O Git coloca todas as mudanças que você faz em todos os arquivos dentro desse diretorio e nos subdiretorios juntos em um commit. Edite ambos os arquivos e veja:
```
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
```
$ git add main.py
$ git add readme.txt 
$ git commit -m "Pequenas modificações"
[master 9d71ac2] Pequenas modificações
 2 files changed, 2 insertions(+), 2 deletions(-)
```
> Existem duas coisa que você pode fazer para tornar esse processo mais rápido. Um é executar ``git add .``, que adiciona as modificações de todos os arquivos e arquivos untracked. 
> Outra, ainda melhor, é executar ``git commit -am 'sua mensagem'``. Ele adiciona as mudanças e cria um novo commit. Só tome cuidado porque o ``git commit -am`` **NÃO** adiciona arquivos que estão untracked.
> Sempre execute ``git status`` para entender o que você vai estar commitando. É comum fazer mudanças que você não espera e commita-lás sem querer.

Um ponto interessante que você pode estar se perguntando é: porque eu preciso sempre confirmar todas as mudanças que estou fazendo com ``git add``? A resposta é que o Git permite que você commite apenas *parte* das mudanças que você realizou. Edite novamente ambos os arquivos e execute ``git add`` em apenas um deles:
```
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
```
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
Esse lugar, onde as mudanças que vão ser commitadas ficam, é chamado de "staging area" ou "the index" pela documentação do Git. Uma mudança "staged" é uma que está pronta para ser commitada.

Ou seja, existem quatro possíveis 'estados' de um arquivo:
- **untracked**, quando o Git não sabe o que fazer com esse arquivo. 
- **tracked**, quando o Git já sabe, mas ele continua igual ao ultimo commit.
- **modificado**, quando o arquivo não está igual ao ultimo commit.
- **staged**, quando modificações no arquivo foram postas na staging area.

Existe mais a falar sobre essa staging area, mas voltaremos a isso depois.

# Usando a Historia
(TODO)

