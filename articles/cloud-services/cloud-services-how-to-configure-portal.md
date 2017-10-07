---
title: "aaaHow tooconfigure cloudové služby (portál) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure cloudových služeb v Azure. Další konfigurace tooupdate hello cloudové služby a konfigurace vzdáleného přístupu toorole instance. Tyto příklady použití hello portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: 969a08558473e8c79153192942bfda587eb5ada5
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

Hello nejčastěji používaná nastavení pro cloudové služby můžete nakonfigurovat v hello portálu Azure. Nebo, pokud chcete tooupdate přímo, konfigurační soubory stáhnout tooupdate soubor konfigurace služby a pak nahrajte hello aktualizovat soubor a aktualizace hello Cloudová služba se změny konfigurace hello. V obou případech aktualizace konfigurace hello jsou instalováni tooall instancí rolí.

Můžete také spravovat hello instancí role cloudové služby nebo vzdálené plochy do nich.

Azure můžete pouze zajistit dostupnost služby 99,95 % během hello aktualizace konfigurace Pokud máte aspoň dvě instance role pro každou roli. Umožňující jeden virtuální počítač tooprocess klientských požadavků v průběhu aktualizace hello jiné. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Změnit cloudové služby
Po otevření hello [portál Azure](https://portal.azure.com/), přejděte tooyour cloudové služby. Tady můžete spravovat mnoho aspektů.

![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)

Hello **nastavení** nebo **všechna nastavení** odkazy se otevře hello **nastavení** okno, kde můžete změnit hello **vlastnosti**, změňte hello **Konfigurace**, spravovat hello **certifikáty**, instalační program **výstrah pravidla**a spravovat hello **uživatelé** a které mají přístup toothis Cloudová služba.

![Okno nastavení služby Azure cloud](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Správa verzí hostovaného operačního systému

Ve výchozím nastavení Azure pravidelně aktualizuje hostovaného operačního systému toohello nejnovější podporovanou bitové kopie v rámci hello řada operačního systému, který jste zadali v konfiguraci služby (.cscfg), například Windows Server 2016.

Pokud potřebujete tootarget konkrétní verze operačního systému, můžete ho nastavit v hello **konfigurace** okno.

![Nastavit verzi operačního systému](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> Výběr konkrétní verze operačního systému zakáže automatické OS aktualizace a zajišťuje opravy vaší povinností. Ujistěte se, že instance role jsou přijímá aktualizace nebo může odhalí ohrožení zabezpečení vaší aplikace toosecurity.

## <a name="monitoring"></a>Monitorování
Můžete přidat výstrahy tooyour cloudové služby. Klikněte na tlačítko **nastavení** > **pravidla výstrahy** > **přidat upozornění**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Zde můžete nastavit výstrahy. S hello **metrika** rozevíracího pole, můžete nastavit upozornění pro následující typy dat hello.

* Čtení z disku
* Zápis disku
* Sítě v
* Sítě out
* Procento CPU

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfigurace monitorování z metriky dlaždice
Místo použití **nastavení** > **pravidla výstrah**, můžete kliknutím na jednu z hello metriky dlaždice v hello **monitorování** části hello **cloudu Služba** okno.

![Cloudová služba monitorování](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Tady můžete přizpůsobit graf hello použít s hello dlaždice nebo přidání pravidla výstrahy.

## <a name="reboot-reimage-or-remote-desktop"></a>Restartování, obnovení z Image nebo vzdálené plochy
V tuto chvíli nelze konfigurovat vzdálenou plochu pomocí hello **portál Azure**. Však můžete nastavit ji prostřednictvím hello [portál Azure classic](cloud-services-role-enable-remote-desktop.md), [prostředí PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), nebo pomocí [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).

První klikněte na instanci služby hello cloudu.

![Instance cloudové služby](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Okno, které se otevře, můžete z hello můžete iniciovat připojení ke vzdálené ploše, vzdáleně restartovat hello instance, nebo vzdáleně obnovení z Image (začněte s novou bitovou kopii) hello.

![Cloudové služby Instance tlačítka](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Překonfigurujte vaší .cscfg
Může být nutné tooreconfigure cloudové služby prostřednictvím hello [konfigurace služby (cscfg)](cloud-services-model-and-package.md#cscfg) souboru. Nejdřív je potřeba toodownload vaše .cscfg souboru, upravte ho a pak nahrajte ho.

1. Klikněte na hello **nastavení** ikony nebo hello **všechna nastavení** odkaz tooopen až hello **nastavení** okno.

    ![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Klikněte na hello **konfigurace** položky.

    ![Okno Konfigurace](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Klikněte na hello **Stáhnout** tlačítko.

    ![Ke stažení](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Po aktualizaci konfigurační soubor služby hello, odesílání a aktualizace konfigurace hello:

    ![Odeslat](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. Vyberte soubor .cscfg hello a klikněte na tlačítko **OK**.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* [Správa služby cloud](cloud-services-how-to-manage-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).
