Virtuální počítače Azure (VM) může někdy restartuje bez zjevné příčiny, aniž by váš s iniciované hello operace restartování. V tomto článku jsou uvedené akce hello a události, které může způsobit tooreboot virtuálních počítačů a poskytuje přehled o tom, jak tooavoid neočekávané restartovat problémy nebo snížení vlivu hello tyto problémy.

## <a name="configure-hello-vms-for-high-availability"></a>Konfigurace hello virtuálních počítačů pro vysokou dostupnost
Hello nejlepší způsob, jak tooprotect aplikace, která běží v Azure pro virtuální počítač se restartuje a výpadky je tooconfigure hello virtuálních počítačů pro vysokou dostupnost.

Tato úroveň redundance tooyour aplikace tooprovide, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby, je k dispozici alespoň jeden virtuální počítač a splňuje hello 99,95 % [smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Další informace o nastavení dostupnosti najdete v části hello následující články:

- [Spravovat hello dostupnosti virtuálních počítačů.](../articles/virtual-machines/windows/manage-availability.md)
- [Konfigurace dostupnosti virtuálních počítačů.](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Informace o stavu prostředků 
Azure Resource Health je služba, která zveřejňuje hello stav jednotlivých prostředků Azure a poskytuje řešitelné pokyny při řešení problémů. V cloudovém prostředí, kde není možné toodirectly servery nebo přístupu prvky infrastruktury hello cílem stav prostředku je tooreduce hello čas strávený řešení potíží. Konkrétně hello cílem je tooreduce hello čas strávený určení, zda kořenový hello hello problému spočívá v hello aplikaci nebo v události uvnitř hello platformy Azure. Další informace najdete v tématu [Rady pro pochopení a používání prostředků stavu](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-hello-vm-tooreboot"></a>Akce a události, které můžou způsobit tooreboot hello virtuálních počítačů

### <a name="planned-maintenance"></a>Plánovaná údržba
Microsoft Azure pravidelně provádí aktualizace napříč hello zeměkouli tooimprove hello spolehlivosti, výkonu a zabezpečení infrastruktury hello hostitele, který je základem virtuálních počítačů. Mnoho z těchto aktualizací, včetně zachování paměti aktualizací, se provádí bez dopady na virtuální počítače nebo cloudové služby.

Ale některé aktualizace vyžadují restartování. V takových případech hello virtuální počítače se ukončuje, zatímco jsme oprava hello infrastruktury, a pak se restartuje hello virtuálních počítačů.

toounderstand je co Azure plánované údržby a jak může ovlivnit dostupnost hello vaše virtuální počítače s Linuxem, najdete v článcích hello tady. Hello články poskytují informace o procesu hello Azure plánované údržby a jak tooschedule plánované údržby toofurther snížit dopad hello.

- [Plánovaná údržba virtuálních počítačů v Azure](../articles/virtual-machines/windows/planned-maintenance.md)
- [Jak tooschedule plánované údržby na virtuálních počítačích Azure](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Aktualizace zachovávající paměť   
Pro tuto třídu aktualizací ve službě Microsoft Azure uživatelé nezaznamenají žádný vliv na jejich spuštěné virtuální počítače. Mnoho z těchto aktualizací je toocomponents nebo služby, které lze aktualizovat bez zasahování hello spuštěna instance. Některé jsou platformy aktualizace infrastruktury na hello hostitelský operační systém, který lze použít bez restartování hello virtuálních počítačů.

Tyto aktualizace se zachováním paměti jsou provést pomocí technologie, která umožňuje migraci za provozu na místě. Když se právě aktualizuje, hello virtuální počítač je umístěn v *pozastavena* stavu. Tento stav zachovává hello paměti RAM, zatímco hello potřebné aktualizace a opravy obdrží hello základní hostitelský operační systém. Hello virtuálního počítače je obnoven běh v rámci 30 sekund byla pozastavena. Po hello obnovení virtuálního počítače jeho hodiny automaticky synchronizovat.

Z důvodu hello pozastavení krátké doby nasazení aktualizací díky tomuto mechanismu výrazně snižuje dopad hello na hello virtuálních počítačů. Tímto způsobem lze nasadit, ale ne všechny aktualizace. 

Aktualizace s více instancemi (pro virtuální počítače v nastavení dostupnosti) jsou použité jednu aktualizační doménu najednou.

> [!NOTE]
> Počítače s Linuxem, které mají staré verze jádra jsou ovlivněné jádra paniky během tato metoda aktualizace. tooavoid tento problém, aktualizace tookernel verze 3.10.0-327.10.1 nebo novější. Další informace najdete v tématu [virtuálního počítače Azure Linux na jádro na základě 3.10 panics po upgradu uzlu hostitele](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Restartování nebo vypnutí akce zahájená uživatelem
 
Pokud provádíte restartování z hello portálu Azure, Azure PowerShell, rozhraní příkazového řádku nebo resetovat rozhraní API, můžete najít hello událostí v hello [protokol činnosti Azure](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Pokud provádíte hello akce z operačního systému hello Virtuálního počítače, najdete v protokolech systému hello hello událostí.

Další scénáře, které obvykle způsobí tooreboot hello virtuálního počítače zahrnují více akcí změně konfigurace. Normálně uvidíte, že zpráva s upozorněním, která spouštění konkrétní akce bude mít za následek restartování hello virtuálních počítačů. Mezi příklady patří žádné virtuálního počítače změňte velikost operace, změna hello heslo účtu správce hello a nastavení statické IP adresy.

### <a name="azure-security-center-and-windows-update"></a>Azure Security Center a aktualizace systému Windows
Azure Security Center monitoruje denní Windows a virtuální počítače s Linuxem pro chybějící aktualizace operačního systému. Security Center načte seznam dostupných zabezpečení a důležité aktualizace ze služby Windows Update nebo Windows Server Update Services (WSUS), podle toho, která je nakonfigurovaná služba virtuální počítač s Windows. Security Center také zkontroluje hello nejnovější aktualizace pro systémy Linux. Pokud virtuální počítač je chybějící aktualizace systému, Security Center doporučuje použít aktualizace systému. aplikace Hello těchto aktualizací systému je řízen pomocí hello Security Center v hello portálu Azure. Po instalaci některé aktualizace, může být vyžadováno restartování virtuálního počítače. Další informace najdete v tématu [aktualizace systému v Azure Security Center](../articles/security-center/security-center-apply-system-updates.md).

Jako na místní servery Azure není push aktualizace ze služby Windows Update tooWindows virtuálních počítačích Azure, protože určený toobe spravuje jejich uživatelé jsou tyto počítače. Jste však podporovat tooleave hello automatické nastavení Windows Update povolena. Automatická instalace aktualizací ze služby Windows Update může také způsobit toooccur restartování počítače po instalaci aktualizací hello. Další informace najdete v tématu [nejčastější dotazy týkající se Windows Update](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-hello-availability-of-your-vm"></a>Další situace, které mají vliv na dostupnost hello virtuálního počítače
Existují další případy, ve kterých může Azure aktivně pozastavit hello použití virtuálního počítače. Obdržíte e-mailová oznámení před provedením této akce, takže budete mít možnost tooresolve, hello základní problémy. Mezi příklady problémy, které mít vliv na dostupnost virtuálního počítače patří narušení zabezpečení a hello vypršení platnosti způsoby platby.

### <a name="host-server-faults"></a>Hostitel serveru chyb 
Hello virtuálního počítače je hostován na fyzickém serveru, který běží v datovém centru Azure. Hello fyzický server běží na agenta volá hello Agent hostitele v tooa přidání několika Azure komponent. Když se tyto Azure softwarové komponenty na fyzickém serveru hello přestat reagovat, hello monitorování systému aktivuje restartování hello hostitele serveru tooattempt obnovení. Hello virtuálních počítačů je obvykle dostupná znovu za pět minut a pokračuje toolive na hello stejně jako dříve hostitele.

Server chyby jsou obvykle způsobeno selhání hardwaru, například selhání hello na pevný disk nebo jednotku SSD. Azure nepřetržitě monitoruje tyto události, identifikuje základní chyby hello a vydává aktualizace po zmírnění hello je implementována a otestovat.

Protože některé chyby serveru hostitele může být server konkrétní toothat, opakované situaci restartování virtuálního počítače může vylepšené pomocí ručně opětovného nasazení serveru hostitele tooanother hello virtuálního počítače. Tato operace se aktivuje pomocí hello **znovu nasaďte** možnost na stránce Podrobnosti hello hello virtuálního počítače nebo zastavením a restartováním hello virtuálních počítačů v hello portálu Azure.

### <a name="auto-recovery"></a>Automatické obnovení
Pokud z nějakého důvodu nelze restartovat hostitelský server hello, zahájí hello platformy Azure automatického obnovení akce tootake hello vadný hostitelského serveru mimo otočení dále prozkoumat. 

Všechny virtuální počítače na tomto hostiteli jsou automaticky přemístěné tooa v pořádku, jiný hostitelský server. Tento proces je obvykle dokončení do 15 minut. Další informace o procesu automatického obnovení hello toolearn najdete v části [automatického obnovení virtuálních počítačů](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Neplánovaná Údržba
Ve výjimečných případech hello Azure provozní tým může potřebovat tooperform údržby aktivity tooensure hello celkového stavu hello platformy Azure. Toto chování může mít vliv na dostupnost virtuálního počítače a obvykle znamená hello stejné akce automatického obnovení, jak je popsáno výše.  

Neplánované maintenances zahrnout hello následující:

- Defragmentace naléhavé uzlu
- Aktualizace přepínač naléhavé sítě

### <a name="vm-crashes"></a>Dojde k chybě virtuálního počítače
Virtuální počítače může restartovat kvůli problémům v rámci hello virtuální počítač. Hello úlohy nebo role, která běží na hello virtuálních počítačů může spustit kontrolu chyb v rámci hello hostovaného operačního systému. Pro pomoc při rozhodování, hello důvod hello havárií zobrazit protokoly hello systému a aplikací pro virtuální počítače Windows a hello sériové protokoly pro virtuální počítače s Linuxem.

### <a name="storage-related-forced-shutdowns"></a>Související s úložištěm vynucené vypnutí
Virtuální počítače v Azure využívají virtuální disky pro operační systém a úložiště dat, který je hostován na infrastrukturu Azure Storage hello. Vždy, když je pro více než 120 sekundách vliv hello dostupnosti nebo připojení mezi hello virtuálních počítačů a hello přidružené virtuální disky, provede hello platformy Azure vynucené vypnutí hello poškození dat tooavoid virtuálních počítačů. virtuální počítače Hello jsou automaticky zapnutý zpět po obnovení připojení úložiště. 

Hello trvání hello vypnutí může být co nejkratší pět minut, ale mohou být podstatně delší. Následující Hello je jedním z hello konkrétní případů, které souvisí s související s úložištěm vynucené vypnutí: 

**Překročení vstupně-výstupní operace omezení**

Virtuální počítače může dočasně vypnout, při vstupně-výstupní požadavky jsou omezené konzistentně, protože přesahuje omezení hello vstupně-výstupních operací pro hello disk hello objem vstupně-výstupních operací za sekundu (IOPS). (Úložiště – standardní disk je omezená too500 IOPS.) toomitigate tento problém používat prokládání disků nebo můžete nakonfigurovat prostor úložiště hello uvnitř hello hosta virtuálního počítače, v závislosti na zatížení hello. Podrobnosti najdete v tématu [konfigurace virtuálních počítačů Azure pro optimální výkon úložiště](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Vyšší limity IOPS jsou k dispozici prostřednictvím Azure Premium Storage s až too80 000 IOPS. Další informace najdete v tématu [vysoce výkonné úložiště Premium](../articles/storage/common/storage-premium-storage.md).

### <a name="other-incidents"></a>Jiné incidenty
Ve výjimečných případech může ovlivnit problém s rozšířeným více serverů v datovém centru Azure. Pokud k tomuto problému dochází, hello tým Azure odešle e-mailové odběry oznámení toohello vliv. Můžete zkontrolovat hello [řídicí panel stavu služby Azure](https://azure.microsoft.com/status/) a hello portál Azure pro hello stav probíhající výpadky a za incidenty.
