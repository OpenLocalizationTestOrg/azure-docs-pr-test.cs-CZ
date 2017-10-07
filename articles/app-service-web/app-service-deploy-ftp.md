---
title: "aaaDeploy tooAzure vaše aplikace služby App Service pomocí FTP/S | Microsoft Docs"
description: "Zjistěte, jak toodeploy tooAzure vaše aplikace služby App Service pomocí serveru FTP nebo FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 318ae79d4fae269f853ea5c3ce28353b0864131e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service-using-ftps"></a>Nasazení vaší aplikace tooAzure služby App Service pomocí FTP/S

Tento článek ukazuje, jak toouse FTP nebo FTPS toodeploy vaší webové aplikace, back-end mobilní aplikace nebo aplikace API příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Hello koncový bod FTP/S pro aplikace je již aktivní. Nezbytné tooenable FTP/S nasazení není žádná konfigurace.

> [!IMPORTANT]
> Budeme průběžně trvá kroky tooimprove zabezpečení platforma Microsoft Azure. V rámci této snahy probíhající upgrade webové aplikace je plánovaná pro oblasti Německo centrální a Německo – severovýchod. Během této bude webové aplikace zakazuje použití hello protokolu ve formátu prostého textu FTP pro nasazení. Naše zákazníky tooour doporučení je tooswitch tooFTPS pro nasazení. Při tomto upgradu, která je plánovaná pro 9/5 jsme Nečekejte jakoukoli službu, tooyour přerušení. Vážíme si můžete podporovat v této úsilí.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Krok 1: Nastavení přihlašovacích údajů nasazení

server hello FTP tooaccess pro vaši aplikaci, musíte nejdřív přihlašovací údaje nasazení. 

tooset nebo resetování najdete v části přihlašovací údaje nasazení, [přihlašovací údaje nasazení Azure App Service](app-service-deployment-credentials.md). Tento kurz představuje hello používání přihlašovacích údajů uživatele.

## <a name="step-2-get-ftp-connection-information"></a>Krok 2: Získání informace o připojení FTP

1. V hello [portál Azure](https://portal.azure.com), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Vyberte **přehled** v levé nabídce hello, poznamenejte si hodnoty hello **uživatel FTP/nasazení**, **název hostitele FTP**, a **FTPS název hostitele**. 

    ![Informace o připojení FTP](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Hello **uživatel FTP/nasazení** hodnota uživatele, jak se zobrazuje při hello portálu Azure, včetně názvu aplikace hello v pořadí tooprovide správný kontext hello FTP serveru.
    > Můžete najít hello stejné informace, když vyberete **vlastnosti** v levé nabídce hello. 
    >
    > Navíc se nikdy zobrazuje hello nasazení heslo. Pokud zapomenete heslo nasazení, přejděte zpátky příliš[krok 1](#step1) a resetování hesla nasazení.
    >
    >

## <a name="step-3-deploy-files-tooazure"></a>Krok 3: Nasazení souborů tooAzure

1. Z vašeho klienta FTP ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client)atd), použijte informace o připojení hello, které jste shromáždili tooconnect tooyour aplikace.
3. Kopírování souborů a jejich odpovídajících directory struktura toohello [ **/web/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) v Azure (nebo hello **/lokality/wwwroot/App_Data/úlohy/** adresář pro Webové úlohy).
4. Procházet tooyour aplikace URL tooverify hello aplikace běží správně. 

> [!NOTE] 
> Na rozdíl od [nasazení na základě Git](app-service-deploy-local-git.md), FTP nasazení nepodporuje hello následující automatizaci nasazení: 
>
> - obnovení závislosti (například NuGet, NPM, PIP a autora automatizaci)
> - kompilace rozhraní .NET binárních souborů
> - generování souboru Web.config (tady je [Node.js příklad](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Musíte obnovit, sestavení a generovat tyto potřebné soubory ručně na místním počítači a nasadit je spolu s vaší aplikace.
>
>

## <a name="next-steps"></a>Další kroky

Pro pokročilejší scénáře nasazení, zkuste [nasazení tooAzure s Gitem](app-service-deploy-local-git.md). Nasazení na základě Git tooAzure umožňuje verzí, obnovení balíčků, MSBuild a další.

## <a name="more-resources"></a>Další materiály

* [Vytvoření webové aplikace PHP MySQL a nasadit pomocí FTP](web-sites-php-mysql-deploy-use-ftp.md).
* [Přihlašovací údaje pro nasazení služby Azure App Service](app-service-deploy-ftp.md)
