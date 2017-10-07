---
title: "aaaIntroduction tooAzure úložiště | Microsoft Docs"
description: "Přehled Azure Storage, úložiště dat online společnosti Microsoft v cloudu hello. Zjistěte, jak toouse hello nejlepší řešení úložiště k dispozici cloudu ve svých aplikacích."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/26/2017
ms.author: marsma
ms.openlocfilehash: dec8280c77f4b23df4c2a471e1d755e365c14e58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toomicrosoft-azure-storage"></a>Úvod tooMicrosoft Azure Storage

## <a name="overview"></a>Přehled
Azure Storage je řešení cloudového úložiště pro moderní aplikace, které jsou závislé na odolnost, dostupnost a škálovatelnost toomeet hello potřebám zákazníků hello. V tomto článku se vývojáři, profesionálové v oblasti IT a osoby odpovědné za klíčová obchodní rozhodnutí můžou dozvědět:

* Co je Azure Storage a jak se jeho výhody dají využít ve vašich cloudových, mobilních, serverových a desktopových aplikacích
* Jaké druhy dat můžete ukládat pomocí služby Azure Storage hello: blob (objekt) dat, data tabulky NoSQL, fronty zpráv a sdílené složky.
* Jak se spravuje přístup k datům tooyour ve službě Azure Storage
* Jak se chrání vaše data v úložišti Storage pomocí redundance a replikace.
* Kde další toobuild toogo vaší první aplikace Azure Storage

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!-- tooget up and running with Azure Storage quickly, see [Get started with Azure Storage in five minutes](storage-getting-started-guide.md). -->

Podrobné informace o knihovnách a dalších prostředcích pro práci s Azure Storage najdete dole v části [Další kroky](#next-steps).

## <a name="what-is-azure-storage"></a>Co je Azure Storage?
Cloudová výpočetní technika umožňuje nové scénáře pro aplikace, které pro svá data vyžadují škálovatelné, odolné a vysoce dostupné úložiště – a právě proto Microsoft vyvinul Azure Storage. V přidání toomaking je možné pro vývojáře toobuild aplikace ve velkém měřítku toosupport nové scénáře, Azure Storage taky poskytuje foundation hello úložiště pro virtuální počítače Azure, další robustnost tooits důkaz.

Azure Storage je nesmírně škálovatelná služba, proto můžete ukládat a zpracovávat stovky terabajtů dat toosupport hello velkých objemů dat scénáře vyžadují vědeckou nebo finanční analýzu a aplikace média. Nebo můžete ukládat malá množství dat pro malé firmy web hello. Kdykoli budete potřebovat cokoli, platit pouze pro hello data, která ukládáte. Azure Storage aktuálně poskytuje úložiště pro bilión jedinečných zákaznických objektů a v průměru zpracovává miliony požadavků za sekundu.

Azure Storage je flexibilní, aby bylo možné vytvořit aplikace pro velké globální cílovou skupinu a tyto aplikace škálovat podle potřeby – jak z hlediska hello množství dat uložených a hello počet požadavků na přístup. Platíte jen za to, co používáte a když to používáte.

Azure Storage používá systém automatického rozdělování do oddílů, který automaticky rozděluje datovou zátěž podle aktuálního provozu. To znamená, že jako hello nároky na vaši aplikaci zvyšovat, Azure Storage automaticky přiděluje toomeet odpovídající prostředky hello je.

Azure Storage je přístupná odkudkoli hello, world z jakéhokoli typu aplikace, zda je spuštěna v hello cloudu, na ploše hello, na místním serveru nebo na mobilním zařízení. Azure Storage můžete použít v mobilních situacích, kde hello aplikace ukládá část dat na zařízení hello a synchronizuje je s kompletními daty uloženými v cloudu hello.

Azure Storage podporuje klienty běžící na různých operačních systémech (včetně Windows a Linuxu) a různé programovací jazyky (například .NET, Java, Node.js, Python, Ruby, PHP a C++ a mobilní programovací jazyky) a usnadňuje tak vývoj. Azure Storage taky zpřístupňuje datové prostředky přes jednoduchá rozhraní REST API, které jsou k dispozici tooany klienta, který dokáže odesílat a přijímat data přes HTTP nebo HTTPS.

Azure Premium Storage nabízí podporu vysoce výkonných disků s nízkou latencí pro intenzivní úlohy náročné na oblast vstup/výstup, které běží ve virtuálních počítačích Azure. S Azure Premium Storage můžete několik trvalých datových disků tooa virtuální počítač připojit a nakonfigurovat je toomeet vašim požadavkům na výkon. Pro maximální výkon vstupů/výstupů se každý datový disk může opřít o SSD disk v Azure Premium Storage. Další informace najdete v tématu [Premium Storage: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).

## <a name="introducing-hello-azure-storage-services"></a>Představení služby Azure Storage hello
Azure storage poskytuje hello následující čtyři služby: objekt Blob úložiště, úložiště Table, Queue storage a úložiště File.

* Blob Storage ukládá nestrukturované datové objekty. Objekt blob může být jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Úložiště objektů blob je také odkazované tooas úložiště objektů.
* Table Storage ukládá strukturované datové sady. Úložiště Table je úložiště dat klíč atribut typu NoSQL, která umožňuje rychlý vývoj a rychlý přístup toolarge množství dat.
* Queue Storage poskytuje spolehlivé zasílání zpráv pro zpracování úloh a komunikaci mezi komponentami cloudových služeb.
* File Storage nabízí sdílené úložiště pro starší verze aplikace pomocí standardního protokol SMB hello. Cloudových služeb a virtuálních počítačích Azure můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím hello REST API služby File.

Účet úložiště Azure je zabezpečený účet, který poskytuje přístup k tooservices ve službě Azure Storage. Váš účet úložiště poskytuje jedinečný obor názvů hello pro vaše prostředky úložiště. Hello obrázek níže znázorňuje hello vztahy mezi prostředky úložiště Azure hello v účtu úložiště:

![Prostředky Azure Storage](./media/storage-introduction/storage-concepts.png)

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[!INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob Storage
Pro uživatele s velkým množstvím nestrukturovaných dat toostore v cloudu hello úložiště Blob nabízí cenově výhodné a škálovatelné řešení. Můžete použít objekt Blob úložiště toostore obsah jako třeba:

* Dokumenty
* Sociální data, jako jsou fotografie, videa, hudba nebo blogy
* Zálohy souborů, počítačů, databází a zařízení
* Obrázky a text pro webové aplikace
* Konfigurační dat pro cloudové aplikace
* Velké objemy dat, jako jsou protokoly a další velké datové sady

Každý objekt blob se organizuje do kontejneru. Kontejnery také poskytují snadno tooassign zabezpečení zásady toogroups objektů. Účet úložiště může obsahovat libovolný počet kontejnerů a kontejner může obsahovat libovolný počet objektů BLOB až toohello 500 TB limitu kapacity účtu úložiště hello.

Úložiště Blob nabízí tři typy objektů blob – objekty blob bloku, doplňovací objekty blob a objekty blob stránky (disky).

* Objekty blob bloku jsou optimalizované pro streamování a ukládání cloudových objektů a jsou dobrou volbou pro ukládání dokumentů, souborů médií, záloh atd.
* Append – objekty BLOB jsou podobné objektům BLOB tooblock, ale jsou optimalizované pro doplňovací operace. Doplňovací objekt blob může aktualizovat jen přidáním nového end toohello bloku. Append – objekty BLOB jsou dobrou volbou pro scénáře, jako je například protokolování, kde se nová data potřebují zapisovat pouze toohello konec objektu blob hello toobe.
* Objekty BLOB stránky jsou optimalizované pro zastoupení disků IaaS a podporují náhodné zapíše a může být až velikost too1 TB. Disk IaaS připojení přes síť k virtuálnímu počítači Azure je virtuální pevný disk uložený jako objekt blob.

U velkých datových sad, kde omezení sítě zkontrolujte nahrávání nebo stahování dat tooBlob úložiště přes přenosu hello reálné můžete dodávat tooimport tooMicrosoft pevném disku nebo exportovat data z hello datového centra. V tématu [použít hello službu Microsoft Azure Import/Export tooTransfer Data tooBlob úložiště](storage-import-export-service.md).

## <a name="table-storage"></a>Úložiště Table

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Moderní aplikace často potřebují datová úložiště s větší škálovatelností a flexibilitou, než potřebovaly starší generace softwaru. Úložiště Table nabízí vysoce dostupné, enormně škálovatelné úložiště, tak, aby vaše aplikace může automaticky škálovat toomeet žádost uživatele. Úložiště Table je úložiště Microsoftu na bázi NoSQL typu klíč/atribut – a na rozdíl od tradičních relačních databází je bez schématu. S datovým úložištěm je snadné tooadapt data podle hello potřebují měnícím vaší aplikace. Úložiště Table je snadno toouse, takže vývojáři můžou aplikace vytvářet rychle. Toodata přístup je rychlý a nákladově efektivní pro všechny typy aplikací.  Využívání úložiště Table Storage obvykle znamená výrazně nižší náklady než tradiční SQL pro podobné objemy dat.

Úložiště Table je úložiště typu klíč-atribut – to znamená, že každá hodnota v tabulce je uložená se typovým názvem vlastnosti. Název vlastnosti Hello můžete použít pro filtrování a upřesnění kritérií výběru. Kolekce vlastností a jejich hodnot tvoří entitu. Vzhledem k tomu, že úložiště Table nemá schéma, hello dvě entity ve stejné tabulce můžou obsahovat různé kolekce vlastností a tyto vlastnosti můžou být různých typů.

Můžete vytvořit tabulku úložiště toostore flexibilních datových sad, například uživatelských dat pro webové aplikace, adresáře, informace o zařízení a jiný typ metadat, které vaše služba vyžaduje.  V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až toohello limitu kapacity účtu úložiště hello.

Podobně jako objekty BLOB a fronty vývojáři spravovat a přístupu k Table storage pomocí standardních protokolů REST, ale tabulka úložiště také podporuje podmnožinu protokolu OData hello, což výrazně zjednodušuje možnosti pokročilého dotazování a povolení JSON a AtomPub (založeného na XML) formáty.

Pro dnešní aplikace založené na Internetu databáze NoSQL, jako je úložiště Table nabízí Oblíbené alternativní tootraditional relačních databází.

## <a name="queue-storage"></a>Queue Storage
Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle. Queue storage poskytuje spolehlivé řešení zasílání zpráv pro asynchronní komunikaci mezi součástmi aplikací, zda jsou spuštěny v hello cloudu, na ploše hello, na místním serveru nebo na mobilním zařízení. Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.

Účet úložiště může obsahovat libovolný počet front. Fronta může obsahovat libovolný počet zpráv až toohello limitu kapacity účtu úložiště hello. Jednotlivé zprávy můžou mít až velikost too64 KB.

## <a name="file-storage"></a>File Storage
Hello službu Azure souborů umožňuje tooset až vysoce dostupné síťové sdílené složky, které jsou přístupné pomocí hello standardní protokol Server Message Block (SMB). Prostředky, které můžete sdílet víc virtuálních počítačů hello stejné soubory se pro čtení i zápis. Můžete si také přečíst hello soubory pomocí rozhraní REST hello nebo knihovny klienta úložiště hello.

Jeden z věcí, které slouží k rozlišení Azure File storage z podnikové sdílené složky souborů je, že vám přístup k souborům hello, z libovolného místa v hello, world pomocí adresy URL, která ukazuje soubor toohello a obsahuje token sdílený přístupový podpis (SAS). Můžete vygenerovat tokeny SAS; umožňují privátní asset tooa konkrétní přístup pro určité množství času.

Sdílené složky můžete použít pro řadu běžných scénářů:

* Mnoho místních aplikací používá sdílené složky. Tato funkce umožňuje snazší toomigrate aplikací, u nichž sdílet data tooAzure. Pokud připojíte hello toohello sdílené složky souboru stejné písmeno, který hello jednotky místní aplikace používá, hello součástí aplikace, který přistupuje k hello sdílené složky by měly spolupracovat s minimální, pokud existuje, změny.

* Ve sdílené složce je možné ukládat konfigurační soubory, kde k nim bude mít přístup více virtuálních počítačů. Nástroje, které používají více vývojáři ve skupině mohou být uloženy ve sdílené složce, zajistíte, že každý uživatel, najít a že používají hello stejnou verzi.

* Diagnostické protokoly, metriky a výpisy stavu systému jsou jenom tři příklady data, která můžete zapsat tooa sdílené složky a zpracovat nebo analyzovat později.

V tento čas, ověřování založené na službě Active Directory a přístupu nejsou podporovány seznamy řízení (ACL), ale nebudou se někdy v budoucnu hello. přihlašovací údaje účtu úložiště Hello jsou použité tooprovide ověřování pro přístup k toohello sdílené složky. To znamená, každý, kdo má sdílenou složku hello připojené bude mít čtení/zápisu přístup toohello sdílené složky.

## <a name="access-tooblob-table-queue-and-file-resources"></a>TooBlob přístup, tabulky, fronty a soubor prostředky
Ve výchozím nastavení můžete pouze hello vlastníka účtu úložiště přístup k prostředkům v účtu úložiště hello. Hello zabezpečení vašich dat musí být ověřeny každý požadavek směřovaný na prostředky ve vašem účtu. Ověření používá model sdíleného klíče. Objekty BLOB může být také nakonfigurované toosupport anonymní ověřování.

K vašemu účtu se při vytvoření přiřadí dva privátní přístupové klíče, které se používají pro ověření. Dva klíče zajistí, že vaše aplikace bude k dispozici, když pravidelně obnovit hello klíče jako běžnou praxí při zabezpečení správy klíčů.

Pokud potřebujete tooallow uživatele řídí přístup k tooyour úložiště prostředkům, můžete vytvořit sdílený přístupový podpis. Sdílený přístupový podpis (SAS) je token, který může být adresa URL připojením tooa, umožňuje Delegovaný přístup tooa úložiště prostředků. Každý, kdo má hello token můžete přístup k prostředku hello odkazuje toowith hello oprávnění, která určuje, dobu hello je to platný. Azure Storage od verze 2015-04-05 podporuje dva druhy sdílených přístupových podpisů: SAS služby a SAS účtu.

Delegáti SAS služby Hello přístup k prostředku tooa jen v jedné služby úložiště hello: hello služby objektů Blob, fronty, tabulka nebo souboru.

SAS účtu deleguje přístup tooresources v jednom nebo více služeb úložiště hello. Můžete delegovat operace tooservice úroveň přístupu, které nejsou dostupné se SAS služby. Můžete taky delegovat přístup tooread, zápisu a operace odstranění pro kontejnery objektů blob, tabulky, fronty a sdílených složek, které se nedá vymezit přes SAS služby.

Nakonec můžete nastavit, že kontejner a jeho objekty blob, nebo třeba jen konkrétní objekt blob, jsou veřejně přístupné. Když nějaký kontejner nebo objekt blob označíte jako veřejně přístupný, kdokoli si ho může anonymně přečíst bez nutnosti ověření.  Veřejné kontejnery a objekty blob se hodí pro zpřístupnění prostředků, jako jsou média nebo dokumenty, které jsou hostované na webových stránkách.  latence sítě toodecrease pro globální cílovou skupinu, do mezipaměti můžete ukládat data objektů blob, které jsou používány weby s hello Azure CDN.

Další informace o sdílených přístupových podpisech najdete v tématu [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md). V tématu [spravovat toocontainers anonymní přístup pro čtení a objekty BLOB](storage-manage-access-to-resources.md) a [ověřování pro služby úložiště Azure hello](https://msdn.microsoft.com/library/azure/dd179428.aspx) Další informace o účtu úložiště tooyour zabezpečený přístup.

## <a name="replication-for-durability-and-high-availability"></a>Replikace pro odolnost a vysokou dostupnost
Hello data v Microsoft Azure účet úložiště se vždy replikují tooensure odolnost a vysokou dostupnost. Replikace kopie dat, buď v rámci hello stejném datovém centru nebo tooa druhý datového centra, v závislosti na možnosti replikace, kterou zvolíte. Replikace chrání vaše data a chrání vaše aplikace doby provozu v případě hello selhání hardwaru. Pokud vaše data se replikují tooa druhý datového centra softwaru, který také chrání vaše data před závažné chybě v primárním umístění hello.

Replikace zajišťuje, že váš účet úložiště splňuje hello [smlouvy úrovně služeb (SLA) pro úložiště](https://azure.microsoft.com/support/legal/sla/storage/) i v hello vzhled chyb. Zjistit informace o službě Azure Storage hello SLA zaručuje pro odolnost a dostupnost.

Při vytváření účtu úložiště, můžete vybrat jednu z hello následující možnosti replikace:

* **Místně redundantní úložiště (LRS):** Místně redundantní úložiště udržuje tři kopie dat. LRS se replikuje třikrát v rámci jednoho datového centra v jedné oblasti. LRS chrání vaše data před běžnými výpadky hardwaru, ale nikoli z hello selhání jednoho datového centra.

    LRS se nabízí se slevou. Pro maximální odolnost doporučujeme použít geograficky redundantní úložiště popsané dole.
* **Zónově redundantní úložiště (ZRS):** Zónově redundantní úložiště udržuje tři kopie dat. ZRS se replikuje třikrát v rámci dvou toothree zařízení, v jedné oblasti nebo v rámci dvou oblastí, nabízí tak větší odolnost než LRS. ZRS zajistí, aby vaše data byla odolná v jedné oblasti.

    ZRS poskytuje větší odolnost než LRS, ale pro maximální odolnost doporučujeme použít geograficky redundantní úložiště popsané dole.

  > [!NOTE]
  > ZRS je aktuálně dostupné jen pro objekty blob bloku a podporuje se od verze 2014-02-14.
  >
  > Po vytvoření účtu úložiště a vybrané ZRS, nedá se převést toouse tooany jiných typu replikace, nebo naopak.
  >
  >
* **Geograficky redundantní úložiště (GRS):** GRS udržuje šest kopií dat. S GRS data se replikují třikrát v rámci primární oblasti hello a také replikuje třikrát v sekundární oblasti stovky miles od primární oblasti hello, takže poskytuje nejvyšší úroveň odolnosti hello. V případě selhání v primární oblasti hello hello Azure Storage bude sekundární oblasti toohello převzetí služeb při selhání. GRS zajistí, aby vaše data byla odolná ve dvou oblastech.

    Informace o primárních a sekundárních párech podle oblastí najdete v článku [Oblasti Azure](https://azure.microsoft.com/regions/).
* **Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS):** Geograficky redundantní úložiště s přístupem pro čtení replikuje vaše data tooa sekundárního geografického umístění a také poskytuje přístup pro čtení tooyour data v hello sekundárního umístění. Geograficky redundantní úložiště s přístupem pro čtení můžete tooaccess data z hello primární nebo sekundární umístění hello hello události, že jedno umístění nedostupné. Geograficky redundantní úložiště s přístupem pro čtení je hello výchozí možnost pro váš účet úložiště ve výchozím nastavení při jeho vytvoření.

  > [!IMPORTANT]
  > Jak vaše data se replikují po vytvoření účtu úložiště, pokud jste zadali při vytváření účtu hello ZRS, můžete změnit. Všimněte si však, se vám účtovat poplatek za Pokud přejdete z LRS tooGRS nebo RA-GRS další jednorázové data přenos.
  >
  >

Další informace o replikaci úložiště najdete v tématu [Replikace Azure Storage](storage-redundancy.md).

Informace o cenách a sazbách pro jednotlivé způsoby replikace úložiště najdete v tématu [Azure Storage – Ceny](https://azure.microsoft.com/pricing/details/storage/). V článku [Oblasti Azure](https://azure.microsoft.com/regions/#services) najdete další informace o tom, které služby jsou dostupné v jednotlivých oblastech.

Další informace o odolnosti z hlediska architektury v Azure Storage najdete v příspěvku [Studie SOSP - Azure Storage: Služba vysoce dostupného cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

## <a name="transferring-data-tooand-from-azure-storage"></a>Přenos dat tooand ze služby Azure Storage
Blob toocopy nástroj příkazového řádku AzCopy hello, souborů a dat v tabulce můžete použít v rámci účtu úložiště nebo mezi různými účty úložiště. V tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md) Další informace.

AzCopy je postavený na hello [knihovna pro přesun dat Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), která je aktuálně dostupná ve verzi preview.

Hello služba Azure Import/Export nabízí způsob tooimport blob dat do nebo exportovat data objektů blob z účtu úložiště prostřednictvím pevný disk disku, který uživatel zašle toohello datového centra Azure. Další informace o hello službu Import/Export najdete v tématu [použít hello službu Microsoft Azure Import/Export tooTransfer Data tooBlob úložiště](storage-import-export-service.md).

## <a name="pricing"></a>Ceny
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Rozhraní API, knihovny a nástroje služby Storage
Prostředky Azure Storage jsou dostupné přes jakýkoli jazyk, který umí vytvářet požadavky HTTP/HTTPS. Azure Storage dále nabízí programovací knihovny pro několik oblíbených jazyků. Tyto knihovny zjednodušují spoustu aspektů práce s Azure Storage, protože se starají o drobnosti jako synchronní a asynchronní vyvolání, dávkování operací, řízení výjimek, automatické opakování pokusů, operační chování atd. Knihovny jsou aktuálně dostupné pro hello následující jazyky a platformy, s jinými uživateli v hello kanál:

### <a name="azure-storage-data-services"></a>Datové služby Azure Storage
* [REST API služby Storage](http://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Klientská knihovna pro úložiště pro .NET, Windows Phone a Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Klientská knihovna pro úložiště pro C++](https://github.com/Azure/azure-storage-cpp)
* [Klientská knihovna pro úložiště pro Javu/Android](https://azure.microsoft.com/develop/java/)
* [Klientská knihovna pro úložiště pro Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Klientská knihovna pro úložiště pro PHP](https://azure.microsoft.com/develop/php/)
* [Klientská knihovna pro úložiště pro Ruby](https://azure.microsoft.com/develop/ruby/)
* [Klientská knihovna pro úložiště pro Python](https://azure.microsoft.com/develop/python/)
* [Rutiny pro úložiště pro PowerShell 1.0](/powershell/module/azurerm.storage/#storage)

### <a name="azure-storage-management-services"></a>Služby správy pro Azure Storage
* [REST API pro poskytovatele prostředků úložiště – referenční informace](/rest/api/storagerp/)
* [Klientská knihovna pro .NET pro poskytovatele prostředků úložiště](/dotnet/api/microsoft.azure.management.storage)
* [Rutiny pro PowerShell 1.0 pro poskytovatele prostředků úložiště](/powershell/module/azure.storage)
* [REST API pro správu služeb úložiště (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Služby pro přesun dat v Azure Storage
* [REST API pro službu Import/export úložiště](storage-import-export-service.md)
* [Klientská knihovna pro .NET pro přesun dat v úložišti](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Nástroje
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.
* [Klientské nástroje pro Azure Storage](storage-explorers.md)
* [Sady SDK a nástroje Azure](https://azure.microsoft.com/tools/)
* [Emulátor úložiště Azure](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [Nástroj příkazového řádku AzCopy](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Další kroky
Další informace o Azure Storage, toolearn těchto materiálech:

### <a name="documentation"></a>Dokumentace
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Vytvoření účtu úložiště](storage-create-storage-account.md)

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!--* [Get started with Azure Storage in five minutes](storage-getting-started-guide.md)
-->

### <a name="for-administrators"></a>Pro správce
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití Azure CLI s Azure Storage](storage-azure-cli.md)

### <a name="for-net-developers"></a>Pro vývojáře v rozhraní .NET
* [Začínáme s úložištěm Azure Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
* [Začínáme s úložištěm Azure Table pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md)
* [Začínáme s úložištěm Azure Queue pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md)
* [Začínáme s úložištěm Azure File ve Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Pro vývojáře v Javě a Androidu
* [Jak toouse úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Javy](storage-java-how-to-use-table-storage.md)
* [Jak toouse úložiště Queue z Javy](storage-java-how-to-use-queue-storage.md)
* [Jak toouse úložiště File z Javy](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Pro vývojáře v Node.js
* [Jak toouse úložiště objektů Blob z Node.js](storage-nodejs-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Node.js](storage-nodejs-how-to-use-table-storage.md)
* [Jak toouse úložiště Queue z Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Pro vývojáře v PHP
* [Jak toouse úložiště objektů Blob z PHP](storage-php-how-to-use-blobs.md)
* [Jak toouse úložiště Table z PHP](storage-php-how-to-use-table-storage.md)
* [Jak toouse úložiště Queue z PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Pro vývojáře v Ruby
* [Jak toouse úložiště objektů Blob z Ruby](storage-ruby-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Ruby](storage-ruby-how-to-use-table-storage.md)
* [Jak toouse úložiště Queue z Ruby](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Pro vývojáře v Pythonu
* [Jak toouse úložiště objektů Blob z Pythonu](storage-python-how-to-use-blob-storage.md)
* [Jak toouse úložiště Table z Pythonu](storage-python-how-to-use-table-storage.md)
* [Jak toouse úložiště Queue z Pythonu](storage-python-how-to-use-queue-storage.md)
* [Jak toouse úložiště File z Pythonu](storage-python-how-to-use-file-storage.md)
