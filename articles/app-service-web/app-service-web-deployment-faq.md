---
title: "aaaDeployment nejčastější dotazy pro webové aplikace Azure | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se nasazení pro hello funkce Web Apps služby Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 566e1d7028e678f9679200f436118d27dfb07079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Nejčastější dotazy k nasazení pro webové aplikace v Azure

Tento článek obsahuje odpovědi toofrequently kladené dotazy (FAQ) týkající se problémy při nasazení pro hello [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>I se právě Začínáme s webovými aplikacemi App Service. Jak lze publikovat vlastní kód?

Zde jsou některé možnosti pro publikování kódu webové aplikace:

*   Nasazení pomocí sady Visual Studio. Pokud máte hello řešení sady Visual Studio, klikněte pravým tlačítkem na projekt webové aplikace hello a potom vyberte **publikovat**.
*   Nasazení pomocí klienta FTP. V hello portálu Azure, stažení hello profil publikování se pro webovou aplikaci hello má toodeploy váš kód. Potom obsah nahrajete hello soubory too\site\wwwroot pomocí hello stejné Publikovat profil FTP přihlašovací údaje.

Další informace najdete v tématu [nasazení vaší aplikace tooApp služby](web-sites-deploy.md).

## <a name="i-see-an-error-message-when-i-try-toodeploy-from-visual-studio-how-do-i-resolve-this"></a>I když se pokouším toodeploy ze sady Visual Studio zobrazí chybová zpráva. Jak to lze vyřešit?

Pokud se zobrazí následující zprávu hello, pravděpodobně používáte starší verzi hello SDK: "při nasazení pro prostředek"YourResourceName"ve skupině prostředků 'YourResourceGroup' došlo k chybě: MissingRegistrationForLocation: hello předplatné není zaregistrované pro Hello typ prostředku "součástmi" v umístění hello, střed USA". Zkuste se znovu zaregistrovat pro tohoto zprostředkovatele v pořadí toohave přístup toothis umístění." 

tooresolve tato chyba, upgradu toohello [nejnovější SDK](https://azure.microsoft.com/downloads/). Pokud se zobrazí tato zpráva a máte hello nejnovější SDK, odešlete žádost o podporu.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-tooapp-service"></a>Nasazení aplikace ASP.NET z Visual Studio tooApp služby
<a id="deployasp"></a>

kurz Hello [vytvořte první webové aplikace ASP.NET v Azure v pěti minutách](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-get-started/) ukazuje, jak toodeploy technologie ASP.NET web application tooa webové aplikace ve službě App Service pomocí sady Visual Studio 2015.

## <a name="what-are-hello-different-types-of-deployment-credentials"></a>Jaké jsou různé typy hello přihlašovací údaje pro nasazení?

Služby App Service podporuje dva typy přihlašovací údaje pro místní nasazením Git a FTP/S. Další informace o tom, najdete v části přihlašovací údaje nasazení tooconfigure, [nakonfigurovat přihlašovací údaje nasazení pro službu App Service](app-service-deployment-credentials.md).

## <a name="what-is-hello-file-or-directory-structure-of-my-app-service-web-app"></a>Co je hello soubor nebo adresář struktura webová aplikace služby App Service?

Informace o struktuře souborů hello aplikace služby App Service najdete v tématu [struktura souboru v Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-hello-disk-when-i-try-tooftp-my-files"></a>Jak lze vyřešit, "Chyba FTP 550 - zde není dostatek místa na disku hello" při tooFTP Moje soubory?

Pokud se zobrazí tato zpráva, je pravděpodobné, že používáte do kvóty disku v plánu hello služby pro webovou aplikaci. Může být nutné tooscale nahoru vyšší úroveň služby tooa na základě potřeb místa na disku. Další informace o cenách plány a omezení prostředků najdete v tématu [služby App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>Jak nastavit průběžné nasazování pro webovou aplikaci my služby App Service?

Můžete nastavit průběžné nasazování z několika zdrojů, včetně Visual Studio Team Services, OneDrive, Githubu, Bitbucket, Dropbox a další úložiště Git. Tyto možnosti jsou dostupné na portálu hello. [Průběžné nasazování tooApp služby](app-service-continuous-deployment.md) je užitečné kurz, který vysvětluje, jak tooset až průběžné nasazování.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>Jak odstranit problémy s průběžné nasazování z Githubu a Bitbucket?

Pomoc příčin problémů s průběžné nasazování z webu GitHub nebo Bitbucket, najdete v tématu [příčin průběžné nasazování](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-toomy-site-and-publish-my-code-how-do-i-resolve-this"></a>Nelze FTP toomy lokality a publikování vlastní kód. Jak to lze vyřešit?

tooresolve FTP problémy:

1. Ověřte, že jste zadali hello hostitele správný název a pověření. Podrobné informace o různých typech přihlašovací údaje a jak toouse, najdete v části [přihlašovací údaje nasazení](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. Ověřte, že porty hello FTP nejsou blokována bránou firewall. Tato nastavení musí mít Hello porty:
    * Port připojení řízení FTP: 21
    * Port pro připojení FTP data: 989, 10001-10300

## <a name="how-do-i-publish-my-code-tooapp-service"></a>Jak lze publikovat Moje tooApp kódu služby?

Hello Azure Quickstart je navrženou toohelp nasazení aplikace s použitím hello nasazení zásobníku a podle svého výběru. toouse hello rychlé spuštění, v hello portál Azure, přejděte příliš**nastavení** > **nasazení aplikace**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-tooapp-service"></a>Proč aplikace my někdy restartovat po nasazení tooApp služby?

toolearn o hello podmínek, za kterých nasazení aplikace může způsobit restart, najdete v části [nasazení oproti runtime problémy](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Podle popisu v článku hello, nasadí služby App Service složce se soubory toohello wwwroot. Nikdy přímo restartuje vaší aplikace.

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a>Jak integrovat kódu Visual Studio Team Services službou App Service?

Máte dvě možnosti pro průběžné nasazování pomocí Visual Studio Team Services:

*   Pomocí Git projektu. Připojte prostřednictvím služby App Service pomocí hello možnosti nasazení pro tento úložišti.
*   Pomocí projektu Team Foundation verze ovládacího prvku (TFVC). Nasazení pomocí hello sestavení agenta pro službu App Service.

Nasazení průběžné kód pro obě tyto možnosti závisí na stávajících pracovními postupy developer a postupy vrácení se změnami. Další informace najdete v těchto článcích: 

*   [Implementace průběžné nasazování vaší aplikace tooan webu Azure](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Nastavit účet Visual Studio Team Services, takže ho můžete nasadit tooa webové aplikace](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-toodeploy-my-app-tooapp-service"></a>Jak se používá protokol FTP nebo FTPS toodeploy Moje tooApp app Service?

Informace o používání protokol FTP nebo FTPS toodeploy tooApp vaší webové aplikace služby, najdete v části [nasazení vaší aplikace tooApp služby pomocí FTP nebo S](app-service-deploy-ftp.md).
