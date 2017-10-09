


Tento článek se zaměřuje na některé časté dotazy, které uživatelé požádat o Azure virtuální počítače vytvořené pomocí modelu nasazení classic hello.

## <a name="can-i-migrate-my-vm-created-in-hello-classic-deployment-model-toohello-new-resource-manager-model"></a>Můžete migrovat virtuální počítač vytvořené v hello modelu nasazení classic modelu toohello nové Resource Manager?
Ano. Návod, jak toomigrate, najdete v části:

* [Migrace z klasického tooAzure správce prostředků pomocí Azure PowerShell](../articles/virtual-machines/windows/migration-classic-resource-manager-ps.md).
* [Migrace z klasického tooAzure pomocí rozhraní příkazového řádku Azure Resource Manager](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Co můžu spouštět na virtuálním počítači Azure?
Všichni předplatitelé můžou na virtuálním počítači Azure spouštět serverový software. Můžete spouštět nejnovější verze Windows Serveru i celou řadu linuxových distribucí. Podrobnosti o podpoře najdete tady:

• Virtuální počítače s Windows – [Podpora serverového softwaru Microsoft pro virtuální počítače Azure](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• Virtuální počítače s Linuxem – [Linux v distribucích schválených pro Azure](http://go.microsoft.com/fwlink/p/?LinkId=393551)

Některé verze systému Windows 7 a Windows 8.1 klienta bitových kopií systému Windows, jsou k dispozici tooMSDN Azure benefit odběratele a odběratelé MSDN vývoj a testování průběžné platby pro vývoj a testování úlohy. Podrobnosti, včetně pokynů a omezení, najdete v tématu [Image klienta Windows pro předplatitele MSDN](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/).

## <a name="why-are-affinity-groups-being-deprecated"></a>Proč se skupiny vztahů přestávají používat jako zastaralé?
Skupiny vztahů jsou starším konceptem geografického seskupování zákaznických nasazení cloudových služeb a účtů úložiště v rámci Azure. Poskytnou byly původně výkon sítě virtuálních počítačů VM tooimprove do hello časná návrhů síť Azure. Také podporovaly hello počáteční verzi virtuální sítě (virtuální sítě), které byly omezené tooa malého hardwaru v oblasti.

Hello aktuální síť Azure v rámci oblasti je navržené tak, aby se už nevyžadují skupiny vztahů. Virtuální sítě navíc mají regionální rozsah, takže skupina vztahů už není při používání virtuální sítě potřeba. Z důvodu vylepšení toothese už doporučujeme zákazníkům použít skupiny vztahů vzhledem k tomu může být omezení v některých scénářích. Pomocí skupiny vztahů zbytečně přidružíte váš hardware toospecific virtuální počítače, který omezuje hello volba velikostí virtuálních počítačů, které jsou k dispozici tooyou. Chyby související s toocapacity ho může vést také když se pokusíte tooadd nové virtuální počítače, pokud konkrétní hardware hello přidružené skupiny vztahů hello je téměř kapacitu.

Skupina vztahů funkce nepoužívají už v modelu nasazení Azure Resource Manager hello a hello portálu Azure. Pro hello klasický portál Azure jsme se podpora pro vytváření skupin vztahů a vytváření prostředků úložiště, které jsou skupiny vztahů definovaného tooan místo začne. Nepotřebujete toomodify existující cloudové služby, které používají skupiny vztahů. Pro nové cloudové služby byste však skupiny vztahů používat neměli, pokud to nedoporučí specialista podpory Azure.

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Kolik úložiště můžu využít s virtuálním počítačem?
Každý datový disk může být až too1 TB. Hello počet datových disků, které můžete použít závisí na velikosti hello hello virtuálního počítače. Podrobnosti najdete v článku [Velikosti služeb Virtual Machines](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Účet úložiště Azure poskytuje úložiště pro disk operačního systému hello a všechny datové disky. Každý disk je soubor .vhd uložený jako objekt blob stránky. Podrobnosti o cenách najdete v tématu [Podrobnosti o cenách úložiště](http://go.microsoft.com/fwlink/p/?LinkId=396819).

## <a name="which-virtual-hard-disk-types-can-i-use"></a>Jaké typy virtuálních pevných disků můžu použít?
Azure podporuje pouze virtuální pevné disky s pevnou velikostí a ve formátu VHD. Pokud je disk VHDX, které chcete toouse v Azure, je nutné toofirst převést pomocí Správce technologie Hyper-V nebo hello [convert-VHD](http://go.microsoft.com/fwlink/p/?LinkId=393656) rutiny. Poté, co můžete udělat, použijte [přidat AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) rutiny (v režimu správy služby) tooupload hello virtuálního pevného disku tooa účet úložiště v Azure, abyste je mohli používat s virtuálními počítači.

* Linux pokyny najdete v tématu [vytvoření a nahrání virtuálního pevného disku tohoto obsahuje hello operační systém Linux](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
* Windows pokyny najdete v tématu [vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="are-these-virtual-machines-hello-same-as-hyper-v-virtual-machines"></a>Jsou stejné jako virtuální počítače Hyper-V, zda že text hello tyto virtuální počítače?
V mnoha způsoby, které jsou podobné příliš virtuálních počítačů Hyper-V "Generace 1", ale máte hello přesně stejné. Oba typy zadejte virtualizovaného hardwaru a hello formátu VHD virtuální pevné disky jsou kompatibilní. To znamená, že je můžete přesouvat mezi Hyper-V a Azure. Tři hlavní rozdíly, které někdy překvapí uživatele Hyper-V, jsou:

* Azure neposkytuje konzoly přístup tooa virtuálního počítače. Neexistuje žádný způsob, jak tooaccess virtuální počítač, až poté, co spouštění.
* Virtuální počítače Azure ve většině [velikostí](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mají pouze 1 virtuální síťový adaptér, což znamená, že také můžou mít pouze 1 externí IP adresu. (hello A8 a A9 velikosti použít druhý síťový adaptér pro komunikaci s aplikací mezi instancemi v omezené scénáře.)
* Virtuální počítače Azure nepodporují funkce 2. generace virtuálních počítačů Hyper-V. Podrobnosti o těchto funkcích najdete v tématech [Specifikace virtuálních počítačů pro Hyper-V](http://technet.microsoft.com/library/dn592184.aspx) a [Přehled virtuálních počítačů 2. generace](https://technet.microsoft.com/library/dn282285.aspx).

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>Můžou tyto virtuální počítače používat moji stávající místní síťovou infrastrukturu?
Pro virtuální počítače vytvořené v modelu nasazení classic hello můžete použít existující infrastrukturu tooextend Azure Virtual Network. Hello přístup je jako nastavení pobočky. Vám může zřizovat a spravovat virtuální privátní sítě (VPN) v Azure, stejně jako zabezpečené připojení je tooon místní infrastrukturu IT. Podrobnosti najdete v článku [Přehled služby Virtual Network](../articles/virtual-network/virtual-networks-overview.md).

Budete potřebovat toospecify hello sítě, který chcete hello toowhen toobelong virtuální počítač vytvoříte hello virtuální počítač. Nemůže připojit k existující virtuální síť tooa virtuálního počítače. Můžete však tento problém vyřešit odpojování hello virtuální pevný disk (VHD) ze hello existujícího virtuálního počítače a použít jej toocreate nový virtuální počítač s hello síťové konfigurace, které chcete.

## <a name="how-can-i-access--my-virtual-machine"></a>Jak můžu přistupovat k virtuálnímu počítači?
Je nutné tooestablish toolog vzdáleného připojení na toohello virtuálního počítače pomocí připojení ke vzdálené ploše pro virtuální počítač s Windows nebo Secure Shell (SSH) pro virtuální počítač s Linuxem. Pokyny najdete tady:

* [Jak tooLog na tooa virtuální počítač spuštěný Windows Server](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Jsou podporované maximálně 2 souběžných připojení, pokud je hello server nakonfigurovaný jako hostitel relace vzdálené plochy.  
* [Jak tooLog na tooa Linux spuštěný virtuální počítač](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). SSH ve výchozím nastavení umožňuje maximálně 10 souběžných připojení. Toto číslo můžete zvýšit úpravou hello konfigurační soubor.

Pokud máte problémy s vzdálené plochy nebo SSH, nainstalovat a používat hello [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) rozšíření toohelp oprava hello problém.

Pro virtuální počítače s Windows další možnosti zahrnují:

* V hello portál Azure classic, najde hello virtuálního počítače a pak klikněte na tlačítko **resetovat vzdálený přístup** z příkazového řádku hello.
* Zkontrolujte [tooa připojení řešení Vzdálená plocha systému Windows virtuálního počítače Azure](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Použít vzdálenou komunikaci Windows PowerShell tooconnect toohello virtuálních počítačů, nebo vytvořit další koncové body pro ostatní toohello tooconnect prostředky virtuálních počítačů. Podrobnosti najdete v tématu [jak tooSet až koncové body tooa virtuální počítač](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Pokud jste obeznámeni s technologií Hyper-V, může být díváte pro podobné tooVMConnect nástroje. Azure nepodporuje poskytují podobné nástroj, protože konzole přístup tooa virtuálního počítače není podporován.

## <a name="can-i-use-hello-temporary-disk-hello-d-drive-for-windows-or-devsdb1-for-linux-toostore-data"></a>Můžete použít hello dočasným diskovým (hello D: jednotky pro Windows nebo/dev/sdb1 pro Linux) toostore dat?
Neměli byste používat data toostore hello dočasné disku (hello D: jednotky ve výchozím nastavení pro Windows nebo/dev/sdb1 pro Linux). Jsou to pouze dočasná úložiště, takže byste riskovali nenávratnou ztrátu dat. Tato situace může nastat, pokud se přesune virtuální počítač hello tooa jiného hostitele. Změna velikosti virtuálního počítače, aktualizace hello hostitele nebo selhání hardwaru na hostiteli hello jsou některé z důvodů hello, které může přesunout virtuální počítač.

## <a name="how-can-i-change-hello-drive-letter-of-hello-temporary-disk"></a>Jak můžete změnit písmeno jednotky hello hello dočasné disku?
Na virtuálním počítači Windows můžete změnit písmeno jednotky hello přesunutí souborů stránku hello a nové přiřazení písmena jednotek, ale budete potřebovat toomake se, že že Hello kroky v určitém pořadí. Pokyny najdete v tématu [změnit písmeno jednotky hello dočasné disku Windows hello](../articles/virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="how-can-i-upgrade-hello-guest-operating-system"></a>Jak můžete upgradovat hello hostovaného operačního systému?
Hello termín upgrade obvykle znamená přesunutí tooa více nejnovější verzi operačního systému, při zachování hello stejného hardwaru. Pro virtuální počítače Azure, hello proces pro přesun tooa novější verze se liší pro Linux a Windows:

* U virtuálních počítačů Linux pomocí nástroje pro správu balíček hello a postupy, které jsou vhodné pro distribuci hello.
* U virtuálního počítače s Windows je nutné server hello toomigrate pomocí něčeho jako jazyk hello nástrojů pro migraci systému Windows Server. Nepokoušejte tooupgrade hello hostovaný operační systém, když se nachází v Azure. Z důvodu hello riziko ztráty přístupu toohello virtuálního počítače není podporováno. Pokud během upgradu hello dochází k potížím, můžete mohlo dojít ke ztrátě toostart hello možnost vzdálené plochy relace a nebude možné tootroubleshoot hello problémy.

Obecné informace o nástrojích hello a procesy pro migraci Windows serveru najdete v tématu [tooWindows migrace rolí a funkcí serveru](http://go.microsoft.com/fwlink/p/?LinkId=396940).

## <a name="whats-hello-default-user-name-and-password-on-hello-virtual-machine"></a>Co je hello výchozí uživatelské jméno a heslo na hello virtuálního počítače?
Hello Image poskytovaný Azure nemáte, předem nakonfigurovaná uživatelské jméno a heslo. Když vytvoříte virtuální počítač pomocí jedné z těchto obrázků, budete potřebovat tooprovide uživatelského jména a hesla, které budete používat toolog toohello virtuálního počítače.

Pokud jste zapomněli hello uživatelské jméno nebo heslo a jste nainstalovali hello agenta virtuálního počítače, můžete nainstalovat a používat hello [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) rozšíření toofix hello problém.

Další podrobnosti:

* Pro Image Linux hello Pokud používáte hello portál Azure classic, 'azureuser' je zadána jako výchozí uživatelské jméno, ale toto můžete změnit pomocí 'Galerie z' místo 'Vytvořit' jako hello způsob toocreate hello virtuální počítač. Pomocí z Galerie také umožňuje při rozhodování, zda toouse heslo, klíčem SSH nebo oba toolog v. Hello uživatelský účet je bez oprávnění uživatele, který má přístup "sudo" toorun privilegovaný příkazy. Hello "kořenový" účet je zakázán.
* Pro bitové kopie systému Windows budete potřebovat tooprovide uživatelské jméno a heslo při vytváření hello virtuálních počítačů. Hello účet je přidán toohello skupiny Administrators.

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Může Azure na mém virtuálním počítači spouštět antivirové produkty?
Azure nabízí několik možností pro řešení antivirové, ale je tooyou toomanage ho. Například můžete potřebovat samostatné předplatné pro antimalwarový software a toodecide budete potřebovat, když toorun kontroly a nainstalovat aktualizace. Podporu antivirových produktů můžete přidat při vytváření virtuálního počítače s Windows nebo později pomocí rozšíření virtuálního počítače pro Microsoft Antimalware, Symantec Endpoint Protection nebo agenta TrendMicro Deep Security Agent. Hello Symantec a TrendMicro rozšíření umožňují používat bezplatné předplatné zkušební verze časově omezenou nebo existující podnikové předplatné. Microsoft Antimalware je zdarma. Podrobnosti najdete tady:

* [Jak tooinstall a konfigurace Symantec Endpoint Protection na virtuální počítač Azure](http://go.microsoft.com/fwlink/p/?LinkId=404207)
* [Jak tooinstall a nakonfigurujte Trend Micro hluboké Security jako služba na virtuální počítač Azure](http://go.microsoft.com/fwlink/p/?LinkId=404206)
* [Nasazování antimalwarových řešení na virtuálních počítačích Azure](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## <a name="what-are-my-options-for-backup-and-recovery"></a>Jaké mám možnosti zálohování a obnovení?
V určitých oblastech je dostupná služba Azure Backup jako verze Preview. Podrobnosti najdete v článku [Zálohování virtuálních počítačů Azure](../articles/backup/backup-azure-vms.md). Další řešení jsou k dispozici od certifikovaných partnerů. toofind, co je aktuálně k dispozici, vyhledejte hello Azure Marketplace.

Další možnost je toouse hello snímku možnosti úložiště objektů blob. toodo, budete potřebovat tooshut dolů hello virtuálních počítačů před všechny operace, které jsou závislé na snímku objektů blob. To uloží čekající datové zápisy a vloží hello systému souborů v konzistentním stavu.

## <a name="how-does-azure-charge-for-my-vm"></a>Jak se v Azure účtují virtuální počítače?
Azure účtuje hodinovou cenu na základě velikosti hello Virtuálního počítače a operačního systému. V případě neúplných hodin Azure účtuje pouze jako hello minuty. Pokud vytvoříte hello virtuálního počítače s určitým předem nainstalovaného softwaru obsahující image virtuálního počítače, další hodinové softwaru poplatky. Azure poplatky odděleně pro úložiště pro operační systém a datové disky hello Virtuálního počítače. Dočasné diskové úložiště je zdarma.

Budou se vám účtovat když hello stav virtuálního počítače je spuštěná nebo zastaveno, ale vám není účtován při zastavení hello stav virtuálního počítače (zrušit přidělení). tooput virtuálního počítače v hello (navrátit) stavu Zastaveno, proveďte jednu z následujících hello:

* Vypnutí nebo odstranění hello virtuálních počítačů z hello portál Azure classic.
* Použijte rutinu Stop-AzureVM hello v modulu Azure PowerShell hello.
* Pomocí operace vypnutí Role hello v hello REST API pro správu služby a zadejte StoppedDeallocated pro hello PostShutdownAction element.

Další podrobnosti najdete v článku [Ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Bude Azure restartovat můj virtuální počítač kvůli údržbě?
Azure někdy restartuje virtuální počítač jako součást aktualizace regulární, plánované údržby v hello datových centrech Azure.

K neplánovaným událostem údržby může docházet, když Azure zjistí závažný problém s hardwarem, který má vliv na váš virtuální počítač. Pro neplánované události Azure automaticky migruje pořádku hostitele tooa hello virtuálních počítačů a restartuje hello virtuálních počítačů.

Pro všechny samostatnou virtuálního počítače (tj. hello virtuální počítač není součástí skupiny dostupnosti), Azure upozorní správce služeb hello odběru e-mailem alespoň jeden týden před plánovanou údržbu protože hello virtuálních počítačů může být restartován během aktualizace hello. Aplikace běžící na virtuálních počítačích hello může dojít k výpadku.

Také můžete použít hello klasický portál Azure nebo Azure PowerShell tooview hello restartování protokoly při restartování hello došlo k chybě z důvodu údržby tooplanned. Podrobnosti najdete v tématu [Zobrazení protokolů restartování virtuálního počítače](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/).

redundance tooprovide put dva nebo více podobně nakonfigurované virtuální počítače v hello stejné skupině dostupnosti. Tím pomůžete zajistit, že během plánované nebo neplánované údržby bude dostupný alespoň jeden virtuální počítač. Azure pro tuto konfiguraci zaručuje určité úrovně dostupnosti virtuálních počítačů. Podrobnosti najdete v tématu [spravovat hello dostupnosti virtuálních počítačů](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="additional-resources"></a>Další zdroje
[Informace o virtuálních počítačích Azure](../articles/virtual-machines/virtual-machines-linux-about.md)

[Vytvořit a spravovat virtuální počítače Linux s hello rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Vytvoření a správa virtuálních počítačů s Windows pomocí Azure PowerShellu](../articles/virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

