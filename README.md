# Lab 1 — Centraliserad säkerhetsövervakning

Del 1: Säkerhetsövervakningssystem

Steg 1:
Clona ner wazuh-docker.git version 4.14.4
i Single-node mappen så genererade jag certes med generete-indexer-certs.yml för att kunna använda https på min dashboard.

Steg 2:
Starta upp Wazuh manager, Wazuh Index och Wazuh Dashboard med docker compose.

Steg 3:
Installerade Wazuh Agent på min privata PC. Kopplade den till wazuh managern så att information från min dator finns på dashboarden.

Steg 4:
Verifiering av agentens anslutning till manager och att loggar kommer in.

Steg 5:
Utforskade vilka standardregler som fanns med, hur dom utlöses och vilken severity de har.


Del 2: Regelbasserad detektion
 
Steg 6:
Först fick jag installera vim inne i container eftersom den inte hade en texteditor. Sedan la jag till SSH regler local_rules.xml.

Steg 7:
I agentens ossec.conf har jag lagt till en regel i syscheck som kollar ifall /etc/passwd, /etc/crontab eller ssh config så blir det larm och loggar.

Steg 8:

