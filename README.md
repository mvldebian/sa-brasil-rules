# 🇧🇷 SA-Brasil-Rules

Este repositório contém um conjunto de **regras adicionais para o SpamAssassin**, voltadas para o cenário brasileiro.  
O objetivo é melhorar a detecção de SPAM, phishing e malwares comuns em campanhas de e-mail que afetam domínios e usuários do Brasil.

## 📌 O que está incluído

- **regras-brasil.cf** → Regras de corpo, cabeçalho e URI focadas em SPAMs em português e campanhas comuns no Brasil.  
- **domain_blacklist_exim** → Lista de domínios bloqueados para uso no Exim (`blacklist_from`).  
- **freemail_blocklist** → Lista de endereços e domínios abusivos em provedores gratuitos.  
- **ip_blacklist_exim** → Lista de IPs bloqueados para uso no Exim.  
- **sub_domains_block_postfix** → Bloqueio de subdomínios abusivos para Postfix (`check_sender_access`).  

## 🚀 Como usar

### SpamAssassin / Proxmox Mail Gateway
1. Copie os arquivos de regras para o diretório de configuração:
   ```bash
   cd /etc/mail/spamassassin/
   wget -O regras-brasil.cf https://raw.githubusercontent.com/JrZavaschi/sa-brasil-rules/refs/heads/main/regras-brasil.cf
   
2. Inclua no local.cf ou init.pre a referência às regras:
   ```bash
   include /etc/mail/spamassassin/regras-brasil.cf
   
3. Teste a configuração:
   ```bash
   spamassassin --lint

4. Reinicie o serviço:
   ```bash 
   pmgconfig sync
   systemctl restart pmg-smtp-filter
 


Postfix

Para usar os bloqueios de subdomínios:

   smtpd_sender_restrictions =
      check_sender_access hash:/etc/postfix/sub_domains_block_postfix,
      permit_mynetworks,
      reject_non_fqdn_sender,
      reject_unknown_sender_domain,
      permit


## Depois:

postmap /etc/postfix/sub_domains_block_postfix
systemctl reload postfix

## Exim

Inclua os arquivos domain_blacklist_exim e ip_blacklist_exim no seu ACL de verificação de remetente.
Exemplo:

deny senders = /etc/exim/domain_blacklist_exim
deny hosts   = /etc/exim/ip_blacklist_exim

⚠️ Aviso

Algumas regras utilizam listas públicas (URIBL, SURBL, DBL Spamhaus) e podem exigir acesso DNS externo.

Ajuste os scores de acordo com a sua política de SPAM.

Sempre teste em ambiente de homologação antes de usar em produção.

🤝 Contribuindo

Pull Requests são bem-vindos!
Você pode contribuir enviando:

Novas regras adaptadas para o cenário brasileiro

Correções de regex quebradas

Ajustes de score

📄 Licença

Este projeto é disponibilizado sob a licença MIT.
