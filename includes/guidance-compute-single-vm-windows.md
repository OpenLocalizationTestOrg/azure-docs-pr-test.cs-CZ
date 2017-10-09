Tento článek popisuje sadu osvědčené postupy pro spuštění virtuálního počítače (VM) s Windows v Azure, platícího tooscalability pozornost, dostupnosti, možnosti správy a zabezpečení.

> [!NOTE]
> Azure má dva různé modely nasazení: [Azure Resource Manager] [ resource-manager-overview] a classic. Tento článek je zaměřený na Resource Manager, který společnost Microsoft doporučuje pro nová nasazení.
>
>

Nedoporučujeme používat pro nejdůležitější úlohy jenom jeden virtuální počítač, protože tak vzniká jediný bod selhání. Pro zajištění vyšší dostupnosti nasaďte několik virtuálních počítačů ve [skupině dostupnosti][availability-set]. Další informace najdete v tématu [Spuštění několika virtuálních počítačů v Azure][multi-vm].

## <a name="architecture-diagram"></a>Diagram architektury

Zřizování virtuálních počítačů v Azure zahrnuje více přesunutí částí než právě hello virtuální počítač. Existuje prvků výpočty, síť a úložiště.

> Je k dispozici ke stažení hello dokumentů aplikace Visio, která obsahuje tento diagram architektury [webu Microsoft download center][visio-download]. Tento diagram je na hello "Výpočetní – jeden virtuální počítač" stránky.
>
>

![[0]][0]

* **Skupina prostředků:** [*Skupina prostředků*][resource-manager-overview] je kontejner, který obsahuje související prostředky. Vytvořte prostředek skupiny toohold hello prostředky pro tento virtuální počítač.
* **Virtuální počítač:** Můžete zřídit virtuální počítač ze seznamu publikované Image nebo ze souboru virtuálního pevného disku (VHD), můžete nahrávat na server tooAzure úložiště objektů Blob.
* **Disk s operačním systémem:** disk Hello operačního systému je virtuální pevný disk uložený v [Azure Storage][azure-storage]. To znamená, že zůstává i v případě, že hello hostitelský počítač přestane fungovat.
* **Dočasný disk:** Hello virtuální počítač je vytvořen s dočasným diskovým (hello `D:` jednotky v systému Windows). Tento disk je uložený na fyzický disk na hostitelském počítači hello. *Neukládá se* v Azure Storage a při restartování nebo jiných událostech životního cyklu virtuálního počítače může dojít k jeho odstranění. Tento disk používejte jenom pro dočasná data, jako jsou stránkovací nebo odkládací soubory.
* **Datové disky:** [Datový disk][data-disk] je trvalý virtuální pevný disk, který se používá pro data aplikací. Datové disky jsou uložené ve službě Azure Storage jako disk hello operačního systému.
* **Virtuální síť a podsíť:** Každý virtuální počítač v Azure je nasazený do virtuální sítě, která se dál dělí na podsítě.
* **Veřejná IP adresa:** Veřejná IP adresa je potřebné toocommunicate s hello virtuálních počítačů&mdash;například prostřednictvím vzdálené plochy (RDP).
* **Síťové rozhraní (NIC):** Hello síťový adaptér umožňuje hello toocommunicate virtuálních počítačů s hello virtuální sítě.
* **Skupina zabezpečení sítě (NSG):** Hello [NSG] [ nsg] je použité tooallow nebo odepřít toohello podsíť provozu sítě. NSG můžete přidružit k jednotlivým rozhraním NIC nebo k podsíti. Pokud ji přidružit k podsíti, platí pravidla NSG hello tooall virtuálních počítačů v této podsíti.
* **Diagnostika:** Protokolování diagnostiky je zásadní pro správu a řešení potíží s hello virtuálních počítačů.

## <a name="recommendations"></a>Doporučení

Hello následující doporučení platí pro většinu scénářů. Pokud nemáte konkrétní požadavek, který by těmto doporučením nedopovídal, postupujte podle nich.

### <a name="vm-recommendations"></a>Doporučení pro virtuální počítače

Azure nabízí mnoho velikosti jiný virtuální počítač, ale doporučujeme hello DS - a GS-series, protože tyto počítače velikosti podporovat [Storage úrovně Premium][premium-storage]. Pokud nemáte speciální úlohy, jako je třeba vysokovýkonné výpočetní prostředí (HPC), vyberte jednu z těchto velikosti počítačů. Podrobnosti najdete v tématu věnovaném [velikostem virtuálních počítačů][virtual-machine-sizes].

Pokud přecházíte existující tooAzure zatížení, začněte s hello velikost virtuálního počítače, který je nejblíže tooyour shodu hello místní servery. Potom měr hello výkon vašich skutečné úloh s respektují tooCPU, paměti a disku vstupně výstupních operací za sekundu (IOPS) a upravte velikost hello v případě potřeby. Pokud budete potřebovat několik síťových adaptérů pro virtuální počítač, uvědomte si, že hello maximální počet síťových adaptérů je funkce hello [velikost virtuálního počítače][vm-size-tables].   

Při zřizování hello virtuálních počítačů a dalším prostředkům, musíte zadat oblast. Obecně platí, vyberte v oblasti nejbližší tooyour interní uživatele nebo zákazníků. Ne všechny velikosti virtuálních počítačů však může být k dispozici ve všech oblastech. Podrobnosti najdete v tématu [služby podle oblasti][services-by-region]. toosee seznam hello velikosti virtuálních počítačů k dispozici v dané oblasti, spusťte následující příkaz rozhraní příkazového řádku Azure (CLI) hello:

```
azure vm sizes --location <location>
```

Informace o výběru publikované image virtuálního počítače najdete v tématu [vyhledání a vyberte bitových kopií systému Windows virtuálního počítače v Azure pomocí prostředí Powershell nebo rozhraní příkazového řádku][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Doporučení pro disk a úložiště

Pro nejlepší výkon diskových operací, doporučujeme, abyste [Storage úrovně Premium][premium-storage], která ukládá data na jednotky SSD (Solid-State Drive). Náklady na vychází hello velikost zřízeného disku hello. IOPS a propustnost a také závisí na velikosti disku, takže při zřizování disk, vezměte v úvahu všechny tři faktory (kapacitu, IOPS a propustnost).

Vytvořte účty samostatné úložiště Azure pro každý virtuální počítač toohold hello virtuální pevné disky (VHD) v pořadí tooavoid nedosáhli limitů hello IOPS pro účty úložiště.

Přidejte jeden nebo několik datových disků. Když vytvoříte nový virtuální pevný disk, je neformátovaný tvar. Přihlaste se k disku hello tooformat Virtuálního hello. Pokud máte velký počet datových disků, pamatujte na hello celkový počet vstupně-výstupních operací limity účtu úložiště hello. Další informace najdete v tématu věnovaném [omezením disku virtuálního počítače][vm-disk-limits].

Pokud je to možné, instalovat aplikace datový disk, hello disk operačního systému. Některé starší aplikace však může být zapotřebí tooinstall součásti na jednotku C: hello. V takovém případě můžete [Změna velikosti disku hello OS] [ resize-os-disk] pomocí prostředí PowerShell.

Pro nejlepší výkon vytvořte účet samostatného úložiště toohold diagnostické protokoly. Pro diagnostické protokoly stačí standardní účet místně redundantního úložiště (LRS).

### <a name="network-recommendations"></a>Doporučení pro síť

Hello veřejná IP adresa může být dynamická nebo statická. Výchozí hodnota Hello je dynamická.

* Rezerva [statickou IP adresu] [ static-ip] Pokud potřebujete pevnou IP adresu, která nedojde ke změně &mdash; například pokud potřebujete toocreate A záznam v DNS, nebo třeba hello bezpečné seznamu IP adres toobe přidané tooa.
* Můžete také vytvořit plně kvalifikovaný název domény (FQDN) pro hello IP adresu. Poté můžete zaregistrovat [záznam CNAME] [ cname-record] ve službě DNS, který odkazuje toohello plně kvalifikovaný název domény. Další informace najdete v tématu [vytvořit platný plně kvalifikovaný název domény v hello portál Azure][fqdn].

Každá skupina zabezpečení sítě obsahuje sadu [výchozích pravidel][nsg-default-rules], včetně pravidla, které blokuje veškerou příchozí internetovou komunikaci. nelze odstranit Hello výchozí pravidla, ale ostatní pravidla mohou nepřepíšete. tooenable přenosy z Internetu, vytvořit pravidla, která povolí příchozí komunikaci toospecific porty &mdash; například port 80 pro protokol HTTP.  

tooenable protokolu RDP, přidejte pravidlo NSG, které umožní příchozí přenos tooTCP portu 3389.

## <a name="scalability-considerations"></a>Aspekty zabezpečení

Virtuální počítač můžete škálovat nahoru nebo dolů podle [Změna velikosti virtuálních počítačů hello](../articles/virtual-machines/windows/sizes.md). tooscale out uvést vodorovně, dvě nebo více virtuálních počítačů do za službou Vyrovnávání zatížení sadu dostupnosti. Podrobnosti najdete v tématu [spuštění několika virtuálních počítačů v Azure pro škálovatelnost a dostupnost][multi-vm].

## <a name="availability-considerations"></a>Aspekty dostupnosti

Pro zajištění vyšší dostupnosti nasaďte několik virtuálních počítačů ve skupině dostupnosti. To také poskytuje vyššího [smlouva o úrovni] [ vm-sla] (SLA).

Váš virtuální počítač může ovlivňovat [plánovaná][planned-maintenance] nebo [neplánovaná údržba][manage-vm-availability]. Můžete použít [virtuální počítač restartovat protokoly] [ reboot-logs] toodetermine, jestli virtuální počítač restartovat bylo způsobeno plánované údržby.

Disky VHD se ukládají v [úložišti Azure][azure-storage] a úložiště Azure se z důvodů zajištění dostupnosti a odolnosti replikuje.

tooprotect proti před náhodnou ztrátou dat během normálních operací (například z důvodu chyby uživatele), měli byste také implementovat v okamžiku zálohování, pomocí [blob snímky] [ blob-snapshot] nebo jiný nástroj.

## <a name="manageability-considerations"></a>Aspekty správy

**Skupiny prostředků:** Uveďte úzce párované prostředky hello této sdílené složky stejný životní cyklus do hello stejné [skupiny prostředků][resource-manager-overview]. Skupiny prostředků umožňují toodeploy a monitorování prostředky jako skupinu a souhrnné fakturace náklady skupinou prostředků. Prostředky můžete také odstranit jako sadu, což je velmi užitečné pro testovací nasazení. Dejte prostředkům smysluplné názvy. Který umožňuje jednodušší toolocate konkrétní prostředek a pochopit jeho role. Další informace najdete v tématu [Doporučené zásady vytváření názvů pro prostředky Azure][naming conventions].

**Diagnostika virtuálních počítačů:** Povolte monitorování a diagnostiku, včetně základních metrik stavu, diagnostických protokolů infrastruktury a [diagnostiky spouštění][boot-diagnostics]. Diagnostika spouštění můžete diagnostikovat chyby spouštění, jestli virtuální počítač získá do nonbootable stavu. Další informace najdete v tématu [Povolení monitorování a diagnostiky][enable-monitoring]. Použití hello [shromáždění protokolů Azure] [ log-collector] toocollect rozšíření platformy Azure protokoly a k jejich nahrávání tooAzure úložiště.   

Následující příkaz rozhraní příkazového řádku Hello umožňují diagnostiku:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Zastavení virtuálního počítače:** Azure rozlišuje mezi tím, když je virtuální počítač zastavený a když má zrušené přidělení. Budou se účtovat při zastavení hello stav virtuálního počítače, ale ne v případě, že je deallocated hello virtuálních počítačů.

Použijte hello následující rozhraní příkazového řádku příkaz toodeallocate virtuálního počítače:

```
azure vm deallocate <resource-group> <vm-name>
```

V hello portálu Azure, hello **Zastavit** tlačítko zruší přidělení hello virtuálních počítačů. Ale pokud vypnete prostřednictvím hello operačního systému během přihlášení, hello virtuálního počítače je zastavena, ale *není* navrácena, takže vám bude dál účtovat poplatek.

**Odstranění virtuálního počítače:** Při odstranění virtuálního počítače, virtuální pevné disky hello nebudou odstraněny. To znamená, že hello virtuálních počítačů bez ztráty dat můžete bezpečně odstranit. Bude se vám ale účtovat poplatek za úložiště. toodelete hello virtuálního pevného disku, odstraňte soubor hello z [úložiště objektů Blob][blob-storage].

tooprevent náhodným odstraněním, použití [prostředek zámku] [ resource-lock] toolock hello celý zdroj skupiny nebo zámku jednotlivé prostředky, například hello virtuálních počítačů.

## <a name="security-considerations"></a>Aspekty zabezpečení

Použití [Azure Security Center] [ security-center] tooget centrální zobrazení hello stav zabezpečení vašich prostředků Azure. Security Center sledovat potenciální potíže se zabezpečením a poskytuje ucelený přehled o stavu zabezpečení hello vašeho nasazení. Security Center je nakonfigurován za předplatné Azure. Povolit zabezpečení shromažďování dat, jak je popsáno v [použití služby Security Center]. Při shromažďování dat je povolené, Security Center automaticky vyhledá všechny virtuální počítače vytvořené v rámci tohoto předplatného.

**Opravy správy.** Pokud je povoleno, Security Center ověří, zda jsou chybějící aktualizace zabezpečení a důležité aktualizace. Použití [nastavení zásad skupiny] [ group-policy] na aktualizace automatické systému tooenable hello virtuálních počítačů.

**Proti malwaru.** Pokud je povoleno, Security Center kontroluje, zda je nainstalován software proti malwaru. Můžete taky Security Center tooinstall antimalwarový software z uvnitř hello portálu Azure.

**Operace.** Použití [řízení přístupu na základě role] [ rbac] (RBAC) toocontrol přístup toohello Azure prostředky, které nasadíte. RBAC umožňuje přiřadit toomembers autorizace rolí DevOps týmu. Role čtenáře hello nelze například zobrazit prostředky Azure, ale vytvořit, spravovat nebo odstranit. Některé role jsou tooparticular konkrétní typy prostředků Azure. Například role Přispěvatel virtuálních počítačů hello můžete restartovat nebo zrušit přidělení virtuálního počítače, resetovat heslo správce hello, vytvořte nový virtuální počítač a tak dále. Mezi další [předdefinované role RBAC][rbac-roles], které by mohly být pro tuto referenční architekturu užitečné, patří role [Uživatel služby DevTest Labs][rbac-devtest] a [Přispěvatel sítě][rbac-network]. Uživatele můžete přiřadit role toomultiple a můžete vytvořit vlastní role pro i další jemně odstupňovaná oprávnění.

> [!NOTE]
> RBAC neomezuje hello akce, které může uživatel přihlášen virtuálních počítačů provést. Tato oprávnění jsou určeny podle typu účtu hello na hello hostovaný operační systém.   
>
>

heslo místního správce tooreset hello spustit hello `vm reset-access` příkazu příkazového řádku Azure CLI.

```
azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Použití [protokoly auditu] [ audit-logs] toosee zřizování akce a další události, virtuálních počítačů.

**Šifrování dat.** Vezměte v úvahu [Azure Disk Encryption] [ disk-encryption] Pokud potřebujete tooencrypt hello operačního systému a datové disky.

## <a name="solution-deployment"></a>Nasazení řešení

Pro tuto referenční architekturu nasazení je k dispozici na [Githubu][github-folder]. Obsahuje virtuální síť, NSG a jeden virtuální počítač. toodeploy hello architektura, postupujte takto:

1. Klikněte pravým tlačítkem níže uvedené tlačítko hello a vyberte buď "otevřete odkaz na nové kartě" nebo "Otevřete odkaz v novém okně."  
   [![Nasazení tooAzure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. Jakmile hello odkaz otevře ve hello portálu Azure, je nutné zadat hodnoty pro některá z nastavení hello:

   * Hello **skupiny prostředků** název je již definován v souboru parametrů hello, takže vyberte **vytvořit nový** a zadejte `ra-single-vm-rg` hello textového pole.
   * Vyberte hello oblast z hello **umístění** rozevíracího pole.
   * Neupravujte hello **šablony kořenové Uri** nebo hello **parametr kořenové Uri** textových polí.
   * Vyberte **windows** v hello **typ operačního systému** rozevíracího pole.
   * Zkontrolujte hello podmínky a ujednání a pak klikněte na hello **souhlasím toohello podmínky a ujednání, které jsou uvedené výše** zaškrtávací políčko.
   * Klikněte na hello **nákupu** tlačítko.
3. Počkejte toocomplete nasazení hello.
4. soubory parametrů Hello zahrnout pevně správce uživatelské jméno a heslo, a důrazně doporučujeme okamžitě změnit obě. Klikněte na hello virtuálního počítače s názvem `ra-single-vm0 `v hello portálu Azure. Potom klikněte na **resetovat heslo** v hello **podpory a řešení potíží s** okno. Vyberte **resetovat heslo** v hello **režimu** rozevírací pole, pak vyberte nový **uživatelské jméno** a **heslo**. Klikněte na tlačítko hello **aktualizace** tlačítko toopersist hello nové uživatelské jméno a heslo.

Informace o další způsoby toodeploy tuto referenční architekturu, naleznete v souboru readme hello v hello [pokyny single-vm][github-folder]] Githubu složky.

## <a name="customize-hello-deployment"></a>Přizpůsobení hello nasazení
Pokud potřebujete toochange hello nasazení toomatch vašim potřebám, postupujte podle pokynů hello v hello [readme][github-folder].

## <a name="next-steps"></a>Další kroky
Pro zajištění vyšší dostupnosti nasaďte dva nebo víc virtuálních počítačů za nástrojem pro vyrovnávání zatížení. Další informace najdete v tématu [Spuštění několika virtuálních počítačů v Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]:../articles/virtual-machines/windows/tutorial-availability-sets.md
[azure-cli]: /cli/azure/get-started-with-az-cli2
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/storage/storage-about-disks-and-vhds-windows.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]:../articles/virtual-machines/windows/portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]:../articles/virtual-machines/windows/manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]:../articles/virtual-machines/windows/planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-labs-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]:../articles/virtual-machines/windows/expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]:../articles/virtual-machines/windows/cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-account-limits]: ../articles/azure-subscription-service-limits.md#storage-limits
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[použití služby Security Center]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/windows/sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]:../articles/virtual-machines/linux/change-vm-size.md
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[vm-size-tables]: ../articles/virtual-machines/windows/sizes.md
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architektura jednoho virtuálního počítače s Windows v Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
