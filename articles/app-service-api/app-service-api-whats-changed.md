---
title: "aaaApp Service API Apps - co se změnilo | Microsoft Docs"
description: "Zjistěte, co je nového pro API Apps v Azure App Service."
services: app-service\api
documentationcenter: .net
author: mohitsriv
manager: erikre
editor: tdykstra
ms.assetid: a9b58066-e8fd-48b8-a651-4613b1736433
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: rachelap
ms.openlocfilehash: 79df54f1dae91d7c5d3b66d208d0d1c1d7d55ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-api-apps---whats-changed"></a>App Service API Apps – co se změnilo
V hello Connect() událost v listopadu 2015, byly řadu vylepšení tooAzure služby App Service [oznámeno](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Tato vylepšení zahrnují základní změny tooAPI aplikace toobetter zarovnané s Mobile a webové aplikace, snižte počet koncept a zlepšit výkon nasazení a modulu runtime. Spuštění nové aplikace API 30. listopadu 2015, můžete vytvořit pomocí portálu pro správu Azure hello nebo hello nejnovější nástrojů se projeví tyto změny. Tento článek popisuje taky, jak tyto změny tooredeploy existující aplikace tootake výhod hello možností.

## <a name="feature-changes"></a>Funkce změny
klíčové funkce API Apps – ověřování, CORS a rozhraní API metadat – Hello přesunuli přímo do služby App Service. Díky této změně hello funkce jsou dostupné přes webové, mobilní aplikace a aplikace API. Ve skutečnosti všechny tři sdílené složky hello stejné **Microsoft.Web/sites** typ prostředku ve službě Správce prostředků. Brána Hello aplikace API je už potřeby nebo nabízí s API Apps. Také díky tomu jednodušší toouse Azure API Management vzhledem k tomu, že budou existovat jenom hello jedné API Management gateway.

![Přehled aplikace API](./media/app-service-api-whats-changed/api-apps-overview.png)

Princip klíčových návrhu s API Apps aktualizovat hello je tooenable toobring vaše rozhraní API, jako je ve vámi zvolený jazyk.  Pokud vaše rozhraní API je už nasazená jako webovou aplikaci nebo mobilní aplikace, není nutné tooredeploy vaší aplikace tootake výhod nových funkcí hello. Pokud jste aktuálně ve verzi preview rozhraní API aplikace, pokyny migrace je podrobně popsán níže.

### <a name="authentication"></a>Authentication
Hello existující připraveného aplikace API, mobilní služby nebo aplikace a webové aplikace ověřování funkce mají byla unified a jsou k dispozici v jednom okně ověřování služby Azure App Service na portálu pro správu hello. O službách tooauthentication Úvod ve službě App Service, najdete v části [rozšiřování aplikační služby ověřování / autorizace](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Pro scénáře rozhraní API existuje několik relevantní nové funkce:

* **Podpora pro použití služby Azure Active Directory přímo**, bez nutnosti token AAD hello tooexchange pro token relace kód klienta: vašeho klienta zahrnout pouze tokeny AAD hello v hlavičce autorizace hello, podle tokenu nosiče toohello specifikace. Také to znamená, že žádné specifické pro aplikaci služby SDK je potřeba na straně klienta nebo serveru hello. 
* **Service-to-service nebo "Interní" přístup**: Pokud máte démona nebo jiného klienta, která potřebuje přístup tooAPIs bez rozhraní, si můžete vyžádat token pomocí AAD instanční objekt a předejte ji tooApp služby pro ověřování pomocí vašeho aplikace.
* **Odložení autorizace**: mnoho aplikací mít různou omezení přístupu pro různé části hello aplikace. Můžete chtít některé rozhraní API toobe veřejně dostupné, zatímco jiné vyžadují přihlášení. funkce ověřování/autorizace původní Hello se vyjadřuje, s hello celý web, které vyžadují přihlášení. Tato možnost stále existuje, ale můžete případně povolit kódu aplikace toorender přístup rozhodnutí po aplikační služby je ověření uživatele hello.

Další informace o nových funkcích ověřování hello najdete v tématu [ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md). Informace o tom, jak toomigrate stávající aplikace API z aplikace API předchozí hello modelu toohello nové jeden, najdete v tématu [migraci stávající aplikace API](#migrating-existing-api-apps) dále v tomto článku.

### <a name="cors"></a>CORS
Místo oddělený čárkami **MS_CrossDomainOrigins** aplikace nastavení, je nyní okno hello portál pro správu Azure pro konfiguraci CORS. Alternativně lze konfigurovat, pomocí nástrojů, jako je Azure PowerShell, rozhraní příkazového řádku správce prostředků nebo [Průzkumníka prostředků](https://resources.azure.com/). Sada hello **cors** vlastnost hello **Microsoft.Web/sites/config** typ prostředku pro vaše  **&lt;název lokality&gt;/web** prostředků. Například:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>Metadata API
okno Definice rozhraní API Hello je teď dostupná přes webové, mobilní aplikace a aplikace API. V portálu pro správu hello můžete zadat adresu url relativní nebo absolutní adresu url ukazující tooan koncový bod této reprezentace hostitelů Swagger 2.0 rozhraní API. Alternativně můžete konfigurovat, pomocí nástroje Správce prostředků. Sada hello **apiDefinition** vlastnost hello **Microsoft.Web/sites/config** typ prostředku pro vaše  **&lt;název lokality&gt;/web** prostředků. Například:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

V tomto okamžiku hello metadata koncový bod potřebuje toobe veřejně přístupný bez ověřování pro mnoho podřízené klientů (například Visual Studio REST API generování klienta a toku "Přidat rozhraní API" PowerApps) tooconsume ho. To znamená, že pokud používáte ověřování aplikace služby a chcete tooexpose hello definice rozhraní API z v rámci vaší aplikace, budete potřebovat toouse hello odložení ověřování možnost dříve popisované, aby byla metadata Swagger tooyour trasy hello veřejné.

## <a name="management-portal"></a>Portál pro správu
Výběr **nové > Web + mobilní > aplikace API** v hello portál vytvoří aplikace API, které odráží hello nové funkce popsané v článku hello. **Procházet > aplikace API** zobrazí pouze tyto nové aplikace API. Když přejdete do aplikace API, sdílené složky okno hello hello stejné rozložení a možnosti jako webové a mobilní aplikace. Hello pouze rozdíly jsou rychlý start obsah a nastavení řazení.

Stávající aplikace API (nebo aplikace Marketplace API vytvořená z Logic Apps) s hello předchozí funkce Preview se stále nebude zobrazovat v Návrháři Logic Apps hello a při procházení všechny prostředky ve skupině prostředků.

## <a name="visual-studio"></a>Visual Studio
Hello většina webových aplikací nástrojů bude fungovat s nové aplikace API, protože budou sdílet stejnou základní **Microsoft.Web/sites** typ prostředku. Hello nástrojů Azure Visual Studio, ale musí být upgradovaný tooversion 2.8.1 nebo novější, protože vystavuje určitý počet tooAPIs konkrétní možnosti. Stáhnout hello SDK z hello [stránky Azure stahování](https://azure.microsoft.com/downloads/).

S hello racionalizace hello typy služby App Service, publikování je také jednotná pod **publikovat > Microsoft Azure App Service**:

![Publikování aplikace API](./media/app-service-api-whats-changed/api-apps-publish.png)

Další informace o SDK 2.8.1, čtení hello oznámení toolearn [příspěvku na blogu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Alternativně lze ručně importovat hello profilu z portálu tooenable správu hello publikování publikovat. Ale Průzkumník cloudu, generování kódu a vytváření výběr aplikace API bude vyžadovat SDK 2.8.1 nebo vyšší.

## <a name="migrating-existing-api-apps"></a>Migraci stávající aplikace API
Pokud vaše vlastní rozhraní API je předchozí verze Preview toohello nasazené aplikace API, jsme žádost migrovat toohello nový model pro aplikace API po 31. prosince 2015. Vzhledem k tomu, jak hello starý a nový model jsou založená na webové rozhraní API hostovaných ve službě App Service, můžete znovu použít hello většina existujícího kódu.

### <a name="hosting-and-redeployment"></a>Hostování a opětovné nasazení
Hello kroky pro opakované nasazení jsou hello stejné jako nasazení všechny existující tooApp webového rozhraní API služby. pomocí následujících kroků:

1. Vytvoření prázdné aplikace API. To lze provést v portálu hello s nový > aplikace API v sadě Visual Studio z publikovat nebo z nástroje Správce prostředků. Pokud pomocí nástroje Správce prostředků nebo šablony, nastavte hello **druhu** hodnota příliš**rozhraní api** na hello **Microsoft.Web/sites** quickstarts hello toohave typ prostředku a nastaveních v nástroji portál pro správu Hello orientovat na rozhraní API scénáře.
2. Připojte a nasadit projekt toohello prázdná aplikace API pomocí kteréhokoli z mechanismů nasazení hello podporované službou App Service. Čtení [dokumentaci k nasazení služby Azure App Service](../app-service-web/web-sites-deploy.md) toolearn Další. 

### <a name="authentication"></a>Authentication
Hello podpora služby App Service ověřování hello stejné funkce, které byly dostupné s předchozím modelu aplikace API hello. Pokud používáte tokeny relace a vyžadují sady SDK, pomocí hello následujících klientských a serverových sad SDK:

* Klient: [mobilního klienta Azure SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
* Server: [rozšíření ověřování .NET mobilní aplikace Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Pokud jste místo toho použili hello alpha aplikační služby sady SDK, ty jsou nyní zastaralé:

* Klient: [SDK Microsoft Azure App Service](http://www.nuget.org/packages/Microsoft.Azure.AppService)
* Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Zejména s Azure Active Directory, ale žádné služby specifické pro aplikace je požadované Pokud používáte hello AAD token přímo.

### <a name="internal-access"></a>Interní přístup
Aplikace API modelu předchozí Hello zahrnuty úroveň integrovanou interní přístupu. To vyžaduje použití hello SDK pro žádosti o podepsání. Jak je popsáno výše, s hello nový model aplikace API, objekty služby AAD slouží jako náhradní pro ověřování služby služby bez nutnosti aplikaci specifickou pro službu SDK. Další informace v [objekt zabezpečení ověřování služby pro aplikace API v Azure App Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Zjišťování
Hello modelu měli rozhraní API pro zjišťování dalších aplikace API v době běhu předchozí API Apps hello stejnou skupinu prostředků za stejnou bránou hello. Toto je užitečné zejména v případě architektur, které implementují mikroslužbu vzory. Když to není podporováno přímo, jsou k dispozici několik možností:

1. Použijte hello Azure Resource Manager API pro vyhledávání.
2. Uveďte Azure API Management před vaše rozhraní API hostované služby App Service. Azure API Management slouží jako průčelí za a můžete zadat stabilní externí přístupných adresu url, i když se změní je interní topologie.
3. Sestavení vlastní aplikace API zjišťování a další aplikace API zaregistrovat hello zjišťování aplikace po spuštění.
4. V době nasazení naplnění nastavení aplikace hello všech hello rozhraní API aplikace (a klienti) hello koncové body hello jiné aplikace rozhraní API. Toto je přijatelná šablony nasazení a vzhledem k tomu, že aplikace API nyní získáte řízení hello adresy URL.

## <a name="using-api-apps-with-logic-apps"></a>Pomocí aplikace API s Logic Apps
Hello nový model rozhraní API apps pracuje s [aplikací logiky verze schématu 2015-08-01](../logic-apps/logic-apps-schema-2015-08-01.md).

## <a name="next-steps"></a>Další kroky
toolearn víc, přečtěte si hello články v hello [části dokumentace k API Apps](https://azure.microsoft.com/documentation/services/app-service/api/). Nejsou aktualizované tooreflect hello nový model pro aplikace API. Kromě toho oslovení na fóra hello další podrobnosti a pokyny k migraci:

* [Fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-api-apps)

