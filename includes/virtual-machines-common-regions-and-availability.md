# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Oblasti a dostupnost pro virtuální počítače v Azure
Azure funguje v několika datových centrech kolem hello, world. Těchto datových centrech jsou seskupené v toogeographic oblasti, která poskytuje flexibilitu při výběru místa toobuild vaší aplikace. Je důležité toounderstand jak a kde se virtuální počítače (VM) fungovat v Azure, společně s vaší možnosti toomaximize výkon, dostupnost a redundance. Tento článek vám poskytne přehled hello dostupnost a redundance funkce Azure.

## <a name="what-are-azure-regions"></a>Co jsou oblasti Azure?
V definovaných zeměpisné oblasti jako "Západní USA", "Severní Evropa" nebo "jihovýchodní Asie, vytvoříte prostředků Azure. Můžete zkontrolovat hello [seznam oblastí a jejich umístění](https://azure.microsoft.com/regions/). V každé oblasti existuje několik datových center tooprovide redundance a dostupnosti. Tento přístup poskytuje flexibilitu při jeho návrhu aplikace toocreate virtuální počítače nejbližší tooyour uživatelů a toomeet žádné právní, dodržování předpisů, a daň.

## <a name="special-azure-regions"></a>Speciální oblasti Azure
Azure má některé speciální oblasti, můžete taky toouse při sestavování se vaše aplikace pro dodržování předpisů nebo právních důvodů. Mezi tyto speciální oblasti patří:

* **USA (Gov) – Iowa** a **USA (Gov) – Virginia**.
  * Fyzická a logická síťově izolovaná instance Azure pro partnery a úřady státní správy USA, která je obsluhovaná prověřenými občany USA. Zahrnuje další certifikace dodržování předpisů, jako je [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) a [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Další informace o službě [Azure Government](https://azure.microsoft.com/features/gov/)
* **Severní Čína** a **Východní Čína**
  * Tyto oblasti jsou k dispozici prostřednictvím jedinečný partnerství mezi společností Microsoft a 21Vianet, při němž Microsoft neudržuje datových centrech hello přímo. Další informace najdete v tématu [Microsoft Azure v Číně](http://www.windowsazure.cn/).
* **Střední Německo** a **Severovýchodní Německo**
  * Tyto oblasti jsou k dispozici prostřednictvím datový model zplnomocněnec které zákaznická data zůstane v Německu pod kontrolou systémů T německých Telekom společnosti, který funguje jako zplnomocněnec němčině data hello.

## <a name="region-pairs"></a>Párování oblastí
Každé oblasti Azure je spárován s jinou oblast v rámci hello stejné geography (například USA, Evropa nebo Asie). Tento přístup umožňuje pro replikaci hello prostředků, jako je například úložiště virtuálního počítače mezi geography, který by měl snížit pravděpodobnost hello přírodní katastrofy, občanského unrest, výpadky napájení nebo fyzické sítě výpadků ovlivňující obou oblastí najednou. Mezi další výhody párování oblastí patří:

* V případě hello širší Azure výpadku, jedné oblasti prioritu mimo každý pár toohelp snížit hello toorestore čas pro aplikace. 
* Plánované aktualizace Azure se nasazuje toopaired oblasti jeden v výpadek toominimize čas a riziko výpadku aplikace.
* Data pokračuje tooreside v rámci hello geography stejné jako jeho páru (s výjimkou Brazílie – jih) pro účely jurisdikce vynucení daň a zákonem.

Mezi příklady párů oblasti patří:

| Primární | Sekundární |
|:--- |:--- |
| Západní USA |Východ USA |
| Severní Evropa |Západní Evropa |
| Jihovýchodní Asie |Východní Asie |

Můžete zobrazit hello úplné [seznam místní páry zde](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

## <a name="feature-availability"></a>Dostupnost funkcí
Některé služby nebo funkce virtuálních počítačů jsou dostupné jenom v některých oblastech, třeba konkrétní velikosti virtuálních počítačů nebo typy úložišť. Existují také některé globální služby Azure, které vás nevyžadují tooselect konkrétní oblasti, jako například [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md), nebo [Azure DNS](../articles/dns/dns-overview.md). tooassist můžete v návrhu prostředí aplikace, můžete zkontrolovat hello [dostupnost Azure services v každé oblasti](https://azure.microsoft.com/regions/#services). 

## <a name="storage-availability"></a>Dostupnost úložiště
Principy a zeměpisných oblastech Azure změní důležité, pokud byste zvážit hello možnosti replikace úložiště k dispozici. V závislosti na typu hello úložiště máte jinou replikační možností.

**Azure Managed Disks**
* Místně redundantní úložiště (LRS)
  * Replikuje data třikrát v rámci hello oblasti, ve které jste vytvořili účet úložiště.

**Disky založené na účtu úložiště**
* Místně redundantní úložiště (LRS)
  * Replikuje data třikrát v rámci hello oblasti, ve které jste vytvořili účet úložiště.
* Zónově redundantní úložiště (ZRS)
  * Replikuje data mezi dvěma toothree zařízení, v jedné oblasti nebo v rámci dvou oblastí třikrát.
* Geograficky redundantní úložiště (GRS)
  * Replikuje vaše data tooa sekundární oblast, která je stovky miles od primární oblasti hello.
* Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)
  * Replikuje tooa sekundární oblasti vaše data, stejně jako u GRS, ale také pak poskytuje přístup jen pro čtení toohello data v hello sekundárního umístění.

Hello následující tabulka poskytuje rychlý přehled o hello rozdíly mezi typy replikace úložiště hello:

| Strategie replikace | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data se replikují napříč různými zařízeními. |Ne |Ano |Ano |Ano |
| Data lze číst ze sekundárního umístění hello a z primárního umístění hello. |Ne |Ne |Ne |Ano |
| Počet kopií dat uchovávaných na samostatných uzlech |3 |3 |6 |6 |

Další informace o [možnostech replikace služby Azure Storage najdete tady](../articles/storage/common/storage-redundancy.md). Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md).

### <a name="storage-costs"></a>Cena za uložení
Ceny se liší v závislosti na typu hello úložiště a dostupnosti, kterou jste vybrali.

**Azure Managed Disks**
* Disky spravované Premium jsou zajišťované Solid-State disků (SSD) a standardní disky spravované jsou zajišťované disky regulární roztočený. Premium a Standard spravované disky budou účtovat podle kapacity hello zřízení pro hello disk.

**Nespravované disky**
* Storage úrovně Premium je zálohovaný Solid-State disků (SSD) a je účtován na základě hello kapacity disku hello.
* Standardní úložiště je zálohovaný díky regulární roztočený disky a je účtován na kapacitu v použití hello základě a potřeby dostupnosti úložiště.
  * Pro RA-GRS je další přenos dat se geografická replikace zdarma pro šířku pásma hello replikace tohoto data tooanother oblast Azure.

V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) o cenách informace o hello různé typy úložišť a možností dostupnosti.

## <a name="availability-sets"></a>Skupiny dostupnosti
Skupina dostupnosti je logické seskupení virtuálních počítačů, které umožňuje Azure toounderstand jak vaše aplikace je sestavena tooprovide redundance a dostupnosti. Doporučujeme, že dvě nebo více virtuálních počítačů jsou vytvořeny v rámci tooprovide sadu dostupnosti pro vysoce dostupné aplikace a toomeet hello [99,95 % smlouva Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Když je jeden virtuální počítač pomocí [Azure Premium Storage](../articles/storage/common/storage-premium-storage.md), hello platí Smlouva Azure SLA pro události neplánované údržby. 

Skupina dostupnosti se skládá ze dvou další seskupení, které chránit proti selhání hardwaru a povolit aktualizace toosafely být použity - poruch domén (FDs) a aktualizace domény (UDs).

![Koncepční kreslení hello aktualizace domény a chyby konfigurace domény](./media/virtual-machines-common-regions-and-availability/ud-fd-configuration.png)

Další informace o tom, jak toomanage hello dostupnost [virtuální počítače s Linuxem](../articles/virtual-machines/linux/manage-availability.md) nebo [virtuálních počítačů Windows](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Domény selhání
Domény selhání je logické skupiny základní hardware, které sdílejí společný zdroj energie a síťového přepínače, podobně jako rack tooa v rámci překážek místní datacentra. Při vytváření virtuálních počítačů v rámci skupiny dostupnosti, hello platformy Azure automaticky rozděluje virtuálních počítačů na těchto domén selhání. Tento přístup omezuje hello dopad potenciální selhání fyzického hardwaru, výpadků sítě nebo přerušení napájení.

### <a name="update-domains"></a>Aktualizační domény
Doména aktualizace je logické skupiny základní hardware, který můžete projít údržby nebo restartovat na hello stejnou dobu. Při vytváření virtuálních počítačů v rámci skupiny dostupnosti, hello platformy Azure automaticky distribuuje virtuální počítače v těchto aktualizaci domény. Tento přístup zajišťuje, že nejméně jedna instance aplikace vždy zůstává spuštěná jako hello platformy Azure zde nevyskytlo periodické údržby. restartuje se Hello pořadí domén aktualizace, která se resetuje nemusí pokračovat postupně během plánované údržby, ale pouze jednu aktualizační doménu najednou.

### <a name="managed-disk-fault-domains"></a>Spravované domény selhání disku
Virtuální počítače, které používají [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md), odpovídají doménám selhání spravovaných disků (pokud se použije spravovaná skupina dostupnosti). Tato zarovnání zajistí, že všechny hello spravované disky připojené tooa virtuálních počítačů jsou v rámci hello jedné doméně selhání spravovaného disku. Ve spravované skupině dostupnosti je možné vytvořit jenom virtuální počítače se spravovanými disky. Hello počet domén selhání spravovaných disků se liší podle oblastí – dvě nebo tři domén selhání spravovaných disků na každou oblast.

![Domény selhání spravovaných disků](./media/virtual-machines-common-manage-availability/md-fd.png)

> [!IMPORTANT]
> Hello počet domén selhání pro skupiny dostupnosti spravované se liší podle oblastí – dva nebo tři každou oblast. Hello následující tabulka uvádí počet hello podle oblastí

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

## <a name="next-steps"></a>Další kroky
Nyní můžete spustit toouse tyto dostupnost a redundance funkce toobuild prostředí Azure. Informace o doporučených postupech najdete v tématu věnovaném [osvědčeným postupům pro zajištění dostupnosti v Azure](../articles/best-practices-availability-checklist.md).

