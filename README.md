# Laboratório de Força Bruta com Medusa e Kali Linux

## 1. Sobre o Projeto

Este laboratório foi realizado em ambiente controlado e autorizado, com foco em testes de segurança em serviços vulneráveis e autenticação. O objetivo foi demonstrar o uso de ferramentas de reconhecimento, automação e validação em um cenário de estudo e documentação técnica.

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
- Documentar o laboratório para utilização como portfólio.

## 3. Ambiente do Laboratório

| Item | Detalhes |
|---|---|
| Máquina atacante |  |
| Sistema operacional |  |
| Ferramentas |  |
| Máquina alvo |  |
| Sistema operacional |  |
| Serviços analisados |  |
| Tipo de rede |  |
| Virtualizador |  |

[INSERIR EVIDÊNCIA: configuração das máquinas virtuais/rede no VirtualBox]

## 4. Topologia da Rede

A arquitetura foi montada em rede isolada para permitir testes controlados. O Kali Linux atua como máquina atacante, enquanto o Metasploitable 2 e, se aplicável, o DVWA funcionam como ambientes vulneráveis.

```text
Kali Linux
IP: <KALI_IP>
        |
        | Rede host-only / isolada
        |
Metasploitable 2
IP: <TARGET_IP>
```

[INSERIR EVIDÊNCIA: configuração da rede]

## 5. Ferramentas Utilizadas

| Ferramenta | Finalidade |
|---|---|
| Kali Linux | Ambiente atacante |
| Nmap | Reconhecimento de portas e serviços |
| Medusa | Força bruta e password spraying |
| FTP | Serviço analisado |
| SMB | Serviço analisado |
| DVWA | Aplicação web vulnerável para testes |
| VirtualBox | Virtualização do laboratório |
| Git/GitHub | Versionamento e portfólio |

## 6. Reconhecimento do Ambiente

### 6.1 Identificação do endereço IP

Foi realizado o reconhecimento inicial para identificar os endereços IP das máquinas do laboratório e confirmar a conectividade.

```bash
[COMANDO UTILIZADO]
```

[INSERIR EVIDÊNCIA: IP da máquina atacante]

[INSERIR EVIDÊNCIA: IP da máquina alvo]

### 6.2 Varredura com Nmap

A varredura foi realizada para identificar portas abertas e serviços em execução.

```bash
[COMANDO NMAP UTILIZADO]
```

| Porta | Serviço | Estado | Observação |
|---|---|---|---|
|  |  |  |  |

[INSERIR EVIDÊNCIA: resultado do Nmap]

## 7. Ataque de Força Bruta contra FTP

O FTP é um protocolo de transferência de arquivos que utiliza autenticação por usuário e senha. Quando configurado de forma inadequada, pode ser alvo de ataques automatizados.

### 7.1 Wordlist

A lista de usuários e senhas foi utilizada conforme a execução do laboratório.

```text
[CONTEÚDO DA WORDLIST OU COMANDO UTILIZADO]
```

[INSERIR EVIDÊNCIA: wordlist]

### 7.2 Execução do Medusa

```bash
[COMANDO MEDUSA UTILIZADO]
```

Explicação dos parâmetros:
- `-h`: host/endpoint do alvo;
- `-U`: arquivo com usuários;
- `-P`: arquivo com senhas;
- `-M ftp`: módulo FTP do Medusa;
- demais parâmetros conforme execução real.

[INSERIR EVIDÊNCIA: execução do Medusa]

### 7.3 Validação da credencial

Após a identificação de possível credencial válida, foi realizada validação em ambiente controlado.

```bash
[COMANDO UTILIZADO PARA VALIDAÇÃO]
```

[INSERIR EVIDÊNCIA: acesso FTP validado]

### 7.4 Resultado

| Serviço | Usuário | Resultado | Observação |
|---|---|---|---|
| FTP |  |  |  |

## 8. Password Spraying e SMB

A diferença entre as técnicas é:

- Brute force: múltiplas tentativas para um único usuário;
- Password spraying: uma mesma senha em vários usuários;
- Credential stuffing: reutilização de credenciais vazadas.

### 8.1 Enumeração de usuários

Objetivo: identificar contas válidas antes do teste de senha.

```bash
[COMANDO UTILIZADO]
```

[INSERIR EVIDÊNCIA: usuários identificados]

### 8.2 Password Spraying

```bash
[COMANDO MEDUSA UTILIZADO]
```

[INSERIR EVIDÊNCIA: execução do teste]

### 8.3 Validação

| Usuário | Resultado | Observação |
|---|---|---|
|  |  |  |

[INSERIR EVIDÊNCIA: acesso SMB validado]

## 9. Ataque Automatizado contra DVWA

O DVWA é uma aplicação web vulnerável, utilizada em ambientes de estudo para simular autenticação e testes de segurança em aplicações web.

### 9.1 Configuração

Documentar:
- URL utilizada;
- nível de segurança configurado;
- parâmetros relevantes.

[INSERIR EVIDÊNCIA: DVWA funcionando]

### 9.2 Teste de autenticação

```bash
[COMANDO OU PROCEDIMENTO UTILIZADO]
```

[INSERIR EVIDÊNCIA: teste no DVWA]

### 9.3 Resultado

| Item | Descrição |
|---|---|
| Quantidade de tentativas |  |
| Comportamento da aplicação |  |
| Credencial válida encontrada |  |
| Limitações observadas |  |

## 10. Resultados Gerais

| Cenário | Serviço | Técnica | Resultado |
|---|---|---|---|
| 1 | FTP | Brute Force |  |
| 2 | SMB | Password Spraying |  |
| 3 | DVWA | Tentativas automatizadas |  |

## 11. Análise de Segurança

Principais riscos observados no laboratório:

- senhas fracas;
- ausência de bloqueio de conta;
- ausência de rate limiting;
- serviços expostos;
- enumeração de usuários;
- ausência de monitoramento;
- falta de políticas de hardening;
- risco de acesso indevido em caso de credenciais válidas.

## 12. Medidas de Mitigação

- Aplicar políticas fortes de senha;
- Ativar bloqueio após múltiplas falhas;
- Implementar rate limiting;
- Limitar serviços expostos;
- Reduzir enumeração de usuários;
- Usar MFA quando possível;
- Monitorar logs e acessos;
- Realizar hardening de FTP/SMB;
- Revisar permissões e controles de acesso;
- Validar aplicações web em ambientes controlados.

## 13. Considerações Finais

Este laboratório foi conduzido em ambiente autorizado e isolado, com foco em aprendizado técnico e documentação para portfólio. Os registros, comandos e evidências devem refletir a execução real do ambiente, sem inventar resultados, IPs ou credenciais.

## 14. Checklist de Entrega

- [ ] Ambiente configurado;
- [ ] IPs registrados;
- [ ] Nmap executado;
- [ ] FTP testado;
- [ ] SMB testado;
- [ ] DVWA validado, se utilizado;
- [ ] Evidências inseridas;
- [ ] Resultados conferidos;
- [ ] README pronto para portfólio.

## 15. Observações

Este documento foi preparado para ser utilizado como entrega de desafio e como projeto de portfólio GitHub. Os marcadores de evidência devem ser preenchidos com imagens reais ou saídas de comando reais após a execução do laboratório.
