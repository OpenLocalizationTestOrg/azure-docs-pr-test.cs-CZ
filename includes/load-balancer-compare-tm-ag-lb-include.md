## <a name="load-balancer-differences"></a>Rozdíly mezi nástroji pro vyrovnávání zatížení

Existují různé možnosti toodistribute síťového provozu pomocí Microsoft Azure. Tyto možnosti fungují různě, obsahují jiné sady funkcí a podporují různé scénáře. Lze je používat jednotlivě nebo je kombinovat.

* **Azure nástroj pro vyrovnávání zatížení** pracuje na hello přenosové vrstvy (4 vrstvy v zásobníku odkaz sítě OSI hello). Poskytuje distribuční úrovni síťového provozu mezi instancemi aplikace běžící v hello stejném datovém centru Azure.
* **Aplikační brána** pracuje na aplikační vrstvu hello (7 vrstvy v zásobníku odkaz sítě OSI hello). Jedná jako reverznímu proxy serveru služby, se ukončuje připojení klienta hello a předávání požadavků koncové body tooback-end.
* **Správce provozu** pracuje na hello úrovně DNS.  Používá DNS odpovědí toodirect koncového uživatele provoz koncovými body tooglobally distribuované. Klienti se koncové body toothose potom připojují přímo.

Hello následující tabulka shrnuje funkce hello služeb každý služby:

| Služba | Nástroj pro vyrovnávání zatížení Azure | Application Gateway | Traffic Manager |
| --- | --- | --- | --- |
| Technologie |Úroveň přenosu (Vrstva 4) |Úroveň aplikace (Vrstva 7) |Úroveň DNS |
| Podporované protokoly aplikací |Všechny |HTTP, HTTPS a protokoly WebSocket |Všechny (k monitorování koncového bodu je vyžadován koncový bod HTTP) |
| Koncové body |Virtuální počítače Azure a instance rolí služby Cloud Services. |Všechny interní IP adresy Azure, veřejné internetové IP adresy, virtuální počítače Azure nebo cloudová služba Azure |Virtuální počítače Azure, služba Cloud Services, webové aplikace Azure a externí koncové body. |
| Podpora virtuálních sítí |Lze použít pro internetové i interní (VNet) aplikace. |Lze použít pro internetové i interní (VNet) aplikace. |Podporuje pouze internetové aplikace. |
| Monitorování koncového bodu |Podporováno prostřednictvím sond. |Podporováno prostřednictvím sond. |Podporováno prostřednictvím HTTP/HTTPS GET. |

Azure nástroj pro vyrovnávání zatížení a aplikační brány trasy síťový provoz tooendpoints ale mít jiný využití scénáře toowhich provoz toohandle. Hello následující tabulka pomáhá Principy hello rozdíl mezi Vyrovnávání zatížení dvou hello:

| Typ | Nástroj pro vyrovnávání zatížení Azure | Application Gateway |
| --- | --- | --- |
| Protokoly |UDP/TCP |HTTP, HTTPS a protokoly WebSocket |
| Vyhrazení IP adres |Podporuje se |Nepodporuje se |
| Režim vyrovnávání zatížení |Pětinásobné (zdrojová IP adresa, zdrojový port, cílová IP adresa, cílový port, typ protokolu). |Kruhové dotazování.<br>Směrování na základě adresy URL. |
| Režim vyrovnávání zatížení (zdrojová IP adresa/spřažené relace). |Dvojnásobné (zdrojová IP adresa a cílová IP adresa), trojnásobné (zdrojová IP adresa, cílová IP adresa a port). Můžete škálovat nahoru nebo dolů podle hello počet virtuálních počítačů |Spřažení na základě souborů cookie.<br>Směrování na základě adresy URL. |
| Sondy stavu |Výchozí hodnota: interval sondování – 15 sekund. Vyřazení ze smyčky: po dvou selháních za sebou. Podporuje uživatelem definované sondy. |Interval nečinnosti sondy – 30 sekund. Vyřazena ze smyčky po pěti selháních živého provozu za sebou nebo po jediném selhání sondy v režimu nečinnosti. Podporuje uživatelem definované sondy. |
| Přesměrování zpracování SSL |Nepodporuje se |Podporuje se |
| Směrování na základě adresy URL | Nepodporuje se | Podporuje se|
| Zásady protokolu SSL | Nepodporuje se | Podporuje se|
