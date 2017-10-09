---
title: "řešení aaaOracle v Microsoft Azure | Microsoft Docs"
description: "Další informace o podporovaných konfiguracích a omezeních Oracle řešení v Microsoft Azure."
services: virtual-machines-linux
documentationcenter: 
manager: timlt
author: rickstercdn
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: rclaus
ms.openlocfilehash: 54dc62e76129535da7fb6f131af90c83adfec6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Řešení Oracle a jejich nasazení v Microsoft Azure
Tento článek se zabývá informace požadované toosuccesfully nasadit různá řešení Oracle na Microsoft Azure. Tato řešení jsou založené na bitové kopie virtuálních počítačů, které jsou publikované Oracle ve hello Azure Marketplace. tooget seznam aktuálně dostupných imagí, spusťte následující příkaz hello:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
Od verze 6-16-2017 hello seznamu obrázků jsou hello následující:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Tyto Image jsou považovány za "Přineste vlastní licence" a jako takový vám bude jenom účtována pro výpočty, úložiště a náklady na sítě způsobené s virtuálním počítačem.  Předpokládá se, jsou toouse správně licencovaný software Oracle a zda máte aktuální smlouvu o podpoře zavedené v databázi Oracle. Oracle má z místní tooAzure zaručit mobilita licencí. V tématu hello publikovaná [Oracle a Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Poznámka podrobnosti o mobilita licencí. 

Jednotlivce můžete také zvolit toobase svá řešení na vlastních bitových kopií se vytvořit nový v Azure nebo nahrát vlastní Image z jejich v místním prostředí.

## <a name="support-for-jd-edwards"></a>Podpora pro JD Edwards
Podle tooOracle podporu Poznámka [Doc ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) , JD Edwards EnterpriseOne verions 9.2 a vyšší jsou podporovány v **všechny veřejné Cloudová nabídka** , která splňuje jejich konkrétní `Minimum Technical Requirements` (MTR).  Budete potřebovat toocreate vlastní Image, které splňují jejich MTR specifikace kompatibility aplikace operačního systému a softwaru. 

## <a name="oracle-database-virtual-machine-images"></a>Bitové kopie virtuálních počítačů pro databázi Oracle
Oracle podporuje spuštěné verze Oracle DB 12.1 Standard a Enterprise v Azure na Image virtuálního počítače založené na Oracle Linux.  Hello nejlepšího výkonu pro produkční zatížení databáze Oracle na platformě Azure být jisti image virtuálního počítače hello tooproperly velikost a používat spravované disky, které jsou zajišťované Storage úrovně Premium. Pokyny, jak tooquickly získat Oracle DB spuštěná v Azure pomocí hello Oracle publikované image virtuálního počítače, [zkuste hello rychlý start databáze Oracle návod](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Možnosti konfigurace připojený disk

Připojené disky spoléhají na hello služby úložiště objektů Blob Azure. Každý – standardní disk je schopen teoretické maximálně přibližně 500 vstupně výstupních operací za sekundu (IOPS). Naše nabídky disku premium je upřednostňováno pro výkonově náročné úlohy databáze a můžete dosáhnout až too5000 IOps na disku. I když můžete používat jeden disk, pokud výkon, která splňuje musí – hello efektivní IOPS výkon lze zvýšit, pokud používáte více připojených disků, data databáze rozloženy je a pak použít Oracle automatické úložiště Management (ASM). V tématu [Oracle automatické úložiště – přehled](http://www.oracle.com/technetwork/database/index-100339.html) Oracle ASM konkrétní informace. Pro příklad tooinstall a nakonfigurovat Oracle ASM na virtuální počítač Azure s Linuxem – můžete zkusit hello [instalace a konfigurace Oracle automatizované úložiště správy](configure-oracle-asm.md) kurzu.

### <a name="oracle-realtime-application-cluster-rac"></a>Oracle v reálném čase aplikace clusteru (RAC)
Oracle RAC je navrženou toomitigate hello selhání jednoho uzlu v místní konfiguraci clusteru s několika uzly.  Přitom spoléhá na dva místní technologie, které nejsou nativní toohyper měřítku veřejných cloudových prostředích: vícesměrovým vysíláním sítě a sdíleného disku. Existují řešení třetích stran vytvořený společností [například FlashGrid](https://www.flashgrid.io/oracle-rac-in-azure/) napodobují tyto technologie Pokud potřebujete toodeploy Oracle RAC v Azure. 

### <a name="high-availability-and-disaster-recovery-considerations"></a>Aspekty vysoké dostupnosti a po havárii obnovení
Při použití Oracle – databáze v Azure, jste zodpovědní za implementaci vysokou dostupnost a po havárii obnovení řešení tooavoid žádné výpadky. 

Vysoká dostupnost a zotavení po havárii pro Oracle Database Enterprise Edition (bez RAC) v Azure lze dosáhnout pomocí [Data Guard, aktivní Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), nebo [Oracle Golden brány](http://www.oracle.com/technetwork/middleware/goldengate), s dvě databáze v dva samostatné virtuální počítače. Virtuální počítače by měla být v hello stejné [virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) tooensure získají přístup k sobě navzájem přes hello privátní trvalé IP adresu.  Kromě toho doporučujeme umístit hello virtuální počítače v hello stejné dostupnosti nastavte tooallow Azure tooplace do samostatné poruch domén a domén upgradu.  Budete chtít toohave geografická redundance – může mít tyto dvě databáze replikují mezi dvěma různých oblastech a připojení dvě instance hello s bránou sítě VPN.

Máme kurzu "[DataGuard Oracle implementace v Azure](configure-oracle-dataguard.md)" který vás provede hello základní nastavení postupu tootrial to v Azure.  

S Oracle Data Guard jde dosáhnout vysoké dostupnosti s primární databází v jeden virtuální počítač, sekundární databáze (pohotovostní) v jiném virtuálním počítači a nastavení mezi nimi jednosměrný replikace. Výsledkem Hello je přístup pro čtení toohello kopii databáze hello. S GoldenGate Oracle můžete nakonfigurovat obousměrnou replikaci mezi databázemi dvou hello. jak zjistit, tooset až řešení vysoké dostupnosti pro své databáze pomocí těchto nástrojů toolearn [Active Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) a [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) dokumentaci na webu Oracle hello. Pokud budete potřebovat pro čtení a zápis přístup toohello kopii hello databáze, můžete použít [Oracle aktivní Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Máme kurzu "[GoldenGate Oracle implementace v Azure](configure-oracle-golden-gate.md)" který vás provede hello seup základní postup tootrial to v Azure.

Navzdory s řešení s vysokou DOSTUPNOSTÍ a zotavení po Havárii navržen v Azure, budete chtít tooensure máte strategii zálohování v toorestore místní databáze.  Máme kurz [zálohování a obnovení databáze Oracle](oracle-backup-recovery.md) který vás provede hello základní postup pro vytvoření zálohy consistant.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Bitové kopie virtuálního počítače Oracle WebLogic Server
* **Clustering je podporována v edici Enterprise jenom.** Máte licenci Enterprise Edition WebLogic Server jenom při použití clusteringu WebLogic toouse hello. Nepoužívejte clustering s WebLogic Server Standard Edition.
* **Vícesměrové vysílání UDP není podporována.** Azure podporuje jednosměrové vysílání UDP, ale není vícesměrové nebo všesměrové vysílání. WebLogic Server je možné toorely na možnostech jednosměrového vysílání Azure UDP. Pro nejlepší výsledky spoléhat na jednosměrového vysílání UDP, doporučujeme, aby velikost clusteru WebLogic hello zachovány statické, nebo zachovány s více než 10 spravovaných serverů, které jsou součástí clusteru hello.
* **WebLogic Server očekává hello toobe veřejné a privátní porty pro T3 stejný přístup (například při použití podnikové JavaBeans).** Zvažte vícevrstvé scénář, kde aplikace služby vrstvy (EJB) běží na WebLogic Server cluster obsahuje dvě nebo více virtuálních počítačů, ve virtuální síti s názvem **SLWLS**. úroveň Hello klienta je v jiné podsíti v hello stejnou virtuální síť spuštěná jednoduchý program Java pokusu o toocall EJB ve vrstvě služby hello. Protože je nutné tooload vyrovnávání hello služby vrstvy, musí veřejný koncový bod Vyrovnávání zatížení sítě toobe vytvořené pro hello virtuální počítače v hello WebLogic serverového clusteru. Pokud se liší od hello veřejný port (například 7006:7008) hello privátní port, který zadáte, dojde k chybě například hello následující:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Je to proto, že pro všechny vzdálený přístup T3 WebLogic Server očekává portu služby Vyrovnávání zatížení hello a hello WebLogic spravovaný server port toobe hello stejné. V hello výše případu přistupuje klient hello port 7006 (port služby Vyrovnávání zatížení hello) a hello spravovaný server naslouchá na 7008 (hello privátní port). Toto omezení se vztahuje pouze pro přístup k T3, ne protokolu HTTP.

   tooavoid tento problém, použijte některý z hello následující alternativní řešení:

  * Použití hello stejné privátní a čísla veřejného portu pro zatížení vyrovnáváním koncové body vyhrazené tooT3 přístup.
  * Zahrnout hello následující parametr JVM při spouštění WebLogic Server:

         -Dweblogic.rjvm.enableprotocolswitch=true

Související informace najdete v článku **860340.1** v <http://support.oracle.com>.

* **Dynamické clustering a vyrovnávání zatížení omezení.** Předpokládejme, že chcete toouse dynamické cluster WebLogic Server a zpřístupnit ji prostřednictvím jednoho, veřejné Vyrovnávání zatížení sítě koncový bod v Azure. To lze provést tak dlouho, dokud použijete pevný port číslo, pro každý z hello spravovaných serverů (přiřazen není dynamicky z rozsahu) a nespouštět více spravovaných serverů, než je počet počítačů ke sledování správce hello (to znamená, více než jeden spravovaný server podle virt.krychle Protokolování přístupu uživatele počítače). Pokud vaše konfigurace výsledkem další servery WebLogic spuštění než virtuální počítače (to znamená, kde více sdílení instance serveru WebLogic hello stejného virtuálního počítače), pak není možné pro více než jeden z těchto instancí WebLogic Server zadané číslo portu – hello jiné na tomto virtuálním počítači se nepodaří tooa toobind servery.

   Na hello druhé straně, pokud nakonfigurujete hello správce serveru tooautomatically přiřadit jedinečná čísla portů tooits spravovaných serverů, pak Vyrovnávání zatížení není možné, protože Azure nepodporuje mapování z toomultiple privátní porty jeden veřejný port, jako by být vyžaduje se pro tuto konfiguraci.
* **Více instancí systému Weblogic Server na virtuálním počítači.** V závislosti na požadavcích vaší nasazení, můžete zvážit možnost hello spuštěním několika instancí WebLogic Server na hello stejného virtuálního počítače, pokud je virtuální počítač hello dostatečně velké na to. Například na střední velikosti virtuálního počítače, který obsahuje dvě jádra, mohli byste toorun dvě instance WebLogic serveru. Doporučujeme ale stále vyhnout Úvod do vaší architektury, který by byl případ hello, pokud se používá jenom jeden virtuální počítač, který je spuštěno více instancí serveru WebLogic jediný bod selhání systému. Pomocí aspoň dva virtuální počítače může být lepším řešením a každý z těchto virtuálních počítačů pak může spustit víc instancí WebLogic Server. Každý z těchto instancí WebLogic Server stále může být součástí hello stejného clusteru. Všimněte si, ale není aktuálně možné toouse Azure tooload vyrovnávání koncových bodů, které jsou vystavené takovýchto nasazeních WebLogic Server v rámci hello stejného virtuálního počítače, protože pro vyrovnávání zatížení Azure vyžaduje toobe serverů s vyrovnáváním zatížení hello rozdělené mezi Jedinečný virtuální počítače.

## <a name="oracle-jdk-virtual-machine-images"></a>Bitové kopie virtuálních počítačů JDK Oracle
* **JDK 6 a 7 nejnovější aktualizace.** Když vám doporučujeme používat hello nejnovější veřejné, podporovanou verzi jazyka Java (aktuálně Java 8), Azure také umožní JDK 6 a 7 bitové kopie k dispozici. To je určený pro starší aplikace, které se ještě není připravená toobe upgradovat tooJDK 8. Během aktualizace tooprevious JDK Image může nadále již nebudou k dispozici toohello veřejnosti, daný hello Microsoft partnerství s Oracle, hello JDK 6 a 7 Image poskytovaný Azure jsou určena toocontain novější neveřejný aktualizace, které běžně nabízí Oracle tooonly skupinu Oracle je podporována zákazníků. Nové verze hello JDK Image bude k dispozici v čase s aktualizované verze JDK 6 a 7.

   Hello JDK, které jsou k dispozici v tomto JDK 6 a 7 Image a hello virtuální počítače a bitové kopie, které jsou odvozené z nich, můžete použít pouze v rámci Azure.
* **64bitová verze JDK.** bitové kopie virtuálních počítačů Hello Oracle WebLogic Server a Oracle JDK bitové kopie virtuálních počítačů hello poskytovaný platformou Azure obsahují hello 64bitové verze systému Windows Server a hello JDK.

## <a name="next-steps"></a>Další kroky
Teď máte přehled o aktuální řešení Oracle na Microsoft Azure. Dalším krokem je toogo a nasadit první databáze Oracle na platformě Azure.
- Zkuste hello [vytvořit databázi Oracle na platformě Azure](oracle-database-quick-create.md) kurz tooget spuštěna.
