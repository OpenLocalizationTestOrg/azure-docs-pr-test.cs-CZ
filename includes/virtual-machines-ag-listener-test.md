V tomto kroku můžete otestovat hello naslouchací proces skupiny dostupnosti pomocí klientské aplikace, která běží na hello stejné síti.

Připojení klientů má hello následující požadavky:

* Naslouchací proces toohello připojení klienta musí pocházet z počítačů, které se nacházejí v jiné cloudové službě než hello jeden, hostitelé hello repliky dostupnosti Always On.
* Pokud hello Always On repliky jsou v různých podsítích, musí klienti zadat *MultisubnetFailover = True* hello připojovacího řetězce. Výsledkem podmínku paralelní připojení pokusí tooreplicas v hello různých podsítí. Tento scénář zahrnuje nasazení mezi oblastmi vždy na dostupnosti skupiny.

Jedním z příkladů je naslouchací proces toohello tooconnect z jednoho z hello virtuálních počítačů v hello stejné virtuální síti Azure (ale není ten, který je hostitelem repliky). Snadný způsob toocomplete tento test je tootry tooconnect SQL Server Management Studio toohello naslouchací proces skupiny dostupnosti. Jiné jednoduše je toorun [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx), a to takto:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> Pokud je hodnota EndpointPort hello *1433*, nejsou požadované toospecify v hello volání. Hello předchozího volání také předpokládá, že hello klientský počítač je připojený k toohello stejné doméně a že hello volající má uděleno oprávnění v databázi hello pomocí ověřování systému Windows.
> 
> 

Při testování naslouchací proces hello být jisti toofail přes hello dostupnosti skupiny toomake jistotu, že klienti můžou připojit naslouchací proces toohello napříč převzetí služeb při selhání.

