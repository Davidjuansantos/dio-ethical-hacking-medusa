# Auditoria de Segurança e Testes de Autenticação com Kali Linux e Medusa

## 📌 Descrição do Projeto
Este projeto foi desenvolvido como parte do Desafio prático da formação **English4Tech / Cyber Security** da **DIO (Digital Innovation One)**. 

O objetivo principal é documentar a metodologia de análise de resiliência de credenciais em ambientes controlados, simulando testes de auditoria de autenticação contra serviços legados/vulneráveis (como FTP e SMB) e interfaces Web em um laboratório isolado usando **Kali Linux** e a máquina de testes **Metasploitable 2**.

---

## 🛠️ Arquitetura do Laboratório

A infraestrutura foi configurada no **VirtualBox** utilizando redes isoladas para garantir a segurança dos testes:

* **Atacante (Auditor):** Kali Linux (IP na rede Host-Only/Interna)
* **Alvo (Testbed):** Metasploitable 2 (IP na rede Host-Only/Interna)
* **Modo de Rede:** Host-Only (Rede Exclusiva de Hospedeiro) para prevenir tráfego externo acidental.

---

## 📋 Mapeamento e Metodologia de Testes

### 1. Preparação de Wordlists
Para simular cenários reais de senhas fracas e reutilização de credenciais, foram criados arquivos de teste direcionados:
* `users.txt`: Contendo nomes de usuários comuns em sistemas corporativos e padrões de fábrica.
* `passwords.txt`: Contendo dicionários curtos de senhas fracas para validação de políticas.

### 2. Enumeração de Serviços
Antes dos testes de autenticação, o serviço alvo é verificado quanto a portas abertas e banners de versão (ex: serviço FTP na porta 21 ou SMB nas portas 139/445) utilizando ferramentas de varredura como o `Nmap`.

### 3. Testes de Autenticação Paralela
A ferramenta **Medusa** foi utilizada para demonstrar a eficiência de ataques de tentativa de autenticação em lote (paralelismo) contra protocolos de rede:
* **Parâmetros chave do Medusa:**
  * `-h`: Especifica o endereço IP do alvo.
  * `-U`: Caminho para a lista de usuários (`users.txt`).
  * `-P`: Caminho para a lista de senhas (`passwords.txt`).
  * `-M`: Módulo do serviço a ser testado (ex: `ftp` ou `smb`).
  * `-F`: Interrompe a execução ao encontrar o primeiro par de credenciais válido.

### 4. Validação de Acesso
A confirmação de credenciais válidas é realizada conectando-se ao serviço por meio de clientes padrão (como o `smbclient` para compartilhamentos SMB ou o cliente `ftp` para transferências de arquivos), verificando os privilégios obtidos.

---

## 🛡️ Medidas de Mitigação e Prevenção

Com base nos testes realizados, as seguintes práticas defensivas devem ser implementadas para mitigar ataques de força bruta e *password spraying*:

1. **Política de Senhas Fortes:** Exigir complexidade (mínimo de 12 caracteres, caracteres especiais, maiúsculas e números) para inviabilizar dicionários simples.
2. **Autenticação Multifator (MFA):** Tornar a posse do segundo fator obrigatória, inviabilizando o acesso mesmo que a senha seja comprometida.
3. **Bloqueio de Conta (Account Lockout):** Configurar limites de tentativas falhas (ex: bloquear por 15 minutos após 5 tentativas incorretas).
4. **Rate Limiting:** Restringir o número de requisições por segundo por IP em interfaces Web e serviços de autenticação.
5. **Segmentação e Desativação de Protocolos Legados:** Desativar versões vulneráveis do protocolo SMB (SMBv1) e priorizar protocolos criptografados (SFTP/SSH em vez de FTP claro).

---

## 💭 Reflexão Crítica: Ferramentas Clássicas vs. Era da IA e MFA

Embora a Inteligência Artificial traga automação avançada e o MFA reduza drasticamente o impacto do vazamento de senhas isoladas, o domínio de ferramentas clássicas como o **Medusa**, **Nmap** e **Hydra** continua fundamental para profissionais de segurança:

* **Compreensão dos Fundamentos:** Entender como os protocolos tratam requisições de autenticação em baixo nível é essencial para configurar regras eficientes de SIEM e IDS/IPS.
* **Sistemas Legados (Legacy Systems):** Em ambientes corporativos reais, nem todos os sistemas legados suportam integração nativa com MFA, tornando a auditoria tradicional de senhas ainda necessária.
* **Geração Inteligente de Wordlists:** A IA hoje auxilia na criação de dicionários contextuais mais precisos, aumentando a eficiência de ferramentas clássicas de auditoria.

---

## ✒️ Autor
* **Nome:** David Juan Garcia Dos Santos
* **LinkedIn:** https://www.linkedin.com/in/davidjuansantos/
