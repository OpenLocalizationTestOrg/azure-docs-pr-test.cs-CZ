---
title: "aaaAzure SDK pro .NET poznámky k verzi 2.8"
description: "Poznámky k verzi sady Azure SDK pro .NET 2.8"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 2722dcdd27bf9ab65b48e91dcdb545536f987dd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK pro .NET 2.8, 2.8.1 a 2.8.2
## <a name="overview"></a>Přehled
Tento článek obsahuje poznámky k verzi hello (které zahrnuje známé problémy a nejnovější změny) pro hello Azure SDK pro .NET 2.8, 2.8.1 a 2.8.2 uvolní. 

Úplný seznam nových funkcí a aktualizace provedené v této verzi najdete v tématu hello [2.8 Azure SDK pro Visual Studio 2013 a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) oznámení. 

## <a name="azure-sdk-for-net-28"></a>Azure SDK pro .NET 2.8
### <a name="download-azure-sdk-for-net-28"></a>Stáhněte si sadu Azure SDK pro .NET 2.8
[Azure SDK pro .NET 2.8 pro Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK pro .NET 2.8 pro Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>Podpora rozhraní .NET 4.5.2
#### <a name="known-issues"></a>Známé problémy
Azure .NET SDK 2.8 umožňuje vám toocreate .NET 4.5.2 balíčky cloudové služby. Ale 4.5.2 rozhraní .NET framework nebude nainstalován do hello výchozí verzi bitové kopie operačního systému hosta dokud leden 2016 hostovaného operačního systému. Před této, 4.5.2 rozhraní .NET framework bude k dispozici prostřednictvím samostatné verze operačního systému hosta verze – listopad 2015-02. V tématu hello [verzí hostovaného operačního systému Azure a kompatibilních sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md) stránky tootrack při hello bitové kopie, budou vydané.  Po vydání hello listopad 2015-02 image můžete toouse této bitové kopie aktualizací Cloudová služba konfiguračním souboru (.cscfg) souboru. V konfiguraci služby hello souboru nastavit atribut osVersion hello hello objekt ServiceConfiguration element toohello řetězec "WA-Host-OS-4.26_201511-02". Pokud se rozhodnete tooopt v toouse tuto bitovou kopii se už získat automatické aktualizace toohello hostovaného operačního systému. tooget hello automatické aktualizace hello osVersion musí být nastaven příliš "*" a .NET 4.5.2 bude k dispozici pouze prostřednictvím funkce Automatické aktualizace v lednu 2016.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Známé problémy
Během **šablony objekt pro vytváření dat** projektu vytvoření zahrnující ukázková data, skriptu azure power shell může selhat, pokud je verze azure power shell nainstalovat na počítač hello po 0.9.8.

V pořadí toosuccessfully vytvořit tento typ projektu, je nutné nainstalovat [azure power shell verzi 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Nástroje Azure Resource Manager
#### <a name="breaking-changes"></a>Nejnovější změny
v této verzi toowork s hello nové rutiny prostředí Azure PowerShell verze 1.0 byl aktualizován Hello skript prostředí PowerShell poskytované hello projekt skupiny prostředků Azure.  Tento nový skript nebude fungovat z pomocí sady Visual Studio, pokud používáte verzi předchozí too2.8 hello SDK.  

Skripty z projektů vytvořených v dřívějších verzích hello SDK se spustí z Visual Studia až pomocí hello 2,8 SDK.  Všechny skripty bude toowork mimo Visual Studio s hello příslušnou verzi hello rutin prostředí Azure PowerShell.  

Hello 2,8 SDK vyžaduje verzi 1.0 hello rutin prostředí Azure PowerShell.  Všech ostatních verzí hello sady SDK vyžadovat verzi 0.9.8 hello rutin prostředí Azure PowerShell.  Další informace najdete v části [to](http://go.microsoft.com/fwlink/?LinkID=623011) blogu.

### <a name="web-tools-extensions"></a>Rozšíření nástrojů Web
#### <a name="known-issues"></a>Známé problémy
v následující verzi hello budou řešeny Hello následující známé problémy.

* Služby App Service související cloudu a Průzkumníka serveru gesto pro nevýrobní prostředí (například Azure China nebo zákazníků Azure zásobníku) nebudou fungovat. Pro zákazníky v těchto oblastech ovlivněné stahování hello publikování profil z hello portál Azure vám umožní publikování možnost. Budoucí verze bude opravit gesta například "Připojit ladicí program" a "Zobrazit streamování protokoly" pro Azure China a zásobníku zákazníků. 
* Zákazníci mohou zobrazit chyby při vytváření při hello App Insights instance toowhich, které se nasazení je v jiné oblasti, než východní USA služby App Service. V těchto scénářích vytváří aplikační službu hello portálu a stahování hello profil publikování se povolit publikování scénáře. 

### <a name="azure-hdinsight-tools"></a>Nástroje Azure HDInsight
#### <a name="new-updates"></a>Nové aktualizace
* Můžete spuštění dotazu Hive v clusteru s hello prostřednictvím HiveServer2 s téměř žádné režijní náklady a zjistit, že se úloha hello protokolech reálném čase.
* Pomocí hello nové Hive zobrazení spouštění úlohy můžete proniknout do vašeho úlohy hlubší, najdete další podrobnosti a identifikovat potenciální problémy.

Informace najdete v tématu [2.8 Azure SDK pro Visual Studio 2013 a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK pro .NET 2.8.1
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Známé problémy pro Visual Studio 2013 a Visual Studio 2015
1. Aktivované webové úlohy publikuje tooslots bude zobrazit a chyba a nebude sada plánu, ale předá tooAzure hello webové úlohy. Zákazníci, kteří potřebují naplánovaná úloha pak můžete použít hello portálu Azure tooset až hello plán pro hello webové úlohy. 
2. Python zákazníků může zaznamenat problémy s ladicí program. Tým služby se zavedením opravu pro tento, ale pokud zákazníci jsou ovlivněni, sdělte Microsoft vědět ve fórech hello nebo na blogu oznámení hello nebo sekci komentáře poznámky k verzi. 
3. Zákazníci v určité oblasti (například – Jih, Indie) bude mít chyby zřizování App Service. To je konzistentní s hello portál a zákazníky, kteří tomuto problému dojde, můžete použít hello Azure portálu toorequest přístup toopublish toothese geo oblasti. Jakmile požádají oblasti toothese přístup pomocí hello Azure portálu zřizování by měly fungovat. 

## <a name="azure-sdk-for-net-282"></a>Azure SDK pro .NET ve verzi 2.8.2
Po instalaci hello hello ve verzi 2.8.2 nástrojů zákazníků může zaznamenat problém následující hello.         

* Pokud používáte Windows 10 a nemáte nainstalovanou aplikaci Internet Explorer, může dojít k chybě "Internet Explorer nebyla nalezena".
  tooresolve hello problém, nainstalujte aplikaci Internet Explorer pomocí hello přidat nebo odebrat součásti systému Windows dialogu.

Pokud zjistíte tento problém, použijte hello odesílání smajlíka funkce tooreport ho.

Další informace najdete v tématu [to](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) post.

## <a name="other-updates"></a>Další aktualizace
Další aktualizace, najdete v části [post oznámení Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Viz také
[Azure SDK 2.8 oznámení post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Podporu a informace o vyřazení pro hello Azure SDK pro .NET a rozhraní API](https://msdn.microsoft.com/library/azure/dn479282.aspx)

