---
title: "aaaHow toocreate a nasazení cloudové služby | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení cloudové služby v Azure pomocí metody rychlého vytvoření hello."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 09f889f81ccee6e8a7657116183aa4100410214c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a>Jak tooCreate a nasazení cloudové služby
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-create-deploy.md)
> 
> 

Hello klasický portál Azure nabízí dva způsoby, abyste toocreate a nasazení cloudové služby: **rychle vytvořit** a **vytvořit vlastní**.

Toto téma vysvětluje, jak toouse hello toocreate metoda rychle vytvořit novou cloudovou službu a pak použijte **nahrát** tooupload a nasadit balíček cloudové služby v Azure. Pokud použijete tuto metodu, díky hello portál Azure classic k dispozici užitečné odkazy pro dokončení všechny požadavky rovnou. Pokud jste připravené toodeploy vaše cloudové služby při vytváření, můžete provést jak na hello současně pomocí **vytvořit vlastní**.

> [!NOTE]
> Pokud máte v plánu toopublish cloudové služby z Visual Studio Team Services (služby VSTS), použijte **rychle vytvořit**a poté nastavit publikování služby VSTS z **rychlý Start** nebo hello řídicího panelu.
> 
> 

## <a name="concepts"></a>Koncepty
Tři součásti jsou potřeba v pořadí toodeploy aplikace jako cloudová služba v Azure:

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

* Pokud chcete, aby toodeploy Cloudová služba, která používá Secure Sockets Layer (SSL) pro šifrování dat [konfiguraci aplikace](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) pro protokol SSL.
* Pokud chcete, aby tooconfigure instance toorole připojení vzdálené plochy, [konfiguraci rolí hello](cloud-services-role-enable-remote-desktop.md) pro vzdálenou plochu.
* Pokud chcete tooconfigure podrobné monitorování pro cloudové služby, povolte pro hello Cloudová služba Azure Diagnostics. *Minimální monitorování* (hello výchozí sledování úrovně) použije čítače výkonu získané z hello hostitelských operačních systémech pro instance rolí (virtuální počítače). "Podrobné monitorování * shromažďuje další metriky na základě dat o výkonu v rámci hello role instance tooenable blíže analyzovat problémy, ke kterým došlo během zpracování aplikace. v tématu tooenable Azure Diagnostics, toofind jak [povolení diagnostiky v Azure](cloud-services-dotnet-diagnostics.md).

toocreate je Cloudová služba se nasazení webové role nebo rolí pracovního procesu, je nutné [vytvořit balíček služby hello](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Než začnete
* Pokud jste nenainstalovali hello Azure SDK, klikněte na tlačítko **instalovat sadu SDK Azure** tooopen hello [Azure stáhne stránky](https://azure.microsoft.com/downloads/)a potom stáhněte hello SDK pro jazyk hello, ve kterém chcete toodevelop vašeho kódu. (Budete mít toodo možnost to později.)
* Pokud všechny instance role vyžadovat certifikát, vytvořte hello certifikáty. Cloudové služby vyžadují soubor .pfx s privátním klíčem. Můžete [nahrát hello certifikáty tooAzure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) jak vytvářet a nasazovat hello cloudové služby.
* Pokud máte v plánu toodeploy hello cloudové služby tooan skupinu vztahů, vytvořte skupinu vztahů hello. Můžete použít toodeploy skupiny vztahů cloudové služby a další služby Azure toohello stejné umístění, v oblasti. Můžete vytvořit skupinu vztahů hello hello **sítě** oblasti hello portál Azure classic, na hello **skupiny vztahů** stránky.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Postupy: vytvoření cloudové služby pomocí metody rychlého vytvoření
1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **nový**>**výpočetní**>**Cloudová služba** > **Rychle vytvořit**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. V **URL**, zadejte toouse název subdomény v hello veřejnou adresu URL pro přístup ke cloudové služby v produkčním prostředí. Formát adresy URL Hello pro nasazení v produkčním prostředí je: http://*myURL*. cloudapp.net.
3. V **oblast nebo skupinu vztahů**, vyberte hello zeměpisná oblast nebo skupinu vztahů toodeploy hello cloudovou službu. Vyberte skupinu vztahů, pokud chcete toodeploy vaše cloudové služby toohello stejné umístění jako jinými službami Azure v rámci oblasti.
4. Klikněte na **Vytvořit cloudovou službu**.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    Můžete sledovat stav hello hello procesu v oblasti zpráv hello v hello dolní části okna hello.
   
    Hello **cloudové služby** oblasti otevře s hello nová Cloudová služba zobrazí. Při změně stavu hello tooCreated, vytvoření cloudové služby byla úspěšně dokončena.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Postupy: nahrát na server certifikát pro cloudové služby
1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **certifikáty**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Klikněte na možnost **nahrání certifikátu** nebo **nahrát**.
3. V **soubor**, použijte **Procházet** tooselect hello certifikát (soubor .pfx).
4. V **heslo**, zadejte hello soukromý klíč pro certifikát hello.
5. Klikněte na tlačítko **OK** (zaškrtnutí).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Můžete sledovat průběh hello hello nahrávání v oblasti zpráv hello vidíte níže. Po dokončení nahrávání hello je přidat certifikát hello toohello tabulky. V oblasti hello zprávy klikněte na tlačítko OK tooclose uvítací zprávu.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Postupy: nasazení cloudové služby
1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **řídicí panel**.
2. Klikněte na možnost **nahrát nový produkční nasazení** nebo **nahrát**.
3. V **označení nasazení**, zadejte název nové nasazení hello – například MyCloudServicev4.
4. V **balíček**, použijte **Procházet** tooselect hello služby balíčku souboru (.cspkg) toouse.
5. V **konfigurace**, použijte **Procházet** tooselect hello služba konfigurovat toouse souboru (.cscfg).
6. Pokud hello Cloudová služba bude obsahovat všechny role s jedinou instancí, vyberte hello **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** tooproceed nasazení hello tooenable zaškrtávací políčko.
   
    Azure můžete pouze zaručit 99,95 % přístup toohello cloudové služby během údržby a Služba aktualizací Pokud má aspoň dvě instance každé role. V případě potřeby můžete přidat další role instance na hello **škálování** stránky poté, co nasadíte hello cloudové služby. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
7. Klikněte na tlačítko **OK** nasazení služby cloud hello toobegin (zaškrtnutí).
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Můžete sledovat stav hello hello nasazení v oblasti zpráv hello. Klikněte na tlačítko OK toohide uvítací zprávu.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Zkontrolujte vaše nasazení bylo úspěšně dokončeno
1. Klikněte na tlačítko **řídicí panel**.
   
    Hello stavu by měl zobrazit, zda je služba hello **systémem**.
2. V části **rychlého přehledu**, klikněte na tlačítko tooopen adresu URL webu hello cloudové služby ve webovém prohlížeči.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Správa služby cloud](cloud-services-how-to-manage.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).

