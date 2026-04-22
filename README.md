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
Lag till en regel i ossec.conf som sätter en timeout på IP-addresser vid larm.

Steg 9:
Skapat en dashboard med fyra olika visualizations som visar händelser per timme, fördelning av händelser per regelgrupp, top 10 ip addresser med högst antal larm och en heatmap med händelser per timme och i vilken regelkategori händelsen var i.
SCREENSHOT LÄNK HÄR

Steg 10:
Jag simulerade en SSH brute force med hydra attack, gjorde en portscan och ändrade på sshd_config filen. Hydra får connection refused och inga larm skapas. Nmap portscan gav inte heller några tydliga larm. FIM testet gav larm direkt på flera ställen. I FIM recent events visades tid och vilken fil som ändrades. I min dashboard skapades många ändringar på graferna pga av att flera regle grupper aktiverats för att ändra filen.


Del 3: AI-stödd hotdetektion

Steg 11:
Först expoterade jag larm från Wazhu Indexer. Alltså timestamps, rule id, vilken severity osv osv.
Vid tiden det här skrivs fanns det 541 händelser att exportera.

Steg 12:
Skapade ett python script som vars syfte är att hitta avikelser bland larmen. Scriptet gav mig en rapport som ger mig händlser, när dom hände, vilket snitt på severity, unika IP:addresser och ett poängvärde som är baserat på om hur många händelser som var "normala" och "onormala".


Steg 13:
Nästa steg var att skapa ett script som kunde använda datan anomaly_detectorn skapade för att skapa användbara larm.

Steg 14:
Skapade en log som jag kan skicka larmen till. Sedan körde jag en bit python som skickar larm till den loggfilen.
