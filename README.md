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

0015-ossec_rules innehåller regler som mest hanterar Wazuh agenten. Ifall en agent startar, när den startar, ifall den blir bortagen. Många av dom är level 3 då dom mest finns för information.
0900-firewall_rules innehöll inte mycket och det var eftersom brandväggar skapar och skickar sina egna loggar. Det finns dock två regler i som handalr om droped event. I fall en brandvägg droppar packet från samma källa 18 gånger inom 45 sekunder så skapas ett level 10 larm.
0475-suricata_rules.xml, här fanns det inte mycket alls men antar att det är för att lägga en grund så man kan skapa egna suricata regler. Innehållet i den här filen sätter mest ID på olika event som t.ex DNs event eller HTTP event.
0250-apache_rules.xml har regler som varnar när vissa filer eller mappar öppnas gällande apache. Even vid en vanlig sårbarhet i Host header som gör att man kan skicka kod till backend via frontend. Det kan vara en form av SQL injection.
0400-openvpn_rules.xml skapar larm gällande openvpn anslutningar. Den skapar events vid anslutningar och ger larm vid TLS auth error.





Del 2: Regelbasserad detektion
 
Steg 6:
Först fick jag installera vim inne i container eftersom den inte hade en texteditor. Sedan la jag till SSH regler local_rules.xml.

Steg 7:
I agentens ossec.conf har jag lagt till en regel i syscheck som kollar ifall /etc/passwd, /etc/crontab eller ssh config så blir det larm och loggar.

Steg 8:

