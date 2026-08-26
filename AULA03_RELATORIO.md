# Relatório Técnico - Aula Prática 03: Padrão FHS, Navegação e Isolamento Avançado de Diretórios Departamentais

## 1. Identificação
* **Nome do Aluno:** Arthur Soares Silva
* **Curso:** Bacharelado em Sistemas de Informação (BSI)
* **Turma:** Período 9
* **Data de Execução:** 26/08/2026
* **Título da Prática:** Padrão FHS, Navegação e Isolamento Avançado de Diretórios Departamentais

---

## 2. Objetivo
Aprender a explorar a árvore padrão de diretórios do Linux (Filesystem Hierarchy Standard - FHS), compreender a função de suas pastas estruturais e implementar um ambiente de diretórios departamentais isolados (`/srv/ti-dept`, `/srv/vendas-dept` e `/srv/diretoria-dept`) utilizando herança recursiva, propriedades de grupo e permissões octais rígidas (`770` e `660`).

---

## 3. Ambiente
* **Hospedeiro (Host):** Windows 11 Pro
* **Hipervisor:** Oracle VM VirtualBox
* **Sistema Convidado (VM):** Ubuntu Server 26.04 LTS
* **Diretório Base da Prática:** `/srv` (Filesystem Hierarchy Standard)

---

## 4. Procedimento
1. **Inspeção do Sistema FHS:** Navegação em `/etc` (configurações) e leitura dos logs de autenticação em `/var/log/auth.log` com `sudo tail`.
2. **Criação Recursiva de Estruturas:** Utilização do comando `mkdir -p` para criar pastas e subpastas departamentais em uma única operação.
3. **Gestão de Grupos e Usuários:**
   * Criação dos grupos `ti-group`, `vendas-group` e `diretoria-group`.
   * Vinculação dos usuários com `usermod -aG`: `fulano` em `ti-group`, `cicrano` em `vendas-group` e `beltrano` em `diretoria-group`.
4. **Aplicação de Isolamento Recursivo:**
   * Definição de propriedade dos diretórios usando `chown -R administrador:<grupo> /srv/<diretório>`.
   * Restrição absoluta de acesso a terceiros via `chmod -R 770`, garantindo permissões `drwxrwx---`.
5. **Criação e Proteção de Arquivos Internos:**
   * Criação dos arquivos `arquitetura_rede_vpn.txt` e `orcamento_ti.txt`.
   * Ajuste de permissões em formato octal `660` (`-rw-rw----`) e posse vinculada aos respectivos grupos departamentais.

---

## 5. Testes e Evidências
* **Validação de TI (`fulano`):** Sucesso na navegação em `/srv/ti-dept` e visualização do subdiretório `projetos/`.
* **Bloqueio Interdepartamental (`cicrano` tentou acessar TI):** O sistema retornou `-bash: cd: /srv/ti-dept: Permission denied`.
* **Validação do Desafio - Diretoria (`beltrano`):** Acesso liberado à pasta `/srv/diretoria-dept` e leitura do arquivo `orcamento_ti.txt`.
* **Bloqueio do Desafio - Diretoria (`fulano` tentou acessar Diretoria):** Retorno imediato de `Permission denied`.

![Inspeção FHS Parte 1](<img-exe3(img1).png>)
![Inspeção FHS Parte 2](<img-exe3(img2).png>)
![Estrutura Departamental e Permissões](<img-exe3(img3).png>)

---

## 6. Problemas e Soluções
* **Problema: Erro de negação de acesso ou falsos positivos ao testar permissões trocando de usuário no mesmo diretório**
  * *Causa:* Uso do comando `su usuario` sem o hífen, o que preserva o caminho de trabalho e variáveis do usuário anterior.
  * *Solução:* Uso estrito do comando `su - usuario` com o hífen. Isso força a inicialização de uma *login shell* limpa, direcionando o terminal para a `/home` do usuário testado.

---

## 7. Conclusão
Esta prática consolidou a importância do padrão FHS para a organização do sistema e demonstrou a eficiência do controle de acesso em servidores corporativos. A aplicação do modelo octal `770` para pastas e `660` para arquivos garante que apenas usuários explicitamente associados aos seus grupos de trabalho tenham acesso aos dados, mantendo o isolamento rígido e a segurança de dados confidenciais entre departamentos.
