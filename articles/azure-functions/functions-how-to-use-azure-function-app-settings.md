---
title: "aaaConfigure nastavení aplikace funkce Azure | Microsoft Docs"
description: "Zjistěte, jak funkce tooconfigure Azure nastavení aplikace."
services: 
documentationcenter: .net
author: rachelappel
manager: erikre
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: 539e203ac449061ef3ceae5e93df3bdbb326e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-a-function-app-in-hello-azure-portal"></a>Jak toomanage funkce aplikace v hello portálu Azure 

V Azure Functions poskytuje funkce aplikace hello kontext spuštění pro jednotlivé funkce. Chování funkce aplikaci použít funkce tooall hostované danou funkci aplikace. Toto téma popisuje, jak tooconfigure a správě aplikací funkce v hello portálu Azure.

toobegin, přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se tooyour účet Azure. V panelu vyhledávání hello hello horní části portálu hello zadejte název hello funkce aplikace a vyberte ji ze seznamu hello. Po výběru funkce aplikace, najdete v části hello následující stránky:

![Přehled funkce aplikace v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>Karta nastavení aplikace – funkce

![Přehled funkce aplikace v hello portálu Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Hello **nastavení** karta je, kde můžete aktualizovat verzi modulu runtime funkce hello používá funkce aplikace. Je také kde budete spravovat hello hostitele klíčů používaných toorestrict HTTP přístup tooall funkce hostované hello funkce aplikace.

Funkce podporuje spotřeba hostování a hostování plány služby App Service. Další informace najdete v tématu [zvolte hello správné služby plán pro Azure Functions](functions-scale.md). Pro lepší předvídatelnost v plánu spotřeby hello funkce vám umožní omezit využití platformy a to nastavením denní kvóty využití, v GB sekundách. Jakmile bude dosaženo kvóty využití denní hello, hello funkce aplikace je zastavena. Aplikaci funkce zastavit v důsledku dosažení hello výdaje kvóty může být znovu zapnout z hello stejné oblasti jako zřízení hello denně výdaje kvóty. V tématu hello [Azure Functions stránce s cenami](http://azure.microsoft.com/pricing/details/functions/) podrobnosti o fakturaci.   

## <a name="platform-features-tab"></a>Karta funkcí platformy

![Karta funkcí platformy funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Funkce aplikace spustit a udržované pomocí hello platformě Azure App Service. Funkce aplikací mít jako takový přístup toomost hello funkce systému Azure základní webového hostingu platformy. Hello **funkce** karta je, kde přístup hello mnoho funkcí hello platformě App Service, můžete použít ve svých aplikacích funkce. 

> [!NOTE]
> Ne všechny funkce služby App Service jsou k dispozici při spuštění aplikace funkce na hostování plánu spotřeby hello.

Hello zbývající část tohoto tématu se zaměřuje na hello následující funkce služby App Service v hello portálu Azure, které jsou užitečné pro funkce:

+ [Editor služby App Service](#editor)
+ [Nastavení aplikace](#settings) 
+ [Console](#console)
+ [Pokročilé nástroje (Kudu)](#kudu)
+ [Možnosti nasazení](#deployment)
+ [CORS](#cors)
+ [Ověřování](#auth)
+ [Definice rozhraní API.](#swagger)

Další informace o tom, najdete v části toowork s nastavením služby App Service, [nakonfigurovat nastavení aplikace služby Azure](../app-service-web/web-sites-configure.md).

### <a name="editor"></a>Editor služby aplikace

| | |
|-|-|
| ![Funkce aplikace služby App Service editor.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | Hello editor služby App Service je editoru pokročilé v portálu, můžete použít toomodify JSON konfigurační soubory a soubory kódu agentem. Výběrem této možnosti se spustí na záložce prohlížeče samostatné s základní editor. To vám umožní toointegrate s hello Git úložiště, spuštění a ladění kódu a upravit nastavení funkce aplikace. Tohoto editoru poskytuje vylepšené vývojářské prostředí pro funkce ve srovnání s okně hello výchozí funkce aplikace.    |

![Hello editor služby App Service](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Nastavení aplikace

| | |
|-|-|
| ![Nastavení aplikace funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | Hello služby App Service **nastavení aplikace** okno je, kde můžete konfigurovat a spravovat framework verze, vzdálené ladění, nastavení aplikace a připojovací řetězce. Při integraci s jinými Azure a služby třetích stran, funkce aplikace, můžete upravit tato nastavení zde. |

![Konfigurace nastavení aplikace](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Konzola

| | |
|-|-|
| ![Funkce aplikace konzoly v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | Hello v portálu konzoly je ideální vývojáře nástroj Pokud dáváte přednost toointeract s vaší aplikací funkce z příkazového řádku hello. Běžné příkazy zahrnují adresáře a vytváření souborů a navigace, a také provádění dávkové soubory a skripty. |

![Funkce aplikace konzoly](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Pokročilé nástroje (Kudu)

| | |
|-|-|
| ![Funkce aplikace Kudu v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Hello Rozšířené nástroje pro službu App Service (také označované jako Kudu) poskytovat přístup k funkcím pro správu tooadvanced funkce aplikace. Z modulu Kudu spravovat informace o systému, nastavení aplikace, proměnné prostředí, rozšíření lokality, hlaviček protokolu HTTP a proměnných serveru. Můžete také spustit **Kudu** procházením toohello SCM koncový bod pro funkce aplikace, jako je třeba`https://<myfunctionapp>.scm.azurewebsites.net/` |

![Konfigurace modulu Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Možnosti nasazení

| | |
|-|-|
| ![Možnosti nasazení funkce aplikace v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Funkce vám umožní vyvíjet funkce kódu na místním počítači. Potom můžete nahrát tooAzure projektu aplikace vaše místní funkce. Kromě toho tootraditional odeslání na server FTP, funkce umožňuje nasadit funkce aplikace pomocí Oblíbené průběžnou integraci řešení, jako jsou Githubu, služby VSTS, Dropbox, Bitbucket a dalších. Další informace najdete v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md). tooupload ručně pomocí protokol FTP nebo místní Git, musíte také [nakonfigurovat přihlašovací údaje nasazení](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Funkce aplikace CORS v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | spuštění škodlivého kódu tooprevent v službách, služby App Service bloky volá tooyour funkce aplikací z externích zdrojů. Funkce podporuje toolet (CORS), které definujete "seznamem povolných" povolené zdroje, ze kterých může přijmout funkce vzdálené žádosti pro sdílení prostředků různého původu.  |

![Konfigurace funkce aplikace CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Ověřování

| | |
|-|-|
| ![Funkce ověřování aplikace v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Když funkce používají aktivační procedury protokolu HTTP, můžete vyžadovat volání toofirst ověřit. App Service podporuje ověřování Azure Active Directory a přihlaste se pomocí sociálních sítí, jako je Facebook, Microsoft a Twitter. Podrobnosti o konfiguraci zprostředkovatele konkrétní ověřování najdete v tématu [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md). |

![Konfigurovat ověřování pro aplikaci funkce](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>Definice rozhraní API.

| | |
|-|-|
| ![Funkce aplikace API swagger definice v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Funkce podporuje Swagger tooallow klienti toomore snadno využívat funkce aktivované protokolem HTTP. Další informace týkající se vytváření se Swagger definice rozhraní API najdete v článku [Začínáme s API Apps, ASP.NET a Swaggerem v Azure](../app-service-api/app-service-api-dotnet-get-started.md). Funkce proxy toodefine jedné plochy API můžete použít také pro víc funkcí. Další informace najdete v tématu [práce s Azure funkce proxy](functions-proxies.md). |

![Konfigurace funkce aplikace API](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Další kroky

+ [Konfigurace nastavení služby Azure App Service](../app-service-web/web-sites-configure.md)
+ [Průběžné nasazování se službou Azure Functions](functions-continuous-deployment.md)



