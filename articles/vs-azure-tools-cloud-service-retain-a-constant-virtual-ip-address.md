---
title: "aaaHow tooretain konstantní virtuální IP adresy pro cloudové služby Azure | Microsoft Docs"
description: "Zjistěte, jak nezmění tooensure, který hello virtuální IP adresy (VIP) cloudové služby Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 9e27121797ffb61517b8d2c2661ec44ff7298968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Zachovat konstantní virtuální IP adresy pro cloudové služby Azure
Pokud aktualizujete Cloudová služba, která je hostovaná v Azure, bude pravděpodobně třeba tooensure, který hello virtuální adresy IP (VIP) služby hello nemění. Většina služeb správu domény používá hello systému DNS (Domain Name) pro registraci názvů domén. DNS funguje pouze v případě, že zůstanou hello VIP hello stejné. Můžete použít hello **Průvodci publikováním** v tooensure nástroje Azure, který hello VIP cloudové služby nezmění. když ho aktualizujete. Další informace o tom, najdete v části Správa domén DNS toouse pro cloudové služby, [konfigurace vlastního názvu domény pro cloudové služby Azure](cloud-services/cloud-services-custom-domain-name.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Publikování Cloudová služba beze změny jeho VIP
Hello VIP Cloudová služba je přidělen při prvním nasazení je tooAzure v konkrétním prostředí, jako je například hello produkčního prostředí. proces aktualizovat Hello VIP změny jenom v případě explicitně odstranit hello nasazení nebo nasazení hello je implicitně odstraní hello nasazení. tooretain hello VIP, nesmí odstraňte nasazení, a ujistěte se, že Visual Studio automaticky neodstraní vaše nasazení. 

Můžete zadat nastavení nasazení v hello **Průvodci publikováním**, který podporuje několik možností nasazení. Můžete zadat nové nasazení nebo nasazení aktualizace, která může být přírůstkové nebo souběžných. Oba typy nasazení aktualizace zachovat hello VIP. Definice těchto různých typů nasazení naleznete v tématu [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md). Kromě toho můžete určit, zda text hello předchozích nasazení cloudové služby je odstraněna, když dojde k chybě. Pokud tato možnost není správně nastavený, může se neočekávaně měnit hello VIP.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Aktualizace cloudové služby, aniž byste museli měnit jeho VIP
1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio. 

2. V **Průzkumníku**, klikněte pravým tlačítkem na projekt hello. V místní nabídce hello, vyberte **publikovat**.

    ![Publikování nabídky](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. V hello **publikování aplikaci Azure** dialogové okno, vyberte hello předplatného Azure toowhich chcete toodeploy. Přihlášení, pokud potřebné a vyberte **Další**.

    ![Publikování Azure aplikace přihlašovací stránku](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. Na hello **společná nastavení** ověřte, že název hello hello cloudové služby toowhich nasazujete, hello **prostředí**, hello **konfigurace sestavení**a hello **Konfigurace služby** správně.

    ![Publikování karta společné nastavení aplikace Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. Na hello **Upřesnit nastavení** ověřte, že hello **označení nasazení** a hello **účet úložiště** jsou správné. Ověřte, že hello **odstranění nasazení na selhání** políčko není zaškrtnuté a ověřte, že hello **nasazení aktualizace** je zaškrtnuté políčko. Pomocí zrušením hello **odstranění nasazení na selhání** zaškrtávací políčko, zajistíte, že vaše virtuální IP adresy není ke ztrátě, pokud dojde k chybě během nasazení. Výběrem hello **aktualizaci nasazení** zaškrtávací políčko můžete zajistěte, aby vaše nasazení není odstraněn a vaše virtuální IP adresy není ztraceny po publikování aplikace. 

    ![Publikování karta Upřesnit nastavení aplikace Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. toofurther určit, jakým způsobem chcete hello role toobe aktualizovat, vyberte **nastavení** další příliš**nasazení aktualizace**. Vyberte buď **přírůstkové aktualizace** nebo **Souběžná aktualizace**a vyberte **OK**. Zvolte **přírůstkové aktualizace** tooupdate každou instanci vaší aplikace, jedna po druhé, který hello aplikace je vždy k dispozici. Zvolte **Souběžná aktualizace** tooupdate všechny instance aplikace na hello stejnou dobu. Souběžné aktualizace je rychlejší, ale vaše služba nemusí být k dispozici během procesu aktualizace hello. Po dokončení vyberte **Další**.

    ![Publikovat stránku nastavení nasazení aplikace Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. V hello **publikování aplikaci Azure** dialogové okno, vyberte **Další** dokud hello **Souhrn** zobrazí se stránka. Ověřte nastavení a potom vyberte **publikovat**.
   
    ![Publikování na stránce Souhrn aplikace Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Další kroky
- [Pomocí hello Visual Studio Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md)

