---
title: "aaaHow toocreate a nasazení cloudové služby | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení cloudové služby pomocí hello portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: dc8b81a594f3514e662c49c9a46a33da8a551f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a>Jak toocreate a nasazení cloudové služby
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-create-deploy.md)
>
>

Hello portál Azure nabízí dva způsoby, abyste toocreate a nasazení cloudové služby: *rychle vytvořit* a *vytvořit vlastní*.

Tento článek vysvětluje, jak toouse hello toocreate metoda rychle vytvořit novou cloudovou službu a pak použijte **nahrát** tooupload a nasadit balíček cloudové služby v Azure. Pokud použijete tuto metodu, díky hello portál Azure k dispozici užitečné odkazy pro dokončení všechny požadavky rovnou. Pokud jste připravené toodeploy vaše cloudové služby při vytváření, můžete provést jak na hello současně pomocí vytvořit vlastní.

> [!NOTE]
> Pokud máte v plánu toopublish cloudové služby z Visual Studio Team Services (služby VSTS), použijte rychle vytvořit a potom nastavit publikování služby VSTS z řídicího panelu Azure Quickstart nebo hello hello. Další informace najdete v tématu [tooAzure nastavené průběžné doručování podle pomocí Visual Studio Team Services][TFSTutorialForCloudService], nebo naleznete v nápovědě pro hello **rychlý Start** stránky.
>
>

## <a name="concepts"></a>Koncepty
Požadované toodeploy aplikace jako cloudové služby v Azure se tři komponenty:

* **Definice služby**  
  Soubor definice Hello cloudové služby (.csdef) definuje hello model služeb, včetně hello počet rolí.
* **Konfigurace služby**  
  Hello cloudové služby konfiguračního souboru (.cscfg) poskytuje nastavení konfigurace pro hello Cloudová služba a jednotlivé role, včetně hello počet instancí role.
* **Balíček služby**  
  balíček služby Hello (.cspkg) obsahuje kód aplikace hello a konfigurace a souboru definice služby hello.

Další informace o těchto a jak toocreate balíček [zde](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Příprava aplikace
Před nasazením cloudové služby, je nutné vytvořit balíček hello cloudové služby (.cspkg) z kódu aplikace a cloudové služby konfiguračního souboru (.cscfg). Hello Azure SDK poskytuje nástroje pro přípravu tyto soubory požadované k nasazení. Hello SDK můžete nainstalovat z hello [Azure stáhne](https://azure.microsoft.com/downloads/) stránku hello jazyk, ve kterém chcete toodevelop kódu aplikace.

Funkce služby tři cloudu vyžadovat zvláštní konfigurace před exportem balíčku služby:

* Pokud chcete, aby toodeploy Cloudová služba, která používá Secure Sockets Layer (SSL) pro šifrování dat [konfiguraci aplikace](cloud-services-configure-ssl-certificate-portal.md#modify) pro protokol SSL.
* Pokud chcete, aby tooconfigure instance toorole připojení vzdálené plochy, [konfiguraci rolí hello](cloud-services-role-enable-remote-desktop-new-portal.md) pro vzdálenou plochu.
* Pokud chcete tooconfigure podrobné monitorování pro cloudové služby, povolte pro hello Cloudová služba Azure Diagnostics. *Minimální monitorování* (hello výchozí sledování úrovně) použije čítače výkonu získané z hello hostitelských operačních systémech pro instance rolí (virtuální počítače). *Podrobné monitorování* shromažďuje další metriky na základě dat o výkonu v rámci hello role instance tooenable blíže analyzovat problémy, ke kterým došlo během zpracování aplikace. v tématu tooenable Azure Diagnostics, toofind jak [povolení diagnostiky v Azure](cloud-services-dotnet-diagnostics.md).

toocreate je Cloudová služba se nasazení webové role nebo rolí pracovního procesu, je nutné [vytvořit balíček služby hello](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Než začnete
* Pokud jste nenainstalovali hello Azure SDK, klikněte na tlačítko **instalovat sadu SDK Azure** tooopen hello [Azure stáhne stránky](https://azure.microsoft.com/downloads/)a potom stáhněte hello SDK pro jazyk hello, ve kterém chcete toodevelop vašeho kódu. (Budete mít toodo možnost to později.)
* Pokud všechny instance role vyžadovat certifikát, vytvořte hello certifikáty. Cloudové služby vyžadují soubor .pfx s privátním klíčem. Jak vytvořit a nasadit hello cloudové služby můžete nahrát tooAzure certifikáty hello.

## <a name="create-and-deploy"></a>Vytvoření a nasazení
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nový > výpočetní**a potom přejděte dolů na klikněte na tlačítko tooand **cloudové služby**.

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. V nové hello **Cloudová služba** okno, zadejte hodnotu pro hello **název DNS**.
4. Vytvořte novou **skupiny prostředků** nebo vyberte nějaký existující.
5. Vyberte **Umístění**.
6. Klikněte na tlačítko **balíček**. Otevře se hello **nahrání balíčku** okno. Vyplňte hello požadované pole. Pokud některé z vaší rolí obsahuje jednu instanci, ujistěte se, **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** je vybrána.
7. Ujistěte se, že **spuštění nasazení** je vybrána.
8. Klikněte na tlačítko **OK** kterého bude zavřena hello **nahrání balíčku** okno.
9. Pokud nemáte žádné certifikáty tooadd, klikněte na tlačítko **vytvořit**.

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Nahrání certifikátu
Pokud byl balíček pro nasazení [nakonfigurované toouse certifikáty](cloud-services-configure-ssl-certificate-portal.md#modify), teď můžete nahrát certifikát hello.

1. Vyberte **certifikáty**a na hello **přidat certifikáty** okně, vyberte soubor .pfx certifikátu SSL hello a pak zadejte hello **heslo** hello certifikátu
2. Klikněte na tlačítko **připojit certifikát**a potom klikněte na **OK** na hello **přidat certifikáty** okno.
3. Klikněte na tlačítko **vytvořit** na hello **Cloudová služba** okno. Při nasazení hello dosáhl hello **připraven** stavu, abyste mohli pokračovat toohello další kroky.

    ![Publikování cloudové služby](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Zkontrolujte vaše nasazení bylo úspěšně dokončeno
1. Klikněte na tlačítko instance hello cloudové služby.

    Hello stavu by měl zobrazit, zda je služba hello **systémem**.
2. V části **Essentials**, klikněte na tlačítko hello **adresa URL webu** tooopen vaše cloudové služby ve webovém prohlížeči.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* [Správa služby cloud](cloud-services-how-to-manage-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).
