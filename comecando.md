# Git é uma forma de versionar seus arquivos

Git é uma ferramenta incrível, e seu uso mais básico é o de versionar arquivos. Versionar significa criar multiplas versões de um arquivo, de modo que você possa recuperar versões antigas e entender quais foram as mudanças que aconteceram entre essas versões.

## Terminal

Esse tutorial será dado usando como exemplos o git via terminal. Porque não utilizar um cliente Git com interface gráfica?

* Porque interfaces gráficas geralmente simplificam funcionalidades para você ou criam suas próprias formas de funcionar. Isso significa que se você trocar de ferramenta, acabará tendo que aprender muita coisa novamente.
* Dificilmente uma interface gráfica de dara acesso a todas as funcionalidades do Git.
* Procurar ajuda na internet geralmente te leva a pessoas dizendo que comandos executar, porque é muito mais fácil dizer o passo a passo via linha de comando.
* Em alguns ambientes, como servidores remotos, é impossvel abrir um cliente com interface gráfica.

Dito isso, existem casos em que realmente é melhor utilizar um cliente gráfico para Git, que serão comentados ao longo do tutorial.

## Versionando o primeiro arquivo

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

Mesmo que você tenha criado o repositorio, isso não significa que o Git irá versionar tudo que está dentro do repositorio. Você precisa explicitamente dizer a ele quais arquivos você quer versionar. No ``git status`` o Git sempre mostraŕa quais arquivos ele não está rastreando. Caso um arquivo nunca sera rastreado, é possível fazer o Git ignorar esse arquivo, como veremos adiante.

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
Agora que o Git reconhece nossa arquivo, podemos criar um ``commit``, que efetivamente diz ao Git para salvar nossas alterações no repositorio. Como dito antes, o Git permite a você entender as alterações a serem feitas. Por isso, ele irá pedir uma descrição de que alteração foi feita. Vamos simplesmente usar "Adicionando arquivo main.py." como descrição.
> Atenço: O Git precisa de seu nome e email para registrar quem fez as mudanças. A importancia disso ficará mais evidente ao longo do tutorial.
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
