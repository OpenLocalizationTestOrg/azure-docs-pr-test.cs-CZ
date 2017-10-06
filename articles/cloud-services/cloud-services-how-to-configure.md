---
title: "aaaHow tooconfigure cloudové služby (portálu classic) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure cloudových služeb v Azure. Další konfigurace tooupdate hello cloudové služby a konfigurace vzdáleného přístupu toorole instance."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 1ea2320f97f667153f7984e4d61d373a6344cf6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a>Jak tooConfigure cloudových služeb
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-configure-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-configure.md)
> 
> 

Hello nejčastěji používaná nastavení pro cloudové služby můžete nakonfigurovat v hello portál Azure classic. Nebo, pokud chcete tooupdate přímo, konfigurační soubory stáhnout tooupdate soubor konfigurace služby a pak nahrajte hello aktualizovat soubor a aktualizace hello Cloudová služba se změny konfigurace hello. V obou případech aktualizace konfigurace hello jsou instalováni tooall instancí rolí.

Hello portál Azure classic můžete taky příliš[povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)

Azure můžete pouze zajistit dostupnost služby 99,95 % během hello aktualizace konfigurace Pokud máte aspoň dvě instance role pro každou roli. Umožňující jeden virtuální počítač tooprocess klientských požadavků v průběhu aktualizace hello jiné. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Změnit cloudové služby
1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **konfigurace**.
   
    ![Stránka Konfigurace](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    Na hello **konfigurace** stránky, můžete nakonfigurovat nastavení aktualizace role, sledování a zvolte hello hostovaného operačního systému a třídu u instancí role. 
2. V **monitorování**, hello sady monitorování úrovni tooVerbose nebo minimální a konfigurovat hello diagnostiky připojovací řetězce, které jsou požadovány pro podrobné monitorování.
3. Pro služby role (seskupené podle role) můžete aktualizovat hello následující nastavení:
   
    * **Nastavení** upravte hodnoty hello nastavení různé konfigurace, které jsou určené v hello *ConfigurationSettings* elementy souboru definice hello služby (.csdef).

    * **Certifikáty**  
        Změnit hello kryptografický otisk certifikátu, který se používá v šifrování SSL pro roli. toochange certifikát, musíte nejprve nahrát nový certifikát hello (na hello **certifikáty** stránky). Potom aktualizujte kryptografický otisk hello v řetězci certifikátu hello zobrazí v nastavení role hello.
4. V **operačního systému**, můžete změnit operační systém řady hello nebo verze pro instance rolí, nebo zvolte **automatické** tooenable automatické aktualizace programu hello aktuální verzi operačního systému. nastavení operačního systému Hello použijí tooweb rolí a rolí pracovního procesu, ale neovlivní virtuálních počítačů.
   
    Během nasazení hello nejnovější verzi operačního systému je nainstalován na všech instancích role a ve výchozím nastavení se automaticky aktualizují hello operační systémy. 
   
    Pokud budete potřebovat pro vaše toorun cloudové služby na jinou verzi operačního systému z důvodu požadavky na kompatibilitu ve vašem kódu, můžete operačního systému rodiny a verze. Když zvolíte konkrétní verze operačního systému, aktualizací automatické operačního systému pro hello cloudové služby jsou pozastavené. Budete potřebovat tooensure hello operační systémy přijímat aktualizace.
   
    Pokud vyřešit všechny problémy s kompatibilitou, které aplikace mají s hello nejnovější verzi operačního systému, můžete povolit aktualizace automatické operačního systému podle nastavení verze operačního systému hello příliš**automatické**. 
   
    ![Nastavení operačního systému](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. toosave nastavení konfigurace a vložit je toohello instance rolí, klikněte na tlačítko **Uložit**. (Klikněte na tlačítko **zahodit** toocancel hello změny.) **Uložit** a **zahodit** po provedení změny nastavení se přidají toohello příkazového řádku.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualizace konfiguračního souboru cloudové služby
1. Stáhněte si cloudové služby konfiguračního souboru (.cscfg) s aktuální konfigurací hello. Na hello **konfigurace** stránky pro hello cloudovou službu, klikněte na tlačítko **Stáhnout**. Pak klikněte na tlačítko **Uložit**, nebo klikněte na tlačítko **uložit jako** toosave hello souboru.
2. Po aktualizaci konfigurační soubor služby hello, odesílání a aktualizace konfigurace hello:
   
   1. Na hello **konfigurace** klikněte na tlačítko **nahrát**.
      
       ![Nahrát konfigurace](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. V **konfigurační soubor**, použijte **Procházet** tooselect hello aktualizovat souboru .cscfg.
   3. Pokud cloudové služby obsahuje všechny role, které mají jenom jednu instanci, vyberte hello **použít konfiguraci i v případě, že jeden nebo více rolí obsahuje jednu instanci** políčko tooenable hello aktualizace konfigurace pro role tooproceed hello.
      
       Pokud definujete aspoň dvě instance každé role, Azure nemůže zaručit alespoň 99,95 % dostupnosti cloudové služby během aktualizace služby konfigurace. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
   4. Klikněte na tlačítko **OK** (zaškrtnutí). 

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Správa služby cloud](cloud-services-how-to-manage.md).
* [Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure](cloud-services-role-enable-remote-desktop.md)
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).

