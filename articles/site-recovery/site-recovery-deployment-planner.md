---
title: "Plánovač nasazení Site Recovery aaaAzure pro VMware do Azure | Microsoft Docs"
description: "Toto je hello Azure Site Recovery nasazení planner uživatelské příručce."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/28/2017
ms.author: nisoneji
ms.openlocfilehash: a8c13cd47850575769e0186528807bc525bdeec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery Deployment Planner
Tento článek je hello Azure Site Recovery nasazení Planner uživatelské příručce pro nasazení v produkčním prostředí VMware do Azure.

## <a name="overview"></a>Přehled

Než začnete chránit všechny virtuální počítače VMware (VM) pomocí Site Recovery, přidělit dostatečnou šířku pásma, podle vašeho denní míry změny dat, toomeet vaše cíl bodu požadované obnovení (RPO). Být jisti toodeploy hello správné číslo konfigurační servery a proces servery místně.

Musíte taky toocreate hello správný typ a počet cílový účet úložiště Azure. Vytvoříte účty služby Storage úrovně Standard nebo Premium, které budou zohledňovat nárůst počtu vašich zdrojových produkčních serverů způsobený zvyšováním využití v průběhu času. Zvolte typ hello úložiště na virtuální počítač, na základě charakteristik zatížení (například pro čtení a zápis vstupně-výstupních operací za sekundu [IOPS] nebo mísení dat) a omezuje Site Recovery.

Hello Site Recovery nasazení planner ve verzi public preview je nástroj příkazového řádku, který je aktuálně dostupné jen pro scénář hello VMware do Azure. Můžete vzdáleně profil virtuální počítače VMware s použitím tento nástroj (bez jakéhokoli produkční dopadu jakkoli) toounderstand hello šířky pásma a požadavky na úložiště Azure pro úspěšná replikace a testovací převzetí služeb při selhání. Hello nástroj můžete spustit bez jakékoli Site Recovery součásti místní instalace. Ale tooget přesné dosáhnout propustnosti výsledků, doporučujeme vám spustit hello planner v systému Windows Server, který splňuje hello je minimální požadavky hello Site Recovery konfigurační server, nakonec potřebovali byste toodeploy jako jeden z kroků první hello v produkčním nasazení.

Nástroj pro Hello poskytuje hello následující podrobnosti:

**Posouzení kompatibility**

* Vyhodnocení způsobilosti virtuálního počítače na základě počtu disků, velikosti disků, počtu vstupně-výstupních operací za sekundu (IOPS, četnosti změn a typu spuštění (EFI nebo BIOS)
* Hello odhadované šířku pásma sítě, které je nutné pro rozdílová replikace

**Srovnání šířky pásma sítě a posouzení cíle bodu obnovení**

* Hello odhadované šířku pásma sítě, které je nutné pro rozdílová replikace
* Hello propustnost, kterou Site Recovery můžete získat z místní tooAzure
* Hello počet virtuálních počítačů toobatch, podle hello odhadované šířky pásma toocomplete počáteční replikace v dané množství času

**Požadavky na infrastrukturu Azure**

* požadavky na úložiště typu (účet úložiště standard nebo premium) Hello pro každý virtuální počítač
* Celkový počet toobe účty úložiště standard a premium nastaven pro replikaci Hello
* Návrhy pojmenování účtů úložiště na základě pokynů pro Azure Storage
* Hello umístění účtu úložiště pro všechny virtuální počítače
* nastavit několik Hello Azure jader toobe před testovacího převzetí služeb při selhání nebo převzetí služeb při selhání na základě předplatného hello
* Hello velikost virtuálního počítače Azure doporučujeme pro každý místní virtuální počítač

**Požadavky na místní infrastrukturu**
* Hello požadovaný počet konfigurační servery a servery toobe proces nasazení místní

>[!IMPORTANT]
>
>Využití je pravděpodobně tooincrease v čase, a proto všechny hello předchozí nástroj výpočty se provádí za předpokladu, že koeficient růstu 30 procent v charakteristiky zatížení a pomocí 95. hodnotu percentilu všechny hello profilace metriky (pro čtení a zápis IOPS, změn a tak stanovilo). Oba tyto elementy (faktor růstu a výpočet percentilu) je možné konfigurovat. Další informace o koeficient růstu toolearn najdete v části "koeficient růstu aspekty" hello. toolearn Další informace o percentilem, najdete v části "Hodnota percentilu použít pro výpočet hello" hello.
>

## <a name="requirements"></a>Požadavky
Nástroj Hello má dvě hlavní fáze: generování profilování a sestavy. Je také třetí možnost toocalculate propustností jenom. v následující tabulce hello jsou uvedeny požadavky Hello hello serveru, ze které hello měření profilace a propustnost spouští:

| Požadavek na server | Popis|
|---|---|
|Profilace a měření propustnosti| <ul><li>Operační systém: Microsoft Windows Server 2012 R2<br>(v ideálním případě odpovídající alespoň hello [velikost doporučení pro konfigurační server hello](https://aka.ms/asr-v2a-on-prem-components))</li><li>Konfigurace počítače: 8 virtuálních CPU, 16 GB paměti RAM, 300 GB HDD</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Microsoft Visual C++ Redistributable for Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>TooAzure přístup k Internetu z tohoto serveru</li><li>Účet služby Azure Storage</li><li>Práva správce na serveru hello</li><li>Volné místo na disku alespoň 100 GB (za předpokladu 1 000 virtuálních počítačů, každý průměrně se 3 disky a profilovaný po dobu 30 dnů)</li><li>Nastavení úrovně statistiky VMware vCenter měli nastavit too2 nebo vysokou úroveň</li><li>Povolte 443 port: Planner nasazení automatické obnovení systému používá tento port tooconnect toovCenter nebo hostiteli ESXi,</ul></ul>|
| Generování sestav | Počítač s Windows nebo Windows Server s aplikací Microsoft Excel 2013 nebo novější |
| Uživatelská oprávnění | Oprávnění jen pro čtení pro hello uživatelský účet, který byl použit tooaccess hello VMware vCenter server nebo VMware vSphere ESXi hostitele během vytváření profilů |

> [!NOTE]
>
>Nástroj Hello můžete profil jenom virtuální počítače s VMDK a RDM disky. Nemůže profilovat virtuální počítače s disky iSCSI nebo NFS. Site Recovery podporuje iSCSI a NFS disky pro servery VMware, ale protože planner hello nasazení není uvnitř hostovaného hello a jeho profily pouze pomocí čítačů výkonu vCenter, nástroj hello nemá přehled těchto typů disku.
>

## <a name="download-and-extract-hello-public-preview"></a>Stažení a extrakci hello verzi public preview
1. Stáhněte si nejnovější verzi hello hello [verzi public preview služby Site Recovery nasazení planner](https://aka.ms/asr-deployment-planner).  
Nástroj Hello je zabalené do složky .zip. Hello aktuální verze nástroje hello podporuje jenom scénář hello VMware do Azure.

2. Zkopírujte hello .zip složky toohello systému Windows server ze kterého chcete, aby nástroj toorun hello.  
Hello nástroj můžete spustit z Windows serveru 2012 R2, pokud má hello server síťový přístup tooconnect toohello vCenter server vSphere ESXi hostitele, který obsahuje hello virtuální počítače toobe profilovaným. Doporučujeme však můžete spustit nástroj hello na serveru, jehož konfigurace hardwaru splňuje hello [konfigurace serveru pro změnu velikosti platí](https://aka.ms/asr-v2a-on-prem-components). Pokud už jste nasadili Site Recovery součásti místně, spusťte nástroj hello z hello konfigurační server.

 Doporučujeme, abyste měli hello stejnou hardwarovou konfiguraci jako hello konfigurační server (což je serveru v vytvořená proces) na serveru hello kterém jste spustili nástroj hello. Taková konfigurace zajistí, že hello dosáhnout propustnosti této hello nástroj sestavy odpovídá hello Skutečná propustnost, Site Recovery můžete dosáhnout během replikace. Výpočet propustnost Hello závisí na dostupnou šířku pásma sítě na serveru hello a konfigurace hardwaru (procesoru, úložiště a tak dále) hello serveru. Pokud spustíte nástroj hello z druhý server, propustnost hello se počítá z tohoto serveru tooMicrosoft Azure. Navíc vzhledem k tomu, že konfigurace hardwaru hello hello serveru může lišit od hello konfigurace serveru, hello dosáhnout propustnosti, kterou hello nástroj sestavy mohou být nepřesné.

3. Rozbalte složku hello .zip.  
Hello obsahuje více soubory a podsložky. spustitelný soubor Hello je ASRDeploymentPlanner.exe v hello nadřazené složky.

    Příklad:  
    Zkopírujte tooE soubor .zip hello: \ jednotek a rozbalte ho.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Možnosti
V žádném z hello následující tři režimy můžete spustit nástroj příkazového řádku hello (ASRDeploymentPlanner.exe):

1. Profilace  
2. Generování sestav
3. Zjištění propustnosti

Nejprve spusťte nástroj hello v profilaci mísení dat virtuálních počítačů toogather režimu a IOPS. V dalším kroku spuštění hello nástroj toogenerate hello sestavy toofind hello šířky pásma a úložiště požadavky na síť.

## <a name="profiling"></a>Profilace
V profilaci režimu připojí hello nasazení planner nástroj toohello vCenter server vSphere ESXi hostitele toocollect výkonu data o hello virtuálních počítačů.

* Profilace nemá vliv na výkon hello hello provozních virtuálních počítačů, protože žádné přímé připojení se vytvoří toothem. Všechny údaje o výkonu se shromažďují z hostitele ESXi server vSphere vCenter hello.
* tooensure se mělo pouze nepatrný dopad na hello serveru z důvodu profilace hello nástroj dotazy hello vCenter server ESXi hostitelů vSphere jednou za 15 minut. Tento interval dotazu není ohrožení profilování přesnost, protože nástroj hello ukládá data čítače výkonu každou minutu.

### <a name="create-a-list-of-vms-tooprofile"></a>Vytvoří seznam tooprofile virtuální počítače
Nejprve je třeba seznam hello virtuální počítače toobe profilovaným. Všechny názvy hello virtuálních počítačů můžete získat na hostiteli ESXi server vSphere vCenter pomocí hello VMware vSphere PowerCLI příkazů v hello následující postup. Alternativně můžete vytvořit seznam v souboru popisné názvy hello nebo hello IP adresy virtuálních počítačů, které chcete tooprofile ručně.

1. Přihlaste se toohello virtuálních počítačů této VMware vSphere PowerCLI je nainstalována v.
2. Otevřete hello VMware vSphere PowerCLI konzolu.
3. Ujistěte se, zda je povoleno hello zásady spouštění skriptu hello. Pokud je zakázána, spusťte hello VMware vSphere PowerCLI konzolu v režimu správce a poté ji spuštěním hello následující příkaz:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. Vám může hello toorun optionly třeba následující příkaz, pokud připojení VIServer nebyl rozpoznán jako název hello rutiny.
 
            Add-PSSnapin VMware.VimAutomation.Core 

5. tooget hello názvy všech virtuálních počítačů ve vCenter server vSphere ESXi hostitelů a uložit do souboru TXT, spuštění hello dva příkazy tady hello seznamu.
Nahraďte zástupné hodnoty &lsaquo;server name&rsaquo; (název serveru), &lsaquo;user name&rsaquo; (uživatelské jméno), &lsaquo;password&rsaquo; (heslo) a &lsaquo;outputfile.txt&rsaquo; (výstupní soubor) vlastními hodnotami.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

6. Otevřete soubor výstup hello v programu Poznámkový blok a zkopírujte hello názvy všechny virtuální počítače, který má tooprofile tooanother souboru (například ProfileVMList.txt), jeden název virtuálního počítače na každý řádek. Tento soubor je používán jako vstupní toohello *- VMListFile* parametr příkazového řádku nástroje hello.

    ![Seznam názvů virtuálních počítačů v nasazení planner hello](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Spuštění profilace
Až budete mít hello seznam profilovaným toobe virtuální počítače, můžete spustit nástroj hello v profilaci režimu. Tady je seznam hello parametrů povinné a nepovinné hello nástroj toorun v profilaci režimu.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Název parametru | Popis |
|---|---|
| -Operation | StartProfiling |
| -Server | Hello plně kvalifikovaný název domény nebo IP adresu hello vCenter server vSphere hostitele ESXi virtuální počítače, jejichž jsou toobe profilovaným.|
| -User | Hello uživatele název tooconnect toohello vCenter server ESXi hostitelů vSphere. Hello uživatel potřebuje toohave jen pro čtení přístup alespoň.|
| -VMListFile | Hello soubor, který obsahuje seznam hello profilovaným toobe virtuálních počítačů. Cesta k souboru Hello může být absolutní nebo relativní. Hello soubor by měl obsahovat jeden název nebo IP adresy virtuálních počítačů na každém řádku. Název virtuálního počítače zadaný v souboru hello by měl být hello stejný jako název virtuálního počítače hello hostiteli ESXi server vSphere vCenter hello.<br>Například soubor hello VMList.txt obsahuje hello následující virtuální počítače:<ul><li>virtual_machine_A</li><li>10.150.29.110</li><li>virtual_machine_B</li><ul> |
| -NoOfDaysToProfile | Hello počet dní, pro které profilace je toobe spustit. Doporučujeme spustit profilování pro více než 15 dní, po které tooensure, který hello vzor zatížení ve vašem prostředí přes hello zadané období je zachytit a využívat tooprovide přesné doporučení. |
| -Directory | (Volitelné) hello universal zásady vytváření názvů (UNC) nebo místního adresáře cesta toostore profilace údaje získané v průběhu vytváření profilů. Není-li název adresáře, hello adresář s názvem 'ProfiledData' v aktuální cestě hello se použije jako výchozí adresář hello. |
| -Password | (Volitelné) hello heslo toouse tooconnect toohello vCenter server ESXi hostitelů vSphere. Pokud nezadáte jeden nyní, budete vyzváni k ho když se spustí příkaz hello.|
| -StorageAccountName | Název účtu úložiště hello (volitelné), který je použité toofind hello propustnost dosažitelné pro replikaci dat z místní tooAzure. Hello nástroj nahrávání testovací data toothis účet toocalculate propustnost úložiště.|
| -StorageAccountKey | (Volitelné) hello klíč účtu úložiště, který se používá účet úložiště tooaccess hello. Přejděte toohello portálu Azure > účty úložiště ><*název účtu úložiště*>> Nastavení > přístupové klíče > Key1 (nebo primární přístupový klíč pro účet úložiště classic). |
| -Environment | (Volitelné) Toto je vaše cílové prostředí účtu Azure Storage. Může to být jedna ze tří hodnot – AzureCloud, AzureUSGovernment a AzureChinaCloud. Výchozí hodnota je AzureCloud. Pokud vaše cílem oblast Azure Azure US Government nebo Azure China cloudy, pomocí parametru hello. |


Doporučujeme, abyste profil virtuální počítače pro alespoň 15 dnů too30. Během hello profilace období neustále běží ASRDeploymentPlanner.exe. Nástroj Hello přijímá profilování vstup času ve dnech. Pokud chcete použít pro několik hodin nebo minut pro rychlé test hello nástroje tooprofile ve verzi public preview hello, budete potřebovat tooconvert hello čas do hello ekvivalentní měr dní. Například tooprofile 30 minut, musí být vstup hello 30/(60*24) = 0.021 dnů. Hello minimální povolená profilace doba je 30 minut.

Při vytváření profilu, můžete volitelně předat název účtu úložiště a klíč toofind hello propustnosti, kterou můžete dosáhnout Site Recovery v době hello replikace z hello konfigurační server nebo server tooAzure procesu. Pokud během profilace nejsou předány hello název účtu úložiště a klíč, nástroj hello neobsahuje výpočet možná propustnost.

Můžete spustit víc instancí hello nástroje pro různé sady virtuálních počítačů. Ujistěte se, že nejsou v některém z hello profilace nastaví opakuje hello názvy virtuálních počítačů. Například, pokud mají profilovaným deset virtuálních počítačů (VM1 prostřednictvím VM10) a po několik dní chcete tooprofile jiné pět virtuálních počítačů (VM11 prostřednictvím VM15), hello nástroj můžete spustit z jiné konzolu příkazového řádku pro hello druhé sadě virtuálních počítačů (VM11 prostřednictvím VM15). Zkontrolujte, že hello druhé sadě virtuálních počítačů nemají žádné názvy virtuálních počítačů z první instance profilování hello nebo používáte jiný výstupního adresáře pro hello druhý spustit. Pokud chcete použít dvě instance hello nástroje pro profilaci hello stejné virtuální počítače a použít Dobrý den stejné výstupního adresáře, hello generuje sestavy budou nesprávné.

Konfigurace virtuálního počítače zachycení jednou na začátku hello hello profilování operace a uložené v souboru s názvem VMDetailList.xml. Tyto informace se používá při generování sestavy hello. Všechny změny v konfiguraci virtuálního počítače (například vyšší počet jader, disky nebo síťové adaptéry) od hello začátku toohello konce profilace není zachycena. Pokud došlo ke změně PROFILOVANÉHO konfigurace virtuálního počítače během postupu hello profilace ve verzi public preview hello tady je alternativní řešení hello tooget nejnovější virtuálních počítačů podrobnosti při generování sestavy hello:

* Zálohování VMdetailList.xml a hello soubor odstraňte z jeho aktuálního umístění.
* Předání - a - hesla uživatele argumentů v době hello generování sestav.

Hello profilace příkaz generuje několik souborů v hello profilace adresáře. Neodstraňujte žádný hello souborů, protože to nepříznivě ovlivňuje, generování sestav.

#### <a name="example-1-profile-vms-for-30-days-and-find-hello-throughput-from-on-premises-tooazure"></a>Příklad 1: Profil virtuálních počítačů pro 30 dnů a propustnost hello najít z místní tooAzure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Příklad 2: Profilování virtuálních počítačů po dobu 15 dnů

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-hello-tool"></a>Příklad 3: Profil virtuální počítače 1 hodinu pro rychlé testu nástroj hello
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Pokud je tento nástroj hello hello server systémem se restartoval nebo došlo k chybě, nebo pokud zavřete hello nástroj pomocí kombinace kláves Ctrl + C, hello profilovaným dat se zachová. Existuje však riziko chybějící hello PROFILOVANÉHO data za posledních 15 minut. V takové instance znovu spusťte nástroj hello v profilaci režimu po restartování serveru hello.
>* Když hello název účtu úložiště a klíč jsou předány, hello nástroj míry hello propustnost na poslední krok hello profilace. Pokud nástroj hello je ukončeno před dokončením profilace, se nevypočte hello propustnost. propustnost hello toofind před vygenerováním hello sestavy, hello GetThroughput operace můžete spustit z příkazového řádku konzoly hello. Hello vygenerovat sestavu, jinak nebude obsahovat hello propustnost informace.


## <a name="generate-a-report"></a>Generování sestav
Hello Nástroj generuje soubor Microsoft Excel s podporou maker (soubor XLSM) jako výstup hello sestavy, které shrnuje všechny doporučení pro nasazení hello. Sestava Hello jmenuje DeploymentPlannerReport_ <*jedinečný číselný identifikátor*> .xlsm a umístěný v hello zadán adresář.

Po dokončení profilace můžete spustit nástroj hello v režimu generování sestav. Hello následující tabulka obsahuje seznam toorun parametrů povinné a nepovinné nástroj v režimu generování sestav.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Název parametru | Popis |
|-|-|
| -Operation | GenerateReport |
| -Server |  server vCenter/vSphere Hello plně kvalifikovaný název domény nebo IP adresu (hello použijte stejný název nebo IP adresu, která jste použili v době hello profilace) kde hello profilovaným virtuálních počítačů, jejichž sestava je toobe generované nacházejí. Všimněte si, že pokud jste použili vCenter server v době hello profilace, nemůžete použít vSphere server pro generování sestav a naopak.|
| -VMListFile | Hello soubor, který obsahuje seznam hello PROFILOVANÉHO virtuálních počítačů, které hello sestavy je toobe vygenerované. Cesta k souboru Hello může být absolutní nebo relativní. Hello soubor by měl obsahovat jeden název virtuálního počítače nebo IP adresa na každém řádku. Hello názvy virtuálních počítačů, které jsou určené v souboru hello by měl být hello stejné jako názvy hello virtuálních počítačů na hostiteli ESXi server vSphere vCenter hello a shoda, co byl použit při vytváření profilu.|
| -Directory | (Volitelné) hello UNC nebo cestu místního adresáře, kde hello profilovaným data (soubory generované během profilace) je uložen. Tato data jsou požadovány pro generování sestavy hello. Pokud název nezadáte, použije se adresář ProfiledData. |
| -GoalToCompleteIR | (Volitelné) hello počet hodin, ve které hello počáteční replikace hello profilovaným virtuální počítače musí toobe byla dokončena. Sestava Hello generované obsahuje hello počet virtuálních počítačů, pro které dá dokončit počáteční replikaci hello zadaný čas. Výchozí hodnota Hello je 72 hodin. |
| -User | (Volitelné) hello uživatele název toouse tooconnect toohello vCenter vSphere server. Název Hello je použité toofetch hello nejnovější informace o konfiguraci hello virtuálních počítačů, jako je například hello počet disků, počet jader a počet síťových adaptérů, toouse v sestavě hello. Pokud není zadán název hello, použije se na začátku hello hello profilace kickoff shromážděné informace o konfiguraci hello. |
| -Password | (Volitelné) hello heslo toouse tooconnect toohello vCenter server ESXi hostitelů vSphere. Pokud hello heslo není zadané jako parametr, budete vyzváni k ho později když se spustí příkaz hello. |
| -DesiredRPO | (Volitelné) hello požadovaný cíl bodu obnovení, v minutách. Hello výchozí hodnota je 15 minut.|
| -Bandwidth | Šířka pásma v Mb/s. Hello parametr toouse toocalculate hello plánovaný bod obnovení, který jde dosáhnout pro hello zadat šířku pásma. |
| -StartDate | (Volitelné) hello počáteční datum a čas v MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr *StartDate* je nutné zadat společně s parametrem *EndDate*. Pokud je zadána počátečním, sestava hello se generuje pro hello profilovaným data, která se shromažďují mezi počátečním a koncovým datem. |
| -EndDate | (Volitelné) hello koncové datum a čas v MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr *EndDate* je nutné zadat společně s parametrem *StartDate*. Pokud je zadána koncovým datem, sestava hello se generuje pro hello profilovaným data, která se shromažďují mezi počátečním a koncovým datem. |
| -GrowthFactor | Koeficient růstu (volitelné) hello vyjádřený v procentech. Hello výchozí hodnota je 30 procent. |
| -UseManagedDisks | (Volitelné) UseManagedDisks – Yes/No (Ano/Ne). Výchozí hodnota je Yes (Ano). Hello počet virtuálních počítačů, které se dají umístit do jednoho úložiště účet se počítá vzhledem k tomu, jestli je na spravovaných disků na místo nespravované disku provádět převzetí služeb při selhání a testovací převzetí služeb při selhání virtuálních počítačů. |

#### <a name="example-1-generate-a-report-with-default-values-when-hello-profiled-data-is-on-hello-local-drive"></a>Příklad 1: Vygenerování sestavy s výchozími hodnotami, když hello profilovaným data na místním disku hello
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-hello-profiled-data-is-on-a-remote-server"></a>Příklad 2: Vygenerování sestavy, když je hello profilovaným dat na vzdáleném serveru
Měli byste mít přístup pro čtení a zápis na hello vzdáleného adresáře.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-toocomplete-ir-within-specified-time"></a>Příklad 3: Vygenerování sestavy s konkrétní šířky pásma a cílem toocomplete reakcí na Incidenty v rámci určeného časového
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-hello-default-30-percent"></a>Příklad 4: Vygenerování sestavy s koeficient růstu 5 procent místo hello výchozí 30 procent
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Příklad 5: Generování sestavy s použitím podmnožiny profilovaných dat
Například máte 30 dní od data PROFILOVANÉHO a chcete toogenerate sestavy pouze 20 dní.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>Příklad 6: Generování sestavy s použitím 5minutového cíle bodu obnovení
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-hello-calculation"></a>Hodnota percentilu použít pro výpočet hello
**Jaké výchozí hodnotu percentilu hello metriky výkonu shromážděných během profilace nemá hello nástroj pro použití při generování sestavy?**

Hello nástroj výchozí toohello 95. percentil hodnot pro čtení a zápis IOPS, zapisovat IOPS a změn dat, které jsou shromážděny během profilace všechny virtuální počítače hello. Tato metrika zajistí, že hello 100 percentilu Špička virtuálních počítačů může dojít z důvodu dočasného události je nepoužívá toodetermine požadavků cílový účet úložiště a zdroje šířkou pásma. Příkladem dočasné události může být úloha zálohování spouštěná jednou denně, pravidelné indexování databáze, aktivita generování analytických sestav nebo další podobné krátkodobé a jednorázové události.

Pomocí 95. percentil hodnot true přehled o skutečné pracovní zátěže charakteristiky a vám nabízí hello nejlepší výkon při zatížení hello běží v Azure. Jsme není předpokládá, že by to musíte toochange toto číslo. Pokud změníte hodnotu hello (toohello 90. percentil, např.), můžete aktualizovat konfigurační soubor hello *ASRDeploymentPlanner.exe.config* v hello výchozí složku a uložte ho na hello existující profilovaným toogenerate nové sestavy data.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Aspekty faktoru růstu
**Proč bych při plánování nasazení měl brát v úvahu faktor růstu?**

Je důležité tooaccount pro růst v vaše charakteristiky zatížení, za předpokladu, že potenciální zvýšení využití v čase. Po ochrany na místě, je-li změnit váš charakteristiky zatížení si nelze přepnout tooa jiný účet úložiště pro ochranu bez zakázat a znovu povolíte ochranu hello.

Řekněme například, že dnes se váš virtuální počítač vejde do účtu replikace služby Storage úrovně Standard. Přes hello následující tři měsíce, několik změn se pravděpodobně toooccur:

* zvýší počet Hello uživatelům hello aplikace, která běží na hello virtuálních počítačů.
* Výsledný vyšší změn Hello na hello virtuálních počítačů bude vyžadovat úložiště toopremium toogo hello virtuálního počítače tak, aby Site Recovery replikace můžete držet krok.
* V důsledku toho budou mít toodisable a znovu povolte ochranu tooa prémiový účet úložiště.

Důrazně doporučujeme, abyste naplánovali pro růst při plánování nasazení a při hello výchozí hodnota je 30 procent. Jsou hello odborné na projekce vaší aplikace využití vzoru a růst, a toto číslo můžete změnit odpovídajícím způsobem při generování sestavy. Kromě toho můžete generovat sestavy několika různými faktory růst s hello stejné profilovaným dat a zjistit, jaké doporučení pro šířku pásma úložiště a zdroj cílové nejvhodnější pro vás.

Sestava Hello vygenerované Microsoft Excel obsahuje hello následující informace:

* [Input](site-recovery-deployment-planner.md#input) (Vstup)
* [Recommendations](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input) (Doporučení)
* [Recommendations-Bandwidth Input](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input) (Doporučení – Vstupní šířka pásma)
* [VM&lt;-&gt;Storage Placement](site-recovery-deployment-planner.md#vm-storage-placement) (Umístění virtuálních počítačů ve službě Storage)
* [Compatible VMs](site-recovery-deployment-planner.md#compatible-vms) (Kompatibilní virtuální počítače)
* [Incompatible VMs](site-recovery-deployment-planner.md#incompatible-vms) (Nekompatibilní virtuální počítače)

![Deployment Planner](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Zjištění propustnosti

propustnost hello tooestimate, Site Recovery můžete dosáhnout místní tooAzure během replikace, spusťte nástroj hello v GetThroughput režimu. Nástroj Hello vypočítá hello propustnost ze serveru hello hello nástroj běží na. V ideálním případě by tento server je založený na velikost příručce hello konfigurace serveru. Pokud už jste nasadili Site Recovery součásti místní infrastrukturou, spusťte nástroj hello na hello konfiguračním serveru.

Otevřete konzolu příkazového řádku a přejděte plánování složka nástroj pro nasazení Site Recovery toohello. Spusťte ASRDeploymentPlanner.exe s použitím následujících parametrů.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Název parametru | Popis |
|-|-|
| -Operation | GetThroughput |
| -Directory | (Volitelné) hello UNC nebo cestu místního adresáře, kde hello profilovaným data (soubory generované během profilace) je uložen. Tato data jsou požadovány pro generování sestavy hello. Pokud název adresáře není zadaný, použije se adresář ProfiledData. |
| -StorageAccountName | název účtu úložiště Hello, který byl použit toofind hello šířky pásma použít pro replikaci dat z místní tooAzure. Hello nástroj nahrávání testovací data toothis úložiště účet toofind hello spotřebovanou šířku pásma. |
| -StorageAccountKey | Hello klíč účtu úložiště, který se používá účet úložiště tooaccess hello. Přejděte toohello portálu Azure > účty úložiště ><*název účtu úložiště*>> Nastavení > přístupové klíče > Key1 (nebo primární přístupový klíč pro účet úložiště classic). |
| -VMListFile | Hello soubor, který obsahuje seznam hello profilovaným pro výpočet šířky pásma hello spotřebované toobe virtuálních počítačů. Cesta k souboru Hello může být absolutní nebo relativní. Hello soubor by měl obsahovat jeden název nebo IP adresy virtuálních počítačů na každém řádku. názvy virtuálních počítačů Hello zadané v souboru hello by měl být hello stejné jako názvy hello virtuálních počítačů na hostiteli ESXi server vSphere vCenter hello.<br>Například soubor hello VMList.txt obsahuje hello následující virtuální počítače:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Environment | (Volitelné) Toto je vaše cílové prostředí účtu Azure Storage. Může to být jedna ze tří hodnot – AzureCloud, AzureUSGovernment a AzureChinaCloud. Výchozí hodnota je AzureCloud. Pokud vaše cílem oblast Azure Azure US Government nebo Azure China cloudy, pomocí parametru hello. |

Nástroj Hello vytvoří několik 64 MB asrvhdfile <> # VHD soubory na hello zadaný adresář (kde "#" je hello počet souborů). Nástroj Hello ukládání hello soubory toohello úložiště účet toofind hello propustnost. Po hello propustnost je měřena, nástroj hello odstraní všechny soubory hello z účtu úložiště hello a z místního serveru hello. Pokud nástroj hello je ukončeno z jakéhokoli důvodu při jeho je výpočet propustnost, nedojde k odstranění hello soubory z úložiště hello nebo místního serveru hello. Budete mít toodelete je ručně.

Hello propustnosti se měří v určitém bodě v čase a je hello maximální propustnost, Site Recovery můžete dosáhnout během replikace, za předpokladu, že zůstanou jinými faktory hello stejné. Například pokud se všechny aplikace spustí využívání větší šířku pásma na hello, které se liší podle stejné síti, Skutečná propustnost hello během replikace. Pokud používáte hello GetThroughput příkaz z konfigurace serveru, nástroj hello ne všechny chráněné virtuální počítače a probíhající replikace. Hello výsledek měřená propustnost hello se liší, pokud hello GetThroughput operaci je spustit, když hello chráněné virtuální počítače mají vysokou data změn. Doporučujeme spustit nástroj hello v různých okamžicích v době profilace toounderstand jakou propustnost úrovně jde dosáhnout v různé časy. V sestavě hello hello nástroj zobrazí poslední měřená propustnost hello.

### <a name="example"></a>Příklad
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Spusťte nástroj hello na server, který má hello stejné úložiště a rychlost procesoru jako hello konfigurační server.
>
> Pro replikaci nastavte hello doporučené šířky pásma toomeet hello RPO 100 procent času hello. Po nastavení hello správné šířky pásma, pokud nevidíte zvýšit propustnost hello dosáhnout hlášené hello nástroj hello následující:
>
>  1. Toodetermine zkontrolujte, zda jsou všechny sítě služby (QoS Quality Service), je omezení propustnosti Site Recovery.
>
>  2. Toodetermine zkontrolujte, zda je váš trezor Site Recovery v hello nejbližší fyzicky podporované latence sítě toominimize oblast Microsoft Azure.
>
>  3. Vaše místní úložiště vlastností toodetermine zkontrolujte, zda se může zlepšit hello hardwaru (například tooSSD HDD).
>
>  4. Změnit nastavení Site Recovery hello hello procesový server příliš[zvýšit hello šířku pásma sítě používané pro replikaci](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Doporučení s možností zadat požadovaný cíl bodu obnovení jako vstup

### <a name="profiled-data"></a>Profilovaná data

![zobrazení profilovaným dat Hello v nasazení planner hello](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Období PROFILOVANÉHO dat**: hello období, během které hello profilace byla spuštěna. Ve výchozím nastavení nástroj hello zahrnuje všechna PROFILOVANÉHO data výpočtu hello, pokud generuje sestavy hello za určité období pomocí možnosti počátečním a koncovým datem během generování sestavy.

**Název serveru**: hello název nebo IP adresu hello VMware vCenter nebo hostiteli ESXi, jejichž virtuálních počítačů je sestava vygenerována.

**Požadovaného RPO**: hello plánovaného bodu obnovení pro vaše nasazení. Ve výchozím nastavení hello požadované šířky pásma sítě je vypočtena pro hodnoty RPO 15, 30 až 60 minut. Na základě hello výběru, hello vliv hodnoty jsou aktualizovány na listu hello. Pokud jste použili hello *DesiredRPOinMin* parametr při generování sestavy hello, které hodnoty se zobrazí v hello potřeby RPO výsledek.

### <a name="profiling-overview"></a>Přehled profilace

![Profilace má za následek planner nasazení hello](./media/site-recovery-deployment-planner/profiling-overview.png)

**Celkový počet virtuálních počítačů profilovaným**: hello celkový počet virtuálních počítačů, jejichž PROFILOVANÉHO data nejsou k dispozici. Pokud hello VMListFile názvy všech virtuálních počítačů, které nebyly profilovaným, tyto virtuální počítače nejsou zahrnuty do hello generování sestav a jsou vyloučeny z počtu virtuálních počítačů celkový PROFILOVANÉHO hello.

**Kompatibilní virtuální počítače**: hello počet virtuálních počítačů, které můžou být chráněné tooAzure pomocí Site Recovery. Je celkový počet kompatibilní virtuální počítače, pro které hello jsou vypočítávány požadované šířky pásma sítě, počet účtů úložiště, počet jader, Azure a počet konfigurační servery a servery pro další proces hello. nejsou k dispozici v části "Kompatibilní virtuální počítače" hello Hello podrobnosti každých kompatibilní virtuálního počítače.

**Kompatibilní virtuální počítače**: hello počet PROFILOVANÉHO virtuálních počítačů, které jsou kompatibilní pro ochranu pomocí Site Recovery. Hello důvody nekompatibilita jsou uvedené v části "Nekompatibilní VMs" hello. Pokud hello VMListFile názvy všech virtuálních počítačů, které nebyly profilovaným, tyto virtuální počítače jsou vyloučeny z hello nekompatibilní počet virtuálních počítačů. Tyto virtuální počítače jsou uvedeny jako "Dat nebyl nalezen" na konci hello hello "nekompatibilní VMs" oddílu.

**Desired RPO:** Požadovaný cíl bodu obnovení v minutách. Sestava Hello je generována pro tři hodnoty RPO: 15 (výchozí), 30 až 60 minut. doporučení Hello šířky pásma v sestavě hello se změní podle vašeho výběru v hello potřeby RPO rozevíracím seznamu v pravém horním hello hello stylů. Pokud vygenerování sestavy hello pomocí hello *- DesiredRPO* parametr s hodnotou vlastní tato vlastní hodnota zobrazí jako výchozí hello hello potřeby RPO rozevíracího seznamu.

### <a name="required-network-bandwidth-mbps"></a>Požadovaná šířka pásma sítě (Mb/s)

![Požadované šířky pásma sítě v nasazení planner hello](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**toomeet RPO 100 procent času hello:** hello doporučená šířka pásma v MB/s toobe přidělené toomeet vaše požadované RPO 100 procent času hello. Toto množství šířky pásma musí být vyhrazený pro stabilního stavu rozdílové replikace všechny vaše kompatibilní tooavoid virtuální počítače veškerá porušení zásad plánovaný bod obnovení.

**toomeet RPO 90 procent času hello**: z důvodu ceny širokopásmové připojení nebo z jiného důvodu, pokud nelze nastavit hello šířka pásma potřebná toomeet vaše požadované RPO 100 procent času hello můžete toogo s menší šířkou pásma, která nastavení můžete splňují vaše požadované RPO 90 procent času hello. toounderstand důsledky hello nastavení této menší šířkou pásma, hello sestava obsahuje analýz na hello počet a dobu trvání RPO porušení tooexpect.

**Dosažených propustnost:** hello propustnost ze hello serveru, na kterém jste spustili hello GetThroughput příkaz toohello Microsoft Azure oblast, kde hello účet úložiště se nachází. Toto číslo propustnost, bude hladina hello odhadované, můžete dosáhnout při ochraně hello kompatibilní virtuální počítače pomocí Site Recovery za předpokladu, že konfigurační server nebo server proces úložiště a síť charakteristiky zůstanou text hello, stejně jako u Hello server, ze kterého jste spustili nástroj hello.

Pro replikaci byste měli nastavit hello doporučené šířky pásma toomeet hello RPO 100 procent času hello. Po nastavení šířky pásma hello, pokud nevidíte všechny zvýšit propustnost hello dosáhnout vykazované hello nástroj, proveďte následující hello:

1. Toosee zkontrolujte, zda jsou všechny sítě služby (QoS Quality Service), je omezení propustnosti Site Recovery.

2. Toosee zkontrolujte, zda je váš trezor Site Recovery v hello nejbližší fyzicky podporované latence sítě toominimize oblast Microsoft Azure.

3. Vaše místní úložiště vlastností toodetermine zkontrolujte, zda se může zlepšit hello hardwaru (například tooSSD HDD).

4. Změnit nastavení Site Recovery hello hello procesový server příliš[zvýšit hello velikost šířky pásma sítě používané pro replikaci](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Pokud používáte nástroj hello na konfigurační server nebo server proces, který už je chráněný virtuální počítače, spusťte nástroj hello několikrát. Hello dosáhnout propustnosti číslo změny v závislosti na hello množství změn, které jsou zpracovávány v tomto bodě v čase.

Pro všechna podniková nasazení Site Recovery doporučujeme použít [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Požadované účty úložiště
Následující graf ukazuje, které hello celkový počet úložiště účtů (standard a premium), které jsou všechny požadované tooprotect Hello hello kompatibilní virtuální počítače. toolearn které úložiště účet toouse pro každý virtuální počítač, najdete v části "umístění úložiště virtuálního počítače" hello.

![Účty požadované úložiště v nasazení planner hello](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Požadovaný počet jader Azure
Tento výsledek je hello celkový počet jader toobe nastavit před převzetí služeb při selhání nebo testovací převzetí služeb při selhání všech hello kompatibilní virtuální počítače. Pokud jsou k dispozici v rámci předplatného hello příliš málo jader, Site Recovery selže toocreate virtuální počítače v době hello testovací převzetí služeb při selhání nebo převzetí služeb při selhání.

![Počet jader Azure v nasazení planner hello](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Požadovaná místní infrastruktura
Na tomto obrázku je celkový počet hello konfigurační servery a další proces servery toobe nakonfigurována, bude stačit tooprotect všechny hello kompatibilní virtuální počítače. V závislosti na hello podporované [velikost doporučení pro konfigurační server hello](https://aka.ms/asr-v2a-on-prem-components), hello nástroj může doporučit dalších serverů. Hello doporučení je založeno na hello větší hello denní změn nebo hello maximální počet chráněných virtuálních počítačů (za předpokladu, že v průměru tři disky na virtuální počítač), podle toho, co je dosáhl první na konfigurační server hello nebo hello další procesový server. Podrobnosti o hello celkového objemu změn každý den a celkový počet chráněných disků najdete v části "Vstup" hello.

![Místní infrastruktury v nasazení planner hello](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Analýza „co kdyby“
Tato analýza popisuje, kolik porušení mohlo dojít během hello profilace období když nastavíte, menší šířku pásma pro potřeby hello RPO toobe splněné pouze 90 procent času hello. Každý den může dojít k jednomu nebo několika narušením cíle bodu obnovení. Hello graf znázorňuje hello ve špičce plánovaný bod obnovení hello den.
Na základě této analýzy, můžete rozhodnout, pokud je přijatelné s hello počet porušení RPO ve všech dnů a ve špičce RPO přístupů za den hello zadán menší šířkou pásma. Pokud je přijatelné, že kterou můžete přidělit hello menší šířkou pásma pro replikaci, jinak přidělte hello vyšší šířku pásma, jakou navrhované toomeet hello potřeby RPO 100 procent času hello.

![Analýz v nasazení planner hello](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Doporučená velikost dávky virtuálních počítačů pro prvotní replikaci
V této části doporučujeme, abyste hello počet virtuálních počítačů, které se dají chránit v paralelní toocomplete hello počáteční replikace v rámci 72 hodin s hello navrhované, že šířku pásma toomeet potřeby RPO 100 procent času hello Probíhá nastavení. Tato hodnota je konfigurovatelná. toochange ji během generování sestav, použijte hello *GoalToCompleteIR* parametr.

Hello zde graf zobrazuje rozsah hodnot šířky pásma a počítané virtuálních počítačů batch velikost počet toocomplete počáteční replikace za 72 hodin, na základě průměru hello zjistila virtuálních počítačů velikost napříč všemi hello kompatibilní virtuální počítače.

Ve verzi public preview hello hello sestavy není určit, které virtuální počítače mají být zahrnuty v dávce. Můžete použít velikost disku hello uvedené v části toofind hello "kompatibilní virtuální počítače" velikost každého virtuálního počítače a je vybrat pro dávky, nebo můžete vybrat hello virtuálních počítačů na základě zatížení známých charakteristik. čas dokončení Hello hello počáteční replikace změn úměrně, podle hello skutečná velikost disku virtuálního počítače, používá místo na disku a propustnost sítě k dispozici.

![Doporučená velikost dávky virtuálních počítačů](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>Použitý faktor růstu a hodnoty percentilu
Tato část dole hello hello list ukazuje hello percentilu hodnota používaná pro všechny čítače výkonu hello hello profilovaným virtuálních počítačů (výchozí hodnota je 95. percentil) a hello koeficient růstu (výchozí hodnota je 30 procent), který se používá v všech výpočtů hello.

![Použitý faktor růstu a hodnoty percentilu](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Doporučení s možností zadat dostupnou šířku pásma jako vstup

![Doporučení s možností zadat dostupnou šířku pásma jako vstup](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Může nastat situace, kdy víte, že pro účely replikace Site Recovery nemůžete nastavit šířku pásma větší než x Mb/s. Nástroj Hello vám umožní tooinput dostupnou šířku pásma (pomocí hello - parametr šířky pásma při generování sestavy) a získání hello dosažitelné RPO v minutách. Pomocí této dosažitelné hodnoty RPO můžete rozhodnout, jestli potřebujete tooset až dodatečnou šířku pásma nebo jsou OK s nutnosti řešení zotavení po havárii s Tento plánovaný bod obnovení.

![Dosažitelný cíl bodu obnovení pro šířku pásma 500 Mb/s](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Vstup
Hello vstup listu poskytuje že přehled hello profilovaným prostředí VMware.

![Přehled hello profilovaným prostředí VMware](./media/site-recovery-deployment-planner/Input.png)

**Počáteční datum** a **koncové datum**: hello počátečním a koncovým datem hello profilace data považována za pro generování sestav. Ve výchozím nastavení, hello počáteční datum je datum hello profilace při spuštění, a hello koncové datum je hello datum, kdy profilace zastaví. To může být hello 'Počátečním' a 'koncovým datem, hodnoty, pokud hello je sestava vygenerována s těmito parametry.

**Celkový počet dnů profilace**: Celkový počet dnů, za které profilace mezi hello hello počátečního a koncového data, pro které hello je sestava vygenerována.

**Počet virtuálních počítačů kompatibilní**: hello celkový počet kompatibilní virtuální počítače, pro které hello požadované šířky pásma sítě, požadovaný počet úložiště účtů, Microsoft Azure jádra, konfigurační servery a servery další proces Vypočítat.

**Celkový počet disků všech virtuálních počítačů kompatibilní**: hello číslo, které se používá jako jedna z hello vstup toodecide hello počet konfigurační servery a další proces servery toobe používá při nasazení hello.

**Průměrný počet disků na kompatibilní virtuální počítač**: vypočítat hello průměrný počet disků mezi všechny virtuální počítače kompatibilní.

**Průměrná velikost disku (GB)**: vypočítat velikost průměrné hello mezi všechny virtuální počítače kompatibilní.

**Požadovaného plánovaný bod obnovení (minuty)**: buď hello výchozí hodnota bodu obnovení cíl nebo hello předaná pro parametr 'DesiredRPO' hello v době hello tooestimate generování sestav vyžaduje šířky pásma.

**Požadovaného šířky pásma (Mbps)**: hello hodnotu, která mají předaná pro parametr "Šířky pásma" hello v době hello tooestimate generování sestav dosažitelné plánovaný bod obnovení.

**Mísení zjištěnou typické dat za den (GB)**: průměr dat změn hello vysledovat v všechny profilování dnů. Toto číslo se používá jako jedna hello vstupy toodecide hello počet konfigurační servery a další proces servery toobe používá při nasazení hello.


## <a name="vm-storage-placement"></a>Umístění virtuálních počítačů v účtech úložiště

![Umístění virtuálních počítačů v účtech úložiště](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Typ úložiště disku**: buď standard nebo premium účtu úložiště, což je použité tooreplicate všechny hello odpovídající virtuální počítače uveden v hello **virtuální počítače tooPlace** sloupce.

**Navrhované předponu**: hello navrhované třech znacích předponu, která lze použít pro pojmenování hello účet úložiště. Můžete použít vlastní předpony, ale nástroj hello návrhu následuje hello [oddílu zásady vytváření názvů pro účty úložiště](https://aka.ms/storage-performance-checklist).

**Navrhované název účtu**: název účtu úložiště hello po zahrnete hello navrhované předponu. Nahraďte název hello v rámci hello lomené závorky (< a >) s váš vlastní vstup.

**Přihlaste se účet úložiště**: všechny protokoly replikace hello se ukládají do standardní účet úložiště. Pro virtuální počítače, které se replikují tooa prémiový účet úložiště nastavení účtu další standardní úložiště pro úložiště protokolů. Jeden účet úložiště protokolů úrovně Standard může využívat více účtů úložiště replikace úrovně Premium. Virtuální počítače, které jsou replikované toostandard úložiště využívají účty hello stejný účet úložiště pro protokoly.

**Navrhované název účtu protokolu**: protokolu názvem svého účtu úložiště po zahrnete hello navrhované předponu. Nahraďte název hello v rámci hello lomené závorky (< a >) s váš vlastní vstup.

**Souhrn umístění**: Souhrn hello celkové zatížení virtuálních počítačů na účet úložiště hello během hello replikace a testovací převzetí služeb při selhání nebo převzetí služeb při selhání. Obsahuje hello celkový počet virtuálních počítačů namapované toohello účet úložiště, celkový počet čtení/zápisu mezi všechny virtuální počítače, které je umístěn v rámci tohoto účtu úložiště, celkový počet IOPS zápisu (replikace) IOPS, celkový počet nastavení velikosti pro všechny disky a celkový počet disků.

**Virtuální počítače tooPlace**: seznam všech hello virtuálních počítačů, které musí být umístěny na hello zadaný účet úložiště pro zajištění optimálního výkonu a využití.

## <a name="compatible-vms"></a>Kompatibilní virtuální počítače
![Tabulka aplikace Excel s kompatibilními virtuálními počítači](./media/site-recovery-deployment-planner/compatible-vms.png)

**Název virtuálního počítače**: hello název virtuálního počítače nebo IP adresu, která se používá v hello VMListFile při vygenerování sestavy. Tento sloupec uvádí také hello disky (VMDKs), které jsou připojené toohello virtuálních počítačů. vCenter toodistinguish virtuální počítače s duplicitními názvy nebo IP adresy, názvy hello patří název hostitele ESXi hello. Hello uvedené hostitele ESXi je hello jeden kde hello virtuálního počítače byl umístěn při zjištění hello nástroj během hello profilace období.

**VM Compatibility** (Kompatibilita virtuálního počítače): Hodnoty jsou **Yes** (Ano) a **Yes**\* (Ano). **Ano** \* je pro instance v které hello virtuálního počítače je přizpůsobit pro [Azure Premium Storage](https://aka.ms/premium-storage-workload). Zde hello profilovaným vysokou změn nebo disku se vejde hello P20 nebo P30 kategorii, ale hello velikost disku hello způsobí, že ji namapovat dolů tooa P10 nebo P20 toobe. účet úložiště Hello, rozhodne se disku, který úložiště premium zadejte toomap disk, na základě jeho velikosti. Například:
* Menší než 128 GB je P10.
* 128 GB too512 GB je P20.
* 512 GB too1024 GB je P30.
* 1025 GB too2048 GB je P40.
* 2049 GB too4095 GB je P50.

Pokud charakteristiky zatížení hello disku pro něj hello P20 nebo P30 kategorii, ale jeho velikost hello mapuje dolů typ disku úložiště nižší úrovně premium tooa, nástroj hello označí tohoto virtuálního počítače jako **Ano**\*. Nástroj Hello taky doporučuje změníte hello zdroj disku velikost toofit do hello doporučená typ disku úložiště premium nebo změnit hello cílový disk typu post-převzetí služeb při selhání.

**Storage Type** (Typ služby Storage): Standard nebo Premium.

**Navrhované předponu**: hello předpona účtu úložiště tři znaky.

**Účet úložiště**: hello název, který používá předpona hello navrhované účet úložiště.

**R/W IOPS (s koeficient růstu)**: hello ve špičce úloh pro čtení a zápis IOPS na disku hello (výchozí hodnota je 95. percentil), včetně budoucí koeficient růstu hello (výchozí hodnota je 30 procent). Všimněte si, že hello celkový pro čtení a zápis IOPS virtuálního počítače není vždy hello součet hello Virtuálního počítače jednotlivé disky pro čtení a zápis IOPS, protože hello ve špičce pro čtení a zápis IOPS hello virtuální počítač je ve špičce hello hello Sum jeho jednotlivých disků pro čtení a zápis IOPS během každou minutu hello profilace období.

**Data změn v MB/s (s koeficient růstu)**: hello ve špičce klidové vytížení na disku hello (výchozí hodnota je 95. percentil), včetně budoucí koeficient růstu hello (výchozí hodnota je 30 procent). Všimněte si, že hello mísení dat celkový Dobrý den virtuálního počítače není vždy hello součet hello Virtuálního počítače jednotlivé disky změn dat, protože změn dat ve špičce hello Dobrý den virtuální počítač je ve špičce hello hello Sum z jeho jednotlivé disky změn během každou minutu hello profilace období.

**Velikost virtuálního počítače Azure**: hello ideální namapované velikost virtuálního počítače Azure Cloud Services pro to místní počítač. mapování Hello je založena na paměť virtuálního hello místní počítače, počet disků nebo jader nebo síťové adaptéry a IOPS pro čtení a zápis. Hello doporučení je vždy hello nejnižší velikost virtuálního počítače Azure odpovídající všechny vlastnosti virtuálního počítače místní hello.

**Počet disků**: hello celkový počet disky virtuálního počítače (VMDKs) na hello virtuálních počítačů.

**Velikost (GB) na disku**: hello celkový počet nastavení velikosti všech disků hello virtuálních počítačů. Nástroj Hello také ukazuje hello velikost disku pro jednotlivé disky hello v hello virtuálních počítačů.

**Jader**: hello počet procesoru jader hello virtuálních počítačů.

**Paměť (MB)**: hello paměti RAM na hello virtuálních počítačů.

**Síťové adaptéry**: hello počet síťových adaptérů na hello virtuálních počítačů.

**Spouštěcí typ**: je spouštěcí typ hello virtuálních počítačů. Může to být buď BIOS, nebo EFI. Azure Site Recovery aktuálně podporuje pouze typ spuštění BIOS. Všechny virtuální počítače hello EFI spouštěcí typu jsou uvedeny v listu kompatibilní virtuální počítače.

**Typ operačního systému**: hello je typ OS hello virtuálních počítačů. Může to být Windows, Linux, nebo jiný.

## <a name="incompatible-vms"></a>Nekompatibilní virtuální počítače

![Tabulka aplikace Excel s nekompatibilními virtuálními počítači](./media/site-recovery-deployment-planner/incompatible-vms.png)

**Název virtuálního počítače**: hello název virtuálního počítače nebo IP adresu, která se používá v hello VMListFile při vygenerování sestavy. Tento sloupec uvádí také hello VMDKs, které jsou připojené toohello virtuálních počítačů. vCenter toodistinguish virtuální počítače s duplicitními názvy nebo IP adresy, názvy hello patří název hostitele ESXi hello. Hello uvedené hostitele ESXi je hello jeden kde hello virtuálního počítače byl umístěn při zjištění hello nástroj během hello profilace období.

**Virtuální počítač kompatibility**: označuje, proč hello zadaný virtuální počítač není kompatibilní pro použití službou Site Recovery. Hello důvodů jsou popsané pro každý nekompatibilní disk hello virtuálních počítačů a, na základě na publikované [limity úložiště](https://aka.ms/azure-storage-scalbility-performance), může být hello následující:

* Disk je větší než 4 095 GB. Azure Storage v současné době nepodporuje disky větší než 4 095 GB.
* Disk s operačním systémem je větší než 2 048 GB. Azure Storage v současné době nepodporuje disky s operačním systémem větší než 2 048 GB.
* Typ spuštění je EFI. Azure Site Recovery aktuálně podporuje pouze virtuální počítač s typem spuštění BIOS.

* Celková velikost virtuálního počítače (replikace + TFO) přesahuje omezení velikosti účet úložiště hello podporované (35 TB). Tato nekompatibilita obvykle dochází, když jediný disk v hello virtuálních počítačů má funkční vlastnost, která překračuje hello maximální podporovaný Azure nebo Site Recovery limit pro standardní úložiště. Tyto instance doručí hello virtuální počítač do zóny úložiště premium hello. Ale hello maximální podporovaná velikost prémiový účet úložiště je 35 TB a jeden chráněný virtuální počítač nelze chránit napříč více účtů úložiště. Všimněte si, že pokud je na chráněný virtuální počítač je provést testovací převzetí služeb, běží v hello stejný účet úložiště, kde je pokročíte replikace. V tomto případě nastavte 2 x hello velikost hello disku pro replikaci tooprogress a testovací převzetí služeb při selhání toosucceed paralelně.
* Source IOPS exceeds supported storage IOPS limit of 5000 per disk (Počet zdrojových IOPS překračuje podporované omezení úložiště – 5 000 IOPS na disk).
* Source IOPS exceeds supported storage IOPS limit of 80,000 per VM (Počet zdrojových IOPS překračuje podporované omezení úložiště – 80 000 IOPS na virtuální počítač).
* Průměr dat změn překračuje podporovaný limit změn dat Site Recovery 10 MB/s pro průměrnou velikost vstupně-výstupních operací pro hello disk.
* Celková data změn na všech discích na hello virtuálního počítače překračuje limit o Site Recovery data změn hello maximální podporované 54 Mb/s na virtuální počítač.
* Průměrná efektivní zápisu IOPS překračuje limit IOPS obnovení lokality hello podporované 840 pro disk.
* Ukládání počítané snímků překračuje limit úložiště snímků hello podporované 10 TB.

**R/W IOPS (s koeficient růstu)**: hello zatížení ve špičce IOPS na disku hello (výchozí hodnota je 95. percentil), včetně budoucí koeficient růstu hello (výchozí hodnota je 30 procent). Všimněte si, že hello celkový pro čtení a zápis IOPS hello virtuálního počítače není vždy hello součet hello Virtuálního počítače jednotlivé disky pro čtení a zápis IOPS, protože hello ve špičce pro čtení a zápis IOPS hello virtuální počítač je ve špičce hello hello Sum jeho jednotlivých disků pro čtení a zápis IOPS během každou minutu hello profilace období.

**Data změn v MB/s (s koeficient růstu)**: hello ve špičce klidové vytížení na disku hello (výchozí 95. percentil) včetně hello budoucí růst faktor (výchozí 30 procent). Všimněte si, že hello mísení dat celkový Dobrý den virtuálního počítače není vždy hello součet hello Virtuálního počítače jednotlivé disky změn dat, protože změn dat ve špičce hello Dobrý den virtuální počítač je ve špičce hello hello Sum z jeho jednotlivé disky změn během každou minutu hello profilace období.

**Počet disků**: hello celkový počet VMDKs na hello virtuálních počítačů.

**Velikost (GB) na disku**: hello celkový počet nastavení velikosti všech disků hello virtuálních počítačů. Nástroj Hello také ukazuje hello velikost disku pro jednotlivé disky hello v hello virtuálních počítačů.

**Jader**: hello počet procesoru jader hello virtuálních počítačů.

**Paměť (MB)**: hello množství paměti RAM na hello virtuálních počítačů.

**Síťové adaptéry**: hello počet síťových adaptérů na hello virtuálních počítačů.

**Spouštěcí typ**: je spouštěcí typ hello virtuálních počítačů. Může to být buď BIOS, nebo EFI. Azure Site Recovery aktuálně podporuje pouze typ spuštění BIOS. Všechny virtuální počítače hello EFI spouštěcí typu jsou uvedeny v listu kompatibilní virtuální počítače.

**Typ operačního systému**: hello je typ OS hello virtuálních počítačů. Může to být Windows, Linux, nebo jiný.


## <a name="site-recovery-limits"></a>Omezení Site Recovery

**Cíl ukládání replikace** | **Průměrná velikost vstupně-výstupních operací zdrojového disku** |**Průměrná četnost změn dat zdrojového disku** | **Celková denní četnost změn dat zdrojového disku**
---|---|---|---
Storage úrovně Standard | 8 kB | 2 Mb/s | 168 GB na disk
Disk úrovně Premium P10 | 8 kB | 2 Mb/s | 168 GB na disk
Disk úrovně Premium P10 | 16 kB | 4 Mb/s | 336 GB na disk
Disk úrovně Premium P10 | 32 kB nebo větší | 8 Mb/s | 672 GB na disk
Disk úrovně Premium P20 nebo P30 | 8 kB  | 5 Mb/s | 421 GB na disk
Disk úrovně Premium P20 nebo P30 | 16 kB nebo větší |10 Mb/s | 842 GB na disk

Toto jsou průměrné hodnoty za předpokladu, že se vstupně-výstupní operace z 30 % překrývají. Služba Site Recovery je schopna zpracovávat větší propustnost v závislosti na poměru překrývání, větší velikosti zápisů a skutečného chování vstupně-výstupních operací úloh. Hello předchozí čísla předpokládají typické nevyřízené položky přibližně pět minut. To znamená, že zpracování nahrávaných dat a vytvoření bodu obnovení proběhne do pěti minut od nahrání.

Tato omezení se zakládají na našich testováních, nemůžou však pokrýt všechny možné kombinace vstupně-výstupních operací aplikace. Skutečné výsledky se můžou lišit v závislosti na kombinaci vstupně-výstupních operací vaší aplikace. Nejlepších výsledků dosáhnete i po plánování nasazení vždy doporučujeme provést rozsáhlé aplikace testování s využitím obrázku true výkonu hello tooget testovací převzetí služeb při selhání.

## <a name="updating-hello-deployment-planner"></a>Aktualizace nasazení planner hello
tooupdate hello nasazení planner, hello následující:

1. Stáhněte si nejnovější verzi hello hello [planner nasazení Azure Site Recovery](https://aka.ms/asr-deployment-planner).

2. Kopírování hello .zip tooa server složky, které chcete toorun ho na.

3. Rozbalte složku hello .zip.

4. Proveďte jednu z následujících hello:
 * Pokud nejnovější verzi hello neobsahuje profilování opravu a profilace již probíhá na vaší aktuální verzí hello planner, pokračujte profilace hello.
 * Pokud nejnovější verzi hello neobsahuje profilování opravu, doporučujeme zastavit profilování v aktuální verzi a restartujte hello profilace s novou verzí hello.

  >[!NOTE]
  >
  >Při spuštění profilace s hello nové verze, pass hello že stejnou výstupní cesta k adresáři, tak, aby hello nástroj připojí data profilu na hello existující soubory. Kompletní sadu PROFILOVANÉHO dat bude použit toogenerate hello sestavy. Pokud předáte adresář odlišný výstup, se vytváří nové soubory a starý profilovaným data nepoužívá toogenerate hello sestavy.
  >
  >Každé nové nasazení planner je kumulativní aktualizace souboru .zip hello. Nepotřebujete toocopy hello nejnovější toohello předchozí složce se soubory. Můžete vytvořit a použít novou složku.


## <a name="version-history"></a>Historie verzí

### <a name="131"></a>1.3.1
Aktualizováno: 19. července 2017

Je přidána následující nová funkce:

* Přidána podpora velkých disků (větších než 1 TB) v generování sestav. Teď můžete nasazení planner tooplan replikace pro virtuální počítače, které mají velikost disku větší než 1 TB (maximálně 4095 GB).
Další informace o [podpoře velkých disků v Azure Site Recovery](https://azure.microsoft.com/en-us/blog/azure-site-recovery-large-disks/)


### <a name="13"></a>1.3
Aktualizováno: 9. května 2017

Je přidána následující nová funkce:

* Přidána podpora spravovaného disku v generování sestav. Hello počet virtuálních počítačů se může tooa jedno úložiště je účet pro převzetí služeb při selhání a testovací převzetí služeb při selhání je vybrán počítanou na základě Pokud spravovaného disku.        


### <a name="12"></a>1.2
Aktualizace: 7. duben 2017

Byly přidány následující opravy:

* Přidání spouštěcí typ kontroly (systému BIOS nebo EFI) pro každý virtuální počítač toodetermine, zda hello virtuálního počítače není kompatibilní nebo není kompatibilní pro ochranu hello.
* Přidání OS zadejte informace pro každý virtuální počítač v hello kompatibilní virtuální počítače a listů kompatibilní virtuální počítače.
* Hello GetThroughput operace se teď podporuje v oblastech hello US Government a Číně Microsoft Azure.
* Bylo přidáno několik dalších kontrol požadovaných součástí pro vCenter a ESXi Server.
* Nesprávné sestavy bylo generováno při nastavení národního prostředí nastavena toonon angličtina.


### <a name="11"></a>1.1
Aktualizováno: 9. března 2017

Opravené hello následující problémy:

* Nástroj Hello nelze profilů virtuálních počítačů, pokud hello vCenter má dva nebo více virtuálních počítačů s hello stejný název nebo IP adresu ve různých hostitelích ESXi.
* Kopírování a vyhledávání je pro listů hello kompatibilní virtuální počítače a kompatibilní virtuální počítače zakázané.

### <a name="10"></a>1.0
Aktualizováno: 23. února 2017

Azure Site Recovery nasazení Planner verzi public preview 1.0 má hello následující známé problémy (toobe řešit v budoucích aktualizacích):

* Nástroj Hello funguje jenom pro scénáře VMware do Azure, ne pro nasazení technologie Hyper-V-do Azure. Technologie Hyper-V-do Azure scénářů použití hello [nástroje Plánovač kapacity technologie Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Hello GetThroughput operace není podporována v oblastech hello US Government a Číně Microsoft Azure.
* Nástroj Hello nelze profilů virtuálních počítačů, pokud hello vCenter server má dva nebo více virtuálních počítačů s hello stejný název nebo IP adresu ve různých hostitelích ESXi. V této verzi nástroje hello přeskočí profilace pro duplicitní názvy virtuálních počítačů nebo IP adresy v hello VMListFile. alternativní řešení Hello je tooprofile hello virtuálních počítačů pomocí hostiteli ESXi namísto hello vCenter serveru. Pro každého hostitele ESXi je nutné spustit jednu instanci.
