---
title: "App Service API Apps – co se změnilo | Microsoft Docs"
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
ms.openlocfilehash: e4e25f2cd1d39bb0113e3fe2bc37120f92227b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-api-apps---whats-changed"></a>App Service API Apps – co se změnilo
Na událost Connect() v listopadu 2015, byly množství vylepšení do služby Azure App Service [oznámeno](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Tato vylepšení zahrnují základní změny aplikace API lépe zarovnat s Mobile a webové aplikace, snížit počet koncept a zlepšit nasazení a výkon modulu runtime. Spuštění nové aplikace API 30. listopadu 2015, můžete vytvořit pomocí portálu správy Azure nebo nejnovější nástrojů se projeví tyto změny. Tento článek popisuje tyto změny a také jak znovu nasadit existující aplikace, které chcete využít výhod funkcí.

## <a name="feature-changes"></a>Funkce změny
Klíčové funkce API Apps – ověřování, CORS a rozhraní API metadat – přesunuli přímo do služby App Service. Díky této změně funkce jsou dostupné přes webové, mobilní aplikace a aplikace API. Ve skutečnosti všechny tři sdílet stejný **Microsoft.Web/sites** typ prostředku ve službě Správce prostředků. Brána aplikace API je už potřeby nebo nabízí s API Apps. To také usnadňuje používání služby Azure API Management vzhledem k tomu, že budou existovat jenom jedné API Management gateway.

![Přehled aplikace API](./media/app-service-api-whats-changed/api-apps-overview.png)

Princip klíčových návrhu s API Apps aktualizací je umožnit použití rozhraní API je, ve vámi zvolený jazyk.  Pokud vaše rozhraní API je už nasazená jako webovou aplikaci nebo mobilní aplikace, nemáte se znovu nasadit aplikaci, kterou chcete využít výhod nových funkcí. Pokud jste aktuálně ve verzi preview rozhraní API aplikace, pokyny migrace je podrobně popsán níže.

### <a name="authentication"></a>Authentication
Existující připraveného aplikace API, mobilní služby nebo aplikace a webové aplikace ověřování funkce mají byla unified a jsou k dispozici v jednom okně ověřování služby Azure App Service v portálu pro správu. Úvod do služby ověřování v App Service, najdete v části [rozšiřování App Service ověřování / autorizace](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Pro scénáře rozhraní API existuje několik relevantní nové funkce:

* **Podpora pro použití služby Azure Active Directory přímo**, bez nutnosti exchange token AAD pro token relace kód klienta: vašeho klienta zahrnout pouze tokeny AAD v hlavičce autorizace podle specifikace tokenu nosiče. Také to znamená, že žádné specifické pro aplikaci služby SDK je potřeba na straně klienta nebo serveru. 
* **Přístup k Service-to-service nebo "Interní"**: Pokud máte démona nebo jiného klienta potřebovali mít přístup k rozhraní API bez rozhraní, si můžete vyžádat token pomocí AAD instanční objekt a předejte ji do služby App Service pro ověřování pomocí vašeho aplikace.
* **Odložení autorizace**: mnoho aplikací mít různou omezení přístupu pro různé části aplikace. Možná budete chtít některé rozhraní API je veřejně dostupné, zatímco jiné vyžadují přihlášení. Původní funkce ověřování/autorizace se vyjadřuje, s celý web, které vyžadují přihlášení. Tato možnost stále existuje, ale můžete případně povolit kódu aplikace k vykreslení rozhodnutí přístup po ověření uživatele služby App Service.

Další informace o nových funkcích ověřování najdete v tématu [ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md). Informace o tom, jak migrovat stávající aplikace API z předchozí modelu aplikace API k novým najdete v tématu [migraci stávající aplikace API](#migrating-existing-api-apps) dále v tomto článku.

### <a name="cors"></a>CORS
Místo oddělený čárkami **MS_CrossDomainOrigins** aplikace nastavení, je nyní okno Konfigurace CORS na portálu správy Azure. Alternativně lze konfigurovat, pomocí nástrojů, jako je Azure PowerShell, rozhraní příkazového řádku správce prostředků nebo [Průzkumníka prostředků](https://resources.azure.com/). Nastavte **cors** vlastnost **Microsoft.Web/sites/config** typ prostředku pro vaše  **&lt;název lokality&gt;/web** prostředků. Například:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>Metadata API
V okně Definice rozhraní API je nyní k dispozici přes webové, mobilní aplikace a aplikace API. V portálu pro správu můžete zadat adresu url relativní nebo absolutní adresu url ukazující na koncový bod této reprezentace hostitelů Swagger 2.0 rozhraní API. Alternativně můžete konfigurovat, pomocí nástroje Správce prostředků. Nastavte **apiDefinition** vlastnost **Microsoft.Web/sites/config** typ prostředku pro vaše  **&lt;název lokality&gt;/web** prostředků. Například:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

V tomto okamžiku musí být veřejně přístupná bez ověřování pro mnoho podřízené klientů (například Visual Studio REST API generování klienta a PowerApps "Přidat rozhraní API" toku) ho využívají koncový bod metadat. To znamená, že pokud používáte ověřování aplikace služby a chcete vystavit definice rozhraní API z v rámci vaší aplikace, budete muset použít možnost ověřování odložení dříve popisované, aby trasy, která má Swagger metadata veřejné.

## <a name="management-portal"></a>Portál pro správu
Výběr **nové > Web + mobilní > aplikace API** portálu vytvoří aplikace API, které odráží nové funkce, které jsou popsané v článku. **Procházet > aplikace API** zobrazí pouze tyto nové aplikace API. Když přejdete do aplikace API, v okně sdílí stejné rozložení a možnosti jako webové a mobilní aplikace. Pouze rozdíly jsou rychlý start obsah a nastavení řazení.

Stávající aplikace API (nebo aplikace Marketplace API vytvořená z Logic Apps) s předchozí verzí Preview možnosti stále budou viditelné v Návrháři Logic Apps a při procházení všechny prostředky ve skupině prostředků.

## <a name="visual-studio"></a>Visual Studio
Většina nástrojů webové aplikace bude fungovat s nové aplikace API, protože budou sdílet stejnou základní **Microsoft.Web/sites** typ prostředku. Visual Studio Azure nástrojů, ale musí být neupgradovali na verzi 2.8.1 nebo novější vzhledem k tomu, že zpřístupňuje řadu funkcí, které jsou specifické pro rozhraní API. Stažení sady SDK z [stránky Azure stahování](https://azure.microsoft.com/downloads/).

S racionalizace typů služby App Service, publikování je také jednotná pod **publikovat > Microsoft Azure App Service**:

![Publikování aplikace API](./media/app-service-api-whats-changed/api-apps-publish.png)

Další informace o SDK 2.8.1, přečtěte si téma oznámení [příspěvku na blogu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Alternativně lze ručně importovat profil publikování z portálu pro správu umožňující publikování. Ale Průzkumník cloudu, generování kódu a vytváření výběr aplikace API bude vyžadovat SDK 2.8.1 nebo vyšší.

## <a name="migrating-existing-api-apps"></a>Migraci stávající aplikace API
Pokud nasadíte vlastního rozhraní API pro aplikace API v předchozí verzi Preview, žádosti o migrovat do nového modelu pro aplikace API pomocí 31. prosince 2015. Vzhledem k tomu, jak starý a nový model jsou založená na webové rozhraní API hostovaných ve službě App Service, můžete znovu použít většinu existujícího kódu.

### <a name="hosting-and-redeployment"></a>Hostování a opětovné nasazení
Postup pro opakované nasazení jsou stejné jako nasazení všechny existující webového rozhraní API do služby App Service. pomocí následujících kroků:

1. Vytvoření prázdné aplikace API. To lze provést na portálu se nový > aplikace API v sadě Visual Studio z publikovat nebo z nástroje Správce prostředků. Pokud pomocí nástroje Správce prostředků nebo šablony, nastavte **druhu** hodnotu **rozhraní api** na **Microsoft.Web/sites** typ prostředku – elementy QuickStart a nastavení v portál pro správu orientovat na rozhraní API scénáře.
2. Připojte a nasazení projektu do prázdné aplikace API pomocí kteréhokoli z nasazení mechanismů podporované službou App Service. Čtení [dokumentaci k nasazení služby Azure App Service](../app-service-web/web-sites-deploy.md) Další informace. 

### <a name="authentication"></a>Authentication
Ověřovací služby App Service podporují stejné možnosti, které byly dostupné s předchozím modelu aplikace API. Pokud používáte tokeny relace a vyžadují sady SDK, pomocí následujících klientských a serverových sad SDK:

* Klient: [mobilního klienta Azure SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
* Server: [rozšíření ověřování .NET mobilní aplikace Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Pokud jste místo toho použili alpha aplikační služby sady SDK, ty jsou nyní zastaralé:

* Klient: [SDK Microsoft Azure App Service](http://www.nuget.org/packages/Microsoft.Azure.AppService)
* Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Zejména s Azure Active Directory, ale žádné služby specifické pro aplikace je požadované Pokud používáte AAD token přímo.

### <a name="internal-access"></a>Interní přístup
Předchozí modelu aplikace API zahrnuty úroveň integrovanou interní přístupu. To vyžaduje použití sady SDK pro žádosti o podepsání. Jak je popsáno výše, s modelem nové aplikace API objekty služby AAD slouží jako náhradní pro ověřování služby služby bez nutnosti aplikaci specifickou pro službu SDK. Další informace v [objekt zabezpečení ověřování služby pro aplikace API v Azure App Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Zjišťování
Předchozí aplikace API modelu měli rozhraní API pro zjišťování dalších aplikace API v době běhu ve stejné skupině prostředků za stejnou bránou. Toto je užitečné zejména v případě architektur, které implementují mikroslužbu vzory. Když to není podporováno přímo, jsou k dispozici několik možností:

1. Použijte rozhraní API Správce prostředků Azure pro zjišťování.
2. Uveďte Azure API Management před vaše rozhraní API hostované služby App Service. Azure API Management slouží jako průčelí za a můžete zadat stabilní externí přístupných adresu url, i když se změní je interní topologie.
3. Sestavení vlastní aplikace API zjišťování a další aplikace API zaregistrovat zjišťování aplikace po spuštění.
4. V době nasazení naplnění koncových bodů API Apps nastavení aplikace všechny aplikace API (a klienty). Toto je přijatelná šablony nasazení a vzhledem k tomu, že aplikace API nyní získáte ovládací prvek adresy URL.

## <a name="using-api-apps-with-logic-apps"></a>Pomocí aplikace API s Logic Apps
Nový model aplikace API pracuje s [aplikací logiky verze schématu 2015-08-01](../logic-apps/logic-apps-schema-2015-08-01.md).

## <a name="next-steps"></a>Další kroky
Další informace, přečtěte si články v [části dokumentace k API Apps](https://azure.microsoft.com/documentation/services/app-service/api/). Byly aktualizovány tak, aby odrážela nový model pro aplikace API. Kromě toho oslovení ve fórech další podrobnosti a pokyny k migraci:

* [Fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-api-apps)

