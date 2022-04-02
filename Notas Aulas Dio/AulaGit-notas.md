# Entendendo o que é Git e sua importância


## Versionamento

1. Controle de Versão
2. Armazenamento em nuvem
3. Trabalho em equipe
4. Melhorar seu código
5. Reconhecimento

---

## Comandos básicos para um bom desempenho no terminal
**Windows**
```
-cd
-dir
-mkdir
-del/rmdir
```
**Unix**
```
-cd
-ls
-mkdir
-rm -rf
```
#### CLI - Command Line Interface
```
cd / -> Manda para a raiz (da pra usar em linux tb)
cd .. -> Volta 1 nivel
cls -> Clearscreen no windows
clear -> clear no linux (CTRL+L) funciona igual
silent on success (se der certo o terminal nao vai falar nada)

echo hello (vai responder hello)
echo hello > hello.txt (vai criar um arquivo hello.txt com hello dentro)

del -> remove todos os arquivos de uma determinada pasta

rmdir -> remove directory remove o diretorio e todos os arquivos
rmdir workspace /S /Q (as flags S e Q)

rm -rf workspace (No linux, o -rf significa -r significa recursivo e -f force pra nao trazer prompt)
```
---

## Tópicos fundamentais para entender o funcionamento do Git

* **SHA1**  
Secure Hash Algorithm - Conjunto de funções hash criptográficas projetadas pela NSA
Encriptação gera conjunto de caracteres identificador de 40 dígitos
é uma forma curta de representar um arquivo
```
echo "ola mundo" | openssl sha1 > (stdin)= f9fc856e559b950175f2b7cd7dad61facbe58e7b
```

**Usando o SHA1 o GIT consegue verificar se realizou qualquer modificação no arquivo**

---

## Objetos Internos do Git
Objetos Fundamentais

* **Blobs**  
Os objetos ficam guardados dentro dos Blobs.  
Os blob tem os seguintes metadados:  
```
Tamanho (9)
\0
Ola Mundo
```
```
funções usadas:
	echo 'conteudo' | git hash-object --stdin
	-echo -e 'blob 9\0conteudo' | openssl sha1
```

* **Trees**  
As Trees armazenam blobs e elas tem metadados como os Blobs.  
```
Tree <tamanho>
\0
blob sa4d8s texto.txt
```
* **Considerações**  

    * O Blob nao guarda o nome do arquivo (só o sha)  
    * As arvores guardam o nome do arquivo e toda a estrutura dos arquivos  
    * As arvores podem apontar tanto para os Blobs quanto outras arvores.
    * Sha1 com os metadados da arvore (o sha da bolha muda, entao o sha da arvore vai mudar tb).

Exemplo:
   

				Tree
		README	Rakefile	Lib
		blob		blob	  tree
							simplegit.rb
								blob
								
								


* **Commit**  
É o objeto vai juntar tudo o que foi feito no projeto e vai dar sentido a operação.  
Exemplo:  

```
Commit	<tamanho>
-tree	s4aa5sq1
-parente	a98acq1
-autor	perkles
-mensagem	"inicia ..."
-timestamp
````

**O SHA1 dessa commit é o hash de toda essa informação acima**


Se  vc alterar uma Blob (mudar um arquivo), entao vai alterar tambem os metadados dessa arvore,
e o commit aponta pra uma arvore que pode apontar pra outras arvores
então se vc mudar qualquer coisa em um arquivo ele vai refletir acima

Dessa forma vc consegue montar uma linha do tempo das acoes

- **Sistema distribuído e seguro**  
Ele é distribuido porque você tem seu código/repositório hosteado na nuvem,
entao o código que tem lá tem a versão mais atualizado do seu código
entao imagine que tem 40 pessoas contribuindo nele
entao tem uma versao desse código em 40 pessoas e a versão é a mesma

## **Chave SSH e Token**

Precisa se autenticar no github e isso acabou mudando com o tempo
inves de usar apenas o seu username e senha, essa forma já foi desligada, sendo necessário processos mais seguros.

* O primeiro conceito é a Chave SSH  
É uma forma de realizar uma conexão segura e confiável entre 2 máquinas
2 chaves sempre (uma pública e privada) e sempre precisamos passar a pública
para o github e vamos poder enviar códigos sem colocar senha

Geração da senha SSH pelo GITBASH : 
```
ssh-keygen -t ed25519 -C usuario.email@gmail.com
```
navegar ate a pasta proposta e em sequiencia
```
cat id_ed25519.pub
```

Utilizando o Gitbash, use o seguinte comando para abrir o agente:
```
eval $(ssh-agent -s)
```
Aparecerá  um Agent pid -numero e utilize o comando abaixo:
```
ssh-add id_ed25519
```
---

## Processo de clonar um repositório
```
git clone caminhosshquepegounogithub
```

A grande vantagem de ter a Chave SSH devidamente configurada, é a segurança extra e não precisar ficar digitando a senha toda vez. 

* **Token**  
Token de acesso pessoal  
O Token só é mostrado uma vez
Se parece algo como isso:
```
ghp_tvVXfbM3gbUsiCs8X7s0aLTyZuLc63V0zPx
```

* Clonar um Repositório com Token  
Usar o protocolo https - comando: 
```
git clone urlquevcquercopiar
```
Em seguida copia seu Personal Access Token

---

## Primeiros comandos com o GIT


* Iniciar o Git  
    * git init  
    Isso cria os arquivos referente sao Git.

Com relação aos arquivos e pastas, sempre que tem . na frente da pasta ela é ocula.
```
ls -a (mostra arquivos ocultos)
```
* Configurar o seu GitBash
```
git config --global user.email "silverio.terceiro@gmail.com"
git config --global name "SilverioT"
```


* git add
```
git add *
```
* git commit
```
git commit -m "commit inicial"
```

---

## Ciclo de vida dos arquivos no GIT

git init

Tracked ou Untracked

Cria um snapshot no commit, todos os arquivos quando sao commited viram unmodified.  
Quando vc modificar um arquivo ele fica modified e quando vc realiza o add ele vai pra staged.  
Quando utiliza o commit os arquivos voltam para unmodified pois foram modificados e esta é a ultima versão.


## Ambientes: Ambiente de desenvolvimento e Servidor.

```
Servidor
Remote Repository
---------------------
Ambiente de desenvolvimento
WORKING directory -> Staging Area -> Local Repository
```

Ambiente de desenvolvimento é tudo que está na sua máquina e Servidor tudo que está remoto como no GitHub.

Quando vc da add nos arquivos do seu working directory eles vao pro staging area
e quando você da um commit, esses arquivos que estao no staging viram um repositório local.

Só da pra mover o respositório local para o remoto. e só existem coisas no repositório local quando se faz um commit.

```
git restore --staged (to unstage)
```
--- 

## Trabalhando com o GitHub

```
git remote add origin git@github.com:SilverioT/livro-receitas.git
```

Mostra os repositorios
```
git remote -v
```

Por convensão usamos origin como alias

Para Empurrar o repositório
```
git push origin master
```
---

## Resolvendo Conflitos no GitHub e como resolvê-los

Ocorre um problema quando 2 pessoas mudam a mesma linha, ocasionando um:

* Merge Conflict  
O Github não consegue inferir quando a edição ocorre na mesma linha
e você manualmente precisa resolver esse conflito e empurrar o código para o github

Passos para resolver:
```
git pull => puxar (pegar o repositorio)
```
```
git pull origin master <- baixa o repositorio, 
Se mostrar um conflito de merge no arquivo.
```
```
O símbolo '>' que significa comentário
```
Arrume o código e use o comando:
```
git commit -m "resolve conflicts"
```
