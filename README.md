# VM-Ubuntu-

# Relatório Técnico - Aula Prática 01: Introdução à Virtualização e Instalação do Ubuntu Server 26.04

## 1. Identificação
* **Nome do Aluno:** [Seu Nome Completo]
* **Curso:** Bacharelado em Sistemas de Informação (BSI)
* **Turma:** [Sua Turma / Período]
* **Data de Execução:** [Data do Laboratório, ex: 25/08/2026]
* **Título da Prática:** Introdução à Virtualização e Instalação do Ubuntu Server 26.04 LTS

---

## 2. Objetivo
Esta atividade prática teve como objetivo compreender os conceitos fundamentais de virtualização, hipervisores (Tipo 2) e isolamento de recursos. A prática abrangeu a criação e preparação de uma máquina virtual no Oracle VM VirtualBox, a instalação customizada do sistema operacional de rede Ubuntu Server 26.04 LTS utilizando gerenciamento de volumes lógicos (LVM), a ativação do serviço OpenSSH e a validação do ambiente via linha de comando.

---

## 3. Ambiente
* **Hospedeiro (Host Physical Machine):**
  * **Sistema Operacional:** Windows 11 Pro
  * **Processador:** [Ex: Intel Core i5 / AMD Ryzen 5]
  * **Memória RAM:** [Ex: 8 GB / 16 GB]
* **Virtualizador:** Oracle VM VirtualBox (com Extension Pack instalado)
* **Mídia de Instalação:** `ubuntu-26.04-live-server-amd64.iso`
* **Máquina Virtual (VM Target):**
  * **Nome:** `ubuntu_server`
  * **Diretório:** `C:\2026\BSI\VM\[SeuNomeCompleto]\ubuntu_server`
  * **vCPU:** 1 CPU
  * **Memória RAM:** 512 MB
  * **Disco Rígido Virtual:** VDI de 32 GB (Reservado Dinamicamente)
  * **Interface de Rede:** Placa em modo NAT (`enp0s3`)

---

## 4. Procedimento
1. **Organização no Host:** Criação do diretório de originais (`C:\2026\BSI\VM\original`) para armazenamento da imagem ISO e criação do diretório de trabalho do aluno.
2. **Criação da VM:** Configuração de uma nova máquina virtual no VirtualBox especificando o tipo Linux / Ubuntu (64-bit), com 512 MB de RAM e disco VDI dinâmico de 32 GB.
3. **Mídia e Boot:** Anexação da ISO na controladora IDE e inicialização da máquina virtual.
4. **Configurações Iniciais:** Seleção do idioma do instalador em `English`, layout de teclado em `Portuguese (Brazil)` e confirmação da obtenção do endereço IP via DHCP na interface `enp0s3`.
5. **Particionamento LVM Customizado:**
   * Criação da partição `/boot` dedicada de 1 GB (1024M), formatada em `ext4`.
   * Criação de uma partição não formatada (*Leave unformatted*) no espaço restante para alocação de LVM.
   * Criação do Volume Group nomeado `ubuntu-vg`.
   * Criação do Volume Lógico Raiz (`ubuntu-lv`) com 29 GB, formato `ext4` e ponto de montagem `/`.
   * Criação do Volume Lógico de Troca (`swap-lv`) com ~2 GB, formato `swap`.
6. **Credenciais do Sistema:** Configuração do nome do servidor (`ubuntu_server`), nome de usuário (`administrador`) e senha (`adminifal`).
7. **Serviços Adicionais:** Pulo da integração com o Ubuntu Pro e marcação da opção `[X] Install OpenSSH Server` para permitir futuros acessos remotos.

---

## 5. Testes e Validação
A validação do sistema foi efetuada através dos seguintes comandos executados no primeiro login:

* **Validação de Rede (`ip addr`):** Confirmação de que a interface `enp0s3` obteve com sucesso um endereço IP na faixa do DHCP da rede NAT do VirtualBox (ex: `10.0.2.15/24`).
  > *[Inserir Print / Imagem da tela exibindo a saída do comando ip addr]*

* **Validação de Particionamento e LVM (`df -h`):** Confirmação de que a partição `/boot` de 1 GB está montada separadamente e de que o volume lógico `/dev/mapper/ubuntu--vg-ubuntu--lv` está devidamente montado na raiz `/` do sistema com o tamanho especificado.
  > *[Inserir Print / Imagem da tela exibindo a saída do comando df -h]*

* **Atualização do Repositório (`sudo apt-get update`):** Execução da atualização dos índices do gerenciador de pacotes `apt` com privilégios de superusuário, confirmando o pleno acesso do servidor à internet.
  > *[Inserir Print / Imagem da tela exibindo a atualização concluída sem erros]*

---

## 6. Problemas e Soluções
* **Problema 1: Impossibilidade de concluir o particionamento (Botão `[ Done ]` desativado)**
  * *Sintoma:* Mensagem em vermelho no topo do instalador exibindo `"To continue you need to: Select a boot disk"`.
  * *Solução:* Foi necessário aplicar um *Reset* nas alterações e criar explicitamente uma partição não-LVM de 1 GB formatada em `ext4` com o ponto de montagem `/boot`.

* **Problema 2: Opção `Create volume group (LVM)` desativada (em cinza)**
  * *Sintoma:* Ao tentar criar o grupo LVM diretamente sobre o espaço livre, a opção ficava inacessível e a criação de partição solicitava o campo *Size*.
  * *Solução:* No menu do *free space*, utilizou-se a opção *Add GPT Partition*, definindo o campo *Format* para *Leave unformatted*. A criação prévia dessa partição bruta ativou imediatamente o menu de criação do Volume Group (`ubuntu-vg`).

* **Problema 3: Erro de desmontagem ao reiniciar a VM (`failed unmounting /cdrom`)**
  * *Sintoma:* Durante o processo final de reinicialização (`Reboot Now`), o sistema apresentou uma falha no console ao tentar desmontar o drive de CD-ROM.
  * *Solução:* A mensagem é um comportamento comum no VirtualBox devido à ejetação automática da mídia virtual. O problema foi resolvido pressionando a tecla `Enter` no terminal e efetuando uma reinicialização forçada da máquina virtual (`Ctrl + R`).

---

## 7. Conclusão
Esta prática permitiu entender de forma concreta o funcionamento de um ambiente virtualizado sob um hipervisor de Tipo 2 e a importância do isolamento de recursos do hardware físico. A experiência com a instalação do Ubuntu Server destacou a relevância do Logical Volume Manager (LVM) em ambientes de produção, onde a flexibilidade para redimensionar partições e volumes em disco é fundamental para a administração de redes e servidores. Além disso, a habilitacao nativa do OpenSSH garante a infraestrutura necessária para a gestão remota por linha de comando em etapas futuras do curso.


# Relatório Técnico - Aula Prática 02: Administração de Usuários, Grupos e Permissões no Linux

## 1. Identificação
* **Nome do Aluno:** [Seu Nome Completo]
* **Curso:** Bacharelado em Sistemas de Informação (BSI)
* **Turma:** [Sua Turma / Período]
* **Data de Execução:** 25/08/2026
* **Título da Prática:** Administração de Usuários, Grupos e Permissões no Ubuntu Server 26.04 LTS

---

## 2. Objetivo
Capacitar a administração de contas de usuários, criação e gestão de grupos de trabalho e a definição de políticas de acesso no Linux. A prática aplicou a segregação de funções em diretórios compartilhados (`/srv/projeto` e `/srv/financeiro`) utilizando controle de posse (`chown` e `chgrp`), permissões octais (`chmod 770` e `660`) e validação via troca de contexto com `su -`.

---

## 3. Ambiente
* **Hospedeiro (Host):** Windows 11 Pro
* **Hipervisor:** Oracle VM VirtualBox
* **Sistema Convidado (VM):** Ubuntu Server 26.04 LTS
* **Usuário Administrador:** `administrador` (com privilégios `sudo`)

---

## 4. Procedimento
1. **Criação das Contas de Usuário:**
   * Execução do comando `sudo adduser` para criar as contas: `fulano`, `cicrano`, `beltrano` e `novato`.
2. **Criação e Gestão de Grupos:**
   * Criação do grupo `devs` (`sudo groupadd devs`) e associação dos usuários `fulano`, `cicrano` e `beltrano` (`sudo usermod -aG devs <usuario>`).
   * Criação do grupo `financeiro` (`sudo groupadd financeiro`) e associação dos usuários `cicrano` e `beltrano`.
3. **Configuração da Pasta /srv/projeto:**
   * Criação do diretório garantindo a árvore de caminhos: `sudo mkdir -p /srv/projeto`.
   * Ajuste de posse para o dono `administrador` e grupo `devs`: `sudo chown administrador /srv/projeto` e `sudo chgrp devs /srv/projeto`.
   * Aplicação de permissão octal restritiva: `sudo chmod 770 /srv/projeto` (`drwxrwx---`).
   * Criação do arquivo interno via redirecionamento seguro: `echo "Especificacao tecnica do roteador de borda" | sudo tee /srv/projeto/config_redes.txt`.
   * Ajuste de propriedade e permissão do arquivo: `sudo chown administrador:devs /srv/projeto/config_redes.txt` e `sudo chmod 660 /srv/projeto/config_redes.txt` (`-rw-rw----`).
4. **Exercício de Fixação (/srv/financeiro):**
   * Criação do diretório: `sudo mkdir -p /srv/financeiro`.
   * Definição de posse para o grupo financeiro: `sudo chown administrador:financeiro /srv/financeiro`.
   * Aplicação da permissão octal: `sudo chmod 770 /srv/financeiro`.
   * Criação do arquivo `relatorio.txt` com permissão `660` para acesso exclusivo do grupo `financeiro`.

---

## 5. Testes e Evidências
* **Acesso do Grupo Devs (`fulano` em `/srv/projeto`):**
  * Comando: `su - fulano` -> `cd /srv/projeto` -> `echo "Revisado" >> config_redes.txt`
  * *Resultado:* Leitura e escrita efetuadas com sucesso.
* **Bloqueio de Usuário Sem Acesso (`novato` em `/srv/projeto`):**
  * Comando: `su - novato` -> `cd /srv/projeto`
  * *Resultado:* Retorno imediato de `Permission denied`.
* **Acesso do Grupo Financeiro (`cicrano` em `/srv/financeiro`):**
  * Comando: `su - cicrano` -> `cd /srv/financeiro` -> `cat relatorio.txt`
  * *Resultado:* Acesso permitido com sucesso.
* **Isolamento entre Setores (`fulano` em `/srv/financeiro`):**
  * Comando: `su - fulano` -> `cd /srv/financeiro`
  * *Resultado:* Retorno de `Permission denied`, confirmando o isolamento entre departamentos.

---

## 6. Problemas e Soluções
* **Problema 1: Erro `No such file or directory` ao criar arquivos com `tee` ou alterar posse com `chown`**
  * *Causa:* Tentativa de criar arquivos ou aplicar permissões em caminhos cujos diretórios pai ainda não haviam sido criados no sistema.
  * *Solução:* Execução prévia do comando `sudo mkdir -p <caminho>` para garantir a existência dos diretórios antes da criação dos arquivos.
* **Problema 2: Impossibilidade de colar texto diretamente do Windows para a VM no VirtualBox**
  * *Causa:* Ausência de interface gráfica no Ubuntu Server para a área de transferência compartilhada padrão.
  * *Solução:* Utilização do atalho `Ctrl Direito + V` (Host+V) no VirtualBox para simulação de digitação ou acesso remoto via SSH no PowerShell do Windows.
* **Problema 3: Conflito de permissão ou herança de ambiente durante testes entre usuários**
  * *Causa:* Uso do comando `su usuario` sem o hífen, mantendo o diretório atual e variáveis do usuário anterior.
  * *Solução:* Adoção do comando `su - usuario` com o hífen, forçando o carregamento de uma *login shell* limpa e isolada no diretório home da conta testada.

---

## 7. Conclusão
A prática permitiu vivenciar a aplicação do princípio do menor privilégio em sistemas Linux. A combinação da criação de grupos de trabalho com permissões octais bem estruturadas (`770` para diretórios e `660` para arquivos) provou ser um método eficaz para garantir a colaboração interna entre membros de uma mesma equipe, mantendo o isolamento rigoroso contra acessos não autorizados de outros setores.
