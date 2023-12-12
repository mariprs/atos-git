## Quick start - Git installation
Instalando Git:
* sudo yum install git: para sistemas baseados no Debian, como Ubuntu.
**ou**
* sudo apt-get install git: para sistemas baseados em Red Hat, como o CentOS. 
**ou**
* Instale o Git Bash diretamente no website do Git

# Gerenciando configurações usando `git config`:
* git config --global user.name <username>
* git config --global user.email <email_addr>
* `git config --system core.editor <insira seu editor aqui>` -> o Git pode solicitar que você insira texto em um editor para editar um arquivo se precisar de mais informações (se você não der um commit com mensagem, por exemplo)
**OU**
* Use `cat ~/.gitconfig` ou `cat /etc/gitconfig` e, em seguida, formate-o assim:
    ```
    name = <seu nome>
    email = <seu email>
    ```
# Outros comandos iniciais básicos
* `git init <diretório-do-repositório>`: cria um novo repositório no diretório específico. Este comando criará um diretório .git.
* `git add <nome-do-arquivo>` ou `git add *`: usado para inserir arquivos relevantes (ou todos os arquivos) a serem adicionados ao branch.
* `git status`: rastreia informações sobre os arquivos (alterações, quais arquivos serão commitados).
* `git commit -m "texto descrevendo as alterações"`: este comando fornece um espaço para escrever uma mensagem na qual você especificará quais as alterações que estão sendo commitadas à branch (seja criar ou deletar um novo arquivo ou atualizar um existente). Atenção: ao usar `git commit` sem nada, você será levado a um editor de texto. Isso é o Git enfatizando a necessidade de escrever uma mensagem antes de dar o seu push.
* `git rm <arquivo>`: quando você não quer mais rastrear um arquivo, remova-o da árvore usando este comando. Esse comando também excluirá o arquivo. 🫠
* `git checkout -- <arquivo>`: descarta as alterações do diretório de trabalho. Se você excluir algo acidentalmente, com esse comando Git o ressuscitará.
* `git reset HEAD <file>`: desfaz o staging de alterações a serem confirmadas, permitindo que você reavalie as alterações antes de confirmá-las.


# Arquivos GIGANTESCOS (ou "você já ouviu falar do Git LFS?")
* O Git bloqueia arquivos maiores que 100 MiB; para qualquer coisa maior que isso, você deve usar **Git Large File Storage (Git LFS)**.
* Se você tentar adicionar ou atualizar um arquivo maior que 50 MiB, receberá um aviso do Git. As alterações ainda serão enviadas com sucesso para o seu repositório, mas é um aviso interessante para te fazer considerar remover o commit para minimizar o impacto no desempenho. Para removê-lo, faça o seguinte:
  1. `git rm --cached <arquivo-gigantesco>` # isso não removerá o arquivo do disco, a propósito
  2. `git commit --amend -CHEAD` # este comando altera o commit anterior com sua alteração. Meramente criar um novo commit não funcionará, pois você precisa remover o arquivo do histórico não enviado também.
  3. `git push` # agora enviamos o commit sem o arquivo gigantesco

# "Acidentalmente inseri dados sensíveis em um repositório, excluir e fazer um commit novo funcionará para removê-lo completamente de pessoas maliciosas da internet?" Não.
Você precisará removê-lo do histórico do repositório. Pra isso, há duas opções: você pode usar o BFG Repo-Cleaner (ferramenta de código aberto, mais rápida e mais simples que a próxima solução) ou a ferramenta `git filter-repo`, como visto abaixo:

1. **Começando com BFG**:
   Vamos dizer que você commitou outra alteração após o commit contendo a info sensível. Usar BFG deixará seu commit mais recente intocado: basta mandar o comando `bfg --delete-files <seu_arquivo_com_dados_sensíveis>`
   * E para substituir algum texto listado em `senhas.txt` em qualquer lugar em que puder ser encontrado no histórico do seu repositório, execute: `bfg --replace-text senhas.txt`. Depois disso, você deve forçar o push para reescrever o histórico do repositório, usando `git push --force`.

   !!! Importante: esses commits ainda podem ser acessados em clones ou forks do seu repositório. Eles só serão inacessíveis em qualquer branch ou tag do seu repositório. Você terá que entrar em contato com o Suporte do Github pra resolver essa parte.

2. **Agora, usando `git filter-repo`**:
   * Esse método consiste em remover seu arquivo com dados sensíveis do histórico do seu repositório e adicioná-lo ao `.gitignore` para garantir que não seja reconfirmado acidentalmente.
   * Instale a versão mais recente da ferramenta git filter-repo usando o comando `brew install git-filter-repo`.
   * Se você ainda não tiver uma cópia do repositório com dados sensíveis em seu computador local, clone-o com `git clone <repositório com o arquivo sensível>`.
   * Navegue até ele com `cd <pasta com o arquivo sensível>`.
   * Execute o seguinte comando para forçar o Git a processar (mas não fazer checkout) todo o histórico de cada branch e tag, remover o arquivo especificado, assim como qualquer commit vazio, remover algumas configurações (como a URL remota) armazenadas no arquivo .git/config e sobrescrever suas tags existentes: `git filter-repo --invert-paths --path <caminho para seu arquivo com dados sensíveis>`
   * Agora, adicione seu arquivo com dados sensíveis ao .gitignore usando: `echo "seu arquivo com dados sensíveis" >> .gitignore`.
   * Adicione à tree: `git add .gitignore`.
   * Confirme: `git commit -m "acabei de corrigir uma bagunça gigantesca"`.
   * Verifique se você removeu tudo o que desejava do histórico do seu repositório.
   * Use `git remote add origin https://github.com/OWNER/REPOSITORY.git` para restaurar seus commits.
   * Você vai precisar dar um force-push em suas alterações locais para sobrescrever seu repositório no Github.com, assim como os branches que você enviou. Faça: `git push origin --force --all` e, em seguida, você também precisará forçar o push contra suas tags do Git `git push origin --force --tags`.

# **Independente da ferramenta escolhida, você terá que contatar o Suporte do GitHub para tornar essa info inacessível:**
1. Entre em contato com o Github através do Portal de Suporte do Github.
2. Diga aos colegas de trabalho para dar rebase e não fazer merge nos branches que criaram com base no histórico antigo do seu repositório, senão você terá que refazer todo o passo a passo anterior.

* git reset HEAD <file>: unstage changes to be commited



# **After using each one of those tools, you'll have to remove the sensitive data from Github:**
1. Contact Github through the Github Support Portal
2. Tell co-workers to rebase and not merge an branches they created off of your old repository history. 

To write:
# Branching
# Git and GitHub: differences
# `git log`
With this command you can see all commits with info about their author (including their email address), the date and the message they committed 
* If you use `git log --oneline` less info will be displayed (only display the code of the commit and the messages)
* If you use `git log -p`, more info will be displayed (such ass the amount of lines that were added and deleted). You can also perform a quick search using `/` followed by the terms you want to search inside the log or simply write `n` to change between pages and shift + `n` to go back.
# `git config -l`
# Rebase vs Merge
# Git Tags
# Git LFS
# .gitignore
(insert what it is and mention `git config --global core.excludesfile <path>` as an alternative means, add that .gitignore will not work if you already commited the files)
# O que é GitOps?

# Sources
[Git-scm (PT-BR) on Merging and Rebasing](https://git-scm.com/book/pt-br/v2/Branches-no-Git-Rebase)
[Git docs on Git Tags](https://docs.github.com/pt/rest/git/tags?apiVersion=2022-11-28)
[Git docs on Git LFS](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage)
[Git docs on removing sensitive info from your repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)