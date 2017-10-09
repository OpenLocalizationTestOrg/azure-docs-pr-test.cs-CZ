Dále pokud všechny servery v clusteru hello se systémem Windows Server 2008 R2 nebo Windows Server 2012, ověřte, že oprava hotfix hello [KB2854082](http://support.microsoft.com/kb/2854082) je nainstalován na každém hello na místních serverech nebo virtuálních počítačích Azure, které jsou součástí clusteru hello. Libovolný server nebo virtuální počítač, který je v clusteru hello, ale není ve skupině dostupnosti hello, musí být také tato oprava hotfix nainstalovaná.

V hello relace vzdálené plochy pro každý z uzlů clusteru hello, stáhněte si [KB2854082](http://support.microsoft.com/kb/2854082) tooa místního adresáře. Potom nainstalujte hello opravu hotfix na všech uzlech clusteru postupně. Pokud na uzlu clusteru hello je aktuálně spuštěna Clusterová služba hello, hello server restartuje na konci hello hello instalace opravy hotfix.

> [!WARNING]
> Zastavování služby clusteru hello nebo restartování serveru hello ovlivňuje hello stavu kvora clusteru a hello skupiny dostupnosti a může způsobit vaše toogo clusteru do režimu offline. toomaintain hello vysokou dostupnost clusteru během instalace, ujistěte se, že:
> 
> * Hello clusteru je ve stavu optimální kvora. 
> * Před instalací opravy hotfix hello v každém uzlu, jsou všechny uzly clusteru online.
> * Před instalací opravy hotfix hello na další uzel v clusteru hello, povolit na jednom uzlu, včetně plně restartování serveru hello toocompletion toorun instalace opravy hotfix hello.
> 
> 

