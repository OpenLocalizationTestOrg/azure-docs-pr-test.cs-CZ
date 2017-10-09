---
title: "aaaEnable připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services | Microsoft Docs"
description: "Jak tooconfigure vaše azure cloudové služby aplikace tooallow připojení ke vzdálené ploše"
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: 55d7043df571c2e88b04aa9ef01dc8ae1d6784f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Portál Azure Classic](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

Vzdálená plocha umožňuje tooaccess hello ploše role, která běží v Azure. Můžete použít tootroubleshoot připojení vzdálené plochy a diagnostikovat problémy s vaší aplikací, když je spuštěná.

Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje začleněním modulů hello vzdálené plochy v definice služby nebo můžete zvolit tooenable vzdálené plochy prostřednictvím hello rozšíření vzdálené plochy. Hello žádoucí je toouse hello vzdálené plochy rozšíření jako Vzdálená plocha můžete povolit i po nasazení aplikace hello bez nutnosti tooredeploy vaší aplikace.

## <a name="configure-remote-desktop-from-hello-azure-portal"></a>Konfigurace vzdálené plochy z hello portálu Azure
Hello portál Azure používá přístup vzdálené plochy rozšíření hello, takže Vzdálená plocha můžete povolit i po nasazení aplikace hello. Hello **vzdálené plochy** okno pro cloudové služby vám umožní tooenable vzdálené plochy, změna hello místní účet správce používá tooconnect toohello virtuální počítače, hello certifikátu používané v ověřování a nastavte hello Datum vypršení platnosti.

1. Klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **vzdálené plochy**.

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Zvolte, zda jste tooenable vzdálené plochy pro jednotlivé role nebo pro všechny role, a potom změnit hodnotu hello hello přepínači příliš**povoleno**.

3. Vyplňte pole hello požadované pro uživatelské jméno, heslo, vypršení platnosti a certifikát.

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí). tooprevent restartu hello certifikátů používaných tooencrypt hello heslo musí být nainstalován na hello role. tooprevent restartování, [nahrát certifikát pro cloudové služby hello](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte toothis dialogu.
   >
   >
3. V **role**, vyberte roli hello tooupdate nebo vyberte **všechny** u všech rolí.

4. Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **Uložit**. To bude trvat chvíli instance role jsou připravené tooreceive připojení.

## <a name="remote-into-role-instances"></a>Vzdálené do instance rolí
Po povolení vzdálené plochy na hello role můžete zahájit připojení přímo z portálu Azure hello:

1. Klikněte na tlačítko **instance** tooopen hello **instance** okno.
2. Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.
3. Klikněte na tlačítko **Connect** souboru toodownload protokolu RDP pro instanci role hello.

    ![Cloudové služby Vzdálená plocha](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Klikněte na tlačítko **otevřete** a potom **připojit** toostart hello připojení vzdálené plochy.

>[!NOTE]
> Pokud cloudové služby nachází za skupinu NSG, může být nutné toocreate pravidla, která umožňují přenosy na portech **3389** a **20000**.  Vzdálená plocha používá port **3389**.  Cloudové služby instance jsou vyrovnáváním, zatížení, takže nemůže přímo řídit které tooconnect instance k.  Hello *RemoteForwarder* a *RemoteAccess* agenty spravovat provoz protokolu RDP a povolit hello klienta toosend soubor cookie s RDP a zadejte jednotlivé instance tooconnect k.  Hello *RemoteForwarder* a *RemoteAccess* agentů vyžadují tento port **20000*** otevřít, což může být zablokován, pokud máte skupinu NSG.

## <a name="additional-resources"></a>Další zdroje

[Jak tooConfigure cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)
