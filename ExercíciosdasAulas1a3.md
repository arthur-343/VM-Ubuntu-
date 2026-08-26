
# Banco de Questões - Gabarito e Estudo Dirigido

**Disciplina:** Laboratório de Sistemas Operacionais e Redes (LSOR)  
**Aluno:** Arthur Soares Silva  
**Curso:** Bacharelado em Sistemas de Informação (BSI)  

---

## SEÇÃO I: Configuração e Instalação do Ambiente (Aula 1)

**Questão 1**  
Para iniciar os trabalhos práticos no laboratório da disciplina, qual o caminho do compartilhamento de rede no Windows que o aluno deve acessar para copiar a imagem ISO original do Ubuntu Server 26.04?  
* **Resposta Correta:** **C) `\\172.20.22.179\labredes`**  
* **Explicação:** O servidor de arquivos do laboratório hospeda a imagem ISO original no compartilhamento `\\172.20.22.179\labredes`.

**Questão 2**  
Ao criar uma nova Máquina Virtual (VM) no VirtualBox para o nosso servidor Linux de laboratório, quais são as configurações mínimas de hardware recomendadas no roteiro prático?  
* **Resposta Correta:** **B) 512 MB de RAM, 1 CPU e 32 GB de Disco Rígido (VDI Alocação Dinâmica).**  
* **Explicação:** Configuração otimizada para o ambiente acadêmico: 512 MB de RAM, 1 CPU e 32 GB de Disco VDI dinâmico.

**Questão 3**  
Durante o processo de particionamento personalizado via LVM (Logical Volume Manager) na instalação do Ubuntu Server, quais são os tamanhos de partições que os alunos devem configurar, respectivamente, para a raiz (`/`), o diretório de inicialização (`/boot`) e a área de troca (`SWAP`)?  
* **Resposta Correta:** **C) `/` com 29 GB, `/boot` com 1 GB e `SWAP` com 2 GB.**  
* **Explicação:** O disco de 32 GB é particionado em `/boot` de 1 GB (fora do LVM em ext4), `/` com 29 GB (`ubuntu-lv`) e `SWAP` com 2 GB (`swap-lv`).

**Questão 4**  
No processo de configuração de perfil (*Profile setup*) durante a instalação do Ubuntu Server 26.04 no laboratório do semestre 2026.02, quais credenciais padrão devem ser utilizadas?  
* **Resposta Correta:** **B) Nome de usuário `administrador` com a senha `adminifal`.**  
* **Explicação:** No semestre 2026.02, o padrão adotado para o usuário administrativo é `administrador` com a senha `adminifal`.

**Questão 5**  
Após concluir a instalação do sistema operacional e realizar o primeiro login no terminal, qual comando o aluno deve executar para sincronizar os índices dos repositórios de pacotes do Ubuntu antes de instalar novos serviços?  
* **Resposta Correta:** **C) `sudo apt-get update`**  
* **Explicação:** Baixa a lista atualizada de pacotes e versões disponíveis nos repositórios oficiais.

---

## SEÇÃO II: Administração de Usuários, Grupos e Permissões (Aula 2)

**Questão 6**  
Um aluno precisa criar quatro novas contas de usuários para a prática de laboratório: `fulano`, `cicrano`, `beltrano` e `novato`. Qual comando interativo cria uma conta e já solicita a definição de senha, criação do diretório `/home` e preenchimento dos dados do usuário?  
* **Resposta Correta:** **B) `sudo adduser fulano`**  
* **Explicação:** O `adduser` é um script amigável de alto nível que cria o diretório home, copia os arquivos esqueleto e solicita a senha interativamente.

**Questão 7**  
Para fins de trabalho compartilhado em laboratório, foi criado o grupo `devs`. Qual comando abaixo adiciona o usuário `fulano` a esse grupo secundário mantendo-o nos demais grupos que já fazia parte?  
* **Resposta Correta:** **B) `sudo usermod -aG devs fulano`**  
* **Explicação:** A flag `-aG` (*append/Group*) adiciona o usuário a grupos secundários sem removê-lo dos grupos aos quais ele já pertencia.

**Questão 8**  
Analise o seguinte cenário de permissões em um diretório chamado `/srv/projeto` no console do servidor:  
`drwxrwx--- 2 administrador devs 4096 Aug 12 14:30 /srv/projeto`  
O usuário `novato` **não** pertence ao grupo `devs`. O que acontece quando ele tenta acessar a pasta com o comando `cd /srv/projeto`?  
* **Resposta Correta:** **C) O acesso é bloqueado e o console exibe a mensagem: `-bash: cd: /srv/projeto: Permission denied`.**  
* **Explicação:** As permissões `770` (`drwxrwx---`) bloqueiam o acesso para terceiros (*others* = `---`). Como `novato` não é o dono e não pertence ao grupo `devs`, seu acesso é negado.

**Questão 9**  
Qual o comando correto para alterar simultaneamente o dono de um diretório `/srv/projeto` para `administrador` e o grupo associado para `devs`?  
* **Resposta Correta:** **D) `sudo chown administrador:devs /srv/projeto`**  
* **Explicação:** O comando `chown` aceita a sintaxe `usuario:grupo` para alterar proprietário e grupo de uma só vez.

---

## SEÇÃO III: Estrutura de Diretórios, Pastas do Sistema e FHS (Aula 3)

**Questão 10**  
O padrão FHS (Filesystem Hierarchy Standard) define o propósito e o local de cada tipo de arquivo em sistemas Linux. Em qual diretório do sistema o administrador deve procurar pelos arquivos de configuração locais de serviços de rede e do sistema operacional?  
* **Resposta Correta:** **B) `/etc`**  
* **Explicação:** O diretório `/etc` é reservado exclusivamente para os arquivos de configuração do sistema e seus serviços.

**Questão 11**  
Um administrador precisa criar a árvore de subdiretórios `/srv/ti/suporte/infra` de uma única vez, sabendo que as pastas intermediárias (`ti` e `suporte`) ainda não existem no servidor. Qual parâmetro do comando `mkdir` permite criar esses caminhos aninhados de forma recursiva sem gerar erros?  
* **Resposta Correta:** **B) `mkdir -p /srv/ti/suporte/infra`**  
* **Explicação:** O parâmetro `-p` (*parents*) instrui o comando a criar todas as pastas pai intermediárias inexistentes.

**Questão 12**  
De acordo com as definições do padrão FHS do Linux, qual a finalidade principal do diretório `/var` no sistema operacional?  
* **Resposta Correta:** **C) Armazenar arquivos de dados variáveis, como logs do sistema, filas de spool e arquivos temporários de serviços.**  
* **Explicação:** A pasta `/var` (*variable*) armazena dados dinâmicos que crescem constantemente ao longo do tempo (ex: `/var/log`).

**Questão 13**  
Analise as permissões de leitura (`r`), escrita (`w`) e execução (`x`) exibidas a seguir:  
`drwxr-xr-x 5 administrador ti-dept 4096 Aug 19 14:00 /srv/ti`  
O que os membros do grupo `ti-dept` podem fazer neste diretório?  
* **Resposta Correta:** **B) Podem ler e listar o conteúdo, além de entrar no diretório, mas não têm permissão para criar, modificar ou apagar arquivos.**  
* **Explicação:** A permissão do grupo é `r-x`. A ausência da letra `w` (substituída por `-`) impede a criação, modificação ou remoção de arquivos.

**Questão 14**  
Um aluno precisa alterar recursivamente as permissões de grupo do diretório `/srv/ti` e de todo seu conteúdo interno para `ti-dept`. Qual comando executa essa tarefa corretamente?  
* **Resposta Correta:** **C) `sudo chgrp -R ti-dept /srv/ti`**  
* **Explicação:** O comando `chgrp` altera o grupo proprietário, e a flag `-R` (*Recursive*) propaga a alteração para subdiretórios e arquivos.

**Questão 15**  
O padrão FHS separa os binários executáveis das ferramentas do sistema entre as pastas `/bin` e `/sbin`. Qual é a diferença conceitual e operacional básica entre esses dois diretórios no Linux?  
* **Resposta Correta:** **C) `/bin` armazena comandos utilitários acessíveis a todos os usuários do sistema, enquanto `/sbin` armazena executáveis administrativos voltados para a manutenção e que geralmente exigem privilégios de superusuário (`root` ou `sudo`).**  
* **Explicação:** O diretório `/bin` contém utilitários gerais do sistema (`ls`, `cd`), enquanto `/sbin` (*system binaries*) contém ferramentas exclusivas para tarefas administrativas do sistema.
