
**Identificazione delle interfacce di rete della VM**

lanciando il comando:

`ip a`

ottengo visione dei interfacce di rete, attraverso cui la macchina si connette a internet, in questo caso

`┌─[lorenzo@parrot]─[~]`  
`└──╼ $ip a`  
`1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000`  
`link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00`  
`inet 127.0.0.1/8 scope host lo`  
`valid_lft forever preferred_lft forever`  
`inet6 ::1/128 scope host noprefixroute`  
`valid_lft forever preferred_lft forever`  
`2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000`  
`link/ether 08:00:27:16:e5:88 brd ff:ff:ff:ff:ff:ff`  
`altname enx08002716e588`  
`inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3`  
`valid_lft 86381sec preferred_lft 86381sec`  
`inet6 fd17:625c:f037:2:db52:2b05:5624:7801/64 scope global dynamic noprefixroute`  
`valid_lft 86383sec preferred_lft 14383sec`  
`inet6 fe80::5e81:84f8:56ea:a4fa/64 scope link noprefixroute`  
`valid_lft forever preferred_lft forever`  
`3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000`  
`link/ether 08:00:27:35:91:30 brd ff:ff:ff:ff:ff:ff`  
`altname enx080027359130`  
`inet 192.168.56.101/24 brd 192.168.56.255 scope global dynamic noprefixroute enp0s8`  
`valid_lft 581sec preferred_lft 581sec`  
`inet6 fe80::85fe:6b94:5216:2954/64 scope link noprefixroute`  
`valid_lft forever preferred_lft forever`

vedo quindi tre interfacce con internet, le due previste per la connessione a internet e alla NAT che vogliamo creare con le altre macchine. inoltre è presente una interfaccia 
" lo " che non riconosco

**non sono riuscito a trovare le iso delle macchine bersaglio, mi ricordo di aver svolto il laboratorio in aula quindi includilo come svolto negli appunti, esegui tu il codice e analizza comunque con commenti le risposte**

come funziona l'assegnazione delle porte ai servizi in esecuzione su un host? 
cosa significa che un servizio sta in esecuzione su un host? fammi un esempio

comando: nmap

| Flag  | Tipo              | Come funziona                                                        |
| ----- | ----------------- | -------------------------------------------------------------------- |
| `-sT` | TCP connect       | Completa il three-way handshake — visibile nei log                   |
| `-sS` | SYN scan          | Manda solo SYN, non completa — più stealth                           |
| `-sV` | Version detection | Banner grabbing: legge la stringa che il servizio invia all'apertura |
| `-O`  | OS detection      | Fingerprinting del SO                                                |
| `-p-` | All ports         | Scansiona tutte le 65535 porte (default: solo le 1000 più comuni)    |
il come funziona è troppo ristretto, cos'è un threeway handshake, cosa significa visibile nei log? quali log?, cos'è SYN? che stringa invia il servizio all'apertura? apertura significa avvio? a chi lo invia? in che senso identifico il SO attraverso una porta di rete? e poi SO di chi? come funziona l'analisi di tutte le porte? 

non rispondo neanche alle domande, alcune sulle meccaniche di Vbox o riferite alla kill chain o VA/PT sono semplici, le altre richiedono della teoria che non è chiara nel documento. serve un approccio diverso per questa materia, inizia dalla costruzione di questi appunti e poi definiamo l'impostazione del corso