---
title: "aaaManage účet Azure Automation | Microsoft Docs"
description: "Tento článek popisuje, jak toomanage hello konfigurace účtu Automation, například chybné konfigurace, odstranění a obnovení certifikátu."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 75e41f601a138d4e8853aa79dcbab6696a5e9fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-automation-account"></a>Správa účtu Azure Automation
V určitém okamžiku před vypršením platnosti účtu Automation, musíte certifikát toorenew hello. Pokud se domníváte, že došlo k ohrožení zabezpečení tohoto hello účet Spustit jako, můžete odstranit a znovu vytvořit. Tato část pojednává o tom, jak tooperform těchto operací.

## <a name="self-signed-certificate-renewal"></a>Obnovení certifikátu podepsaného svým držitelem
certifikát podepsaný svým držitelem Hello, kterou jste vytvořili pro hello účet Spustit jako vyprší platnost jeden rok z hello data vytvoření. Před vypršením platnosti ho můžete kdykoli obnovit. Při obnovování, je aktuální platný certifikát hello udržených tooensure, že nejsou žádné sady runbook, které jsou zařazeny do fronty až nebo aktivně spuštěná, a který ověření pomocí hello účet Spustit jako, negativní vliv. certifikát Hello zůstává platná do data vypršení platnosti.

> [!NOTE]
> Pokud jste nakonfigurovali vaší spustit v Automation jako účet toouse certifikát vydaný certifikační autoritou rozlehlé sítě a chcete použít tuto možnost, hello podnikový certifikát budou nahrazeny certifikát podepsaný svým držitelem.

toorenew hello certifikátů, hello následující:

1. V hello portálu Azure otevřete účet Automation hello.

2. Na hello **účet Automation** okno, v hello **účet vlastnosti** podokně v části **nastavení účtu**, vyberte **účty spustit jako**.

    ![Podokno vlastností účtu Automation](media/automation-manage-account/automation-account-properties-pane.png)
3. Na hello **účty spustit jako** vlastnosti okně vyberte buď hello účet Spustit jako nebo hello Classic účet Spustit jako, které chcete toorenew hello certifikát pro.

4. Na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **obnovit certifikát**.

    ![Obnovení certifikátu pro účet Spustit jako](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. Při obnovení certifikátu hello se můžete sledovat průběh hello pod **oznámení** nabídce hello.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Odstranění účet Spustit jako nebo Spustit jako pro Classic
Tato část popisuje, jak toodelete a znovu vytvořit účet Spustit jako nebo Classic spustit jako. Když provedete tuto akci, se uchovávají hello účet Automation. Po odstranění účtu spustit jako nebo Classic spustit jako, znovu ji vytvořte hello portálu Azure.

1. V hello portálu Azure otevřete účet Automation hello.

2. Na hello **účet Automation** okno, v hello účet vlastnosti podokně, vyberte **účty spustit jako**.

3. Na hello **účty spustit jako** okno Vlastnosti, vyberte buď hello účet Spustit jako nebo Classic spustit jako účet, který má toodelete. Potom na hello **vlastnosti** okně hello vybraný účet, klikněte na tlačítko **odstranit**.

 ![Odstranění účtu Spustit jako](media/automation-manage-account/automation-account-delete-runas.png)

4. Zatímco se odstraňuje hello účet, můžete sledovat průběh hello v části **oznámení** nabídce hello.

5. Po odstranění účtu hello můžete znovu vytvořit ji na hello **účty spustit jako** okno Vlastnosti výběrem hello vytvořit možnost **Azure účet Spustit jako**.

 ![Znovu vytvořit účet Automation spustit jako hello](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Chybná konfigurace
Některé položky konfigurace potřebné pro toofunction účet Spustit jako nebo Classic spustit jako hello správně může byla odstraněna nebo nesprávně vytvořený během počáteční instalace. položky Hello patří:

* Asset certifikátu
* Asset připojení
* Účet Spustit jako byl odebrán z role Přispěvatel hello
* Instanční objekt nebo aplikace v Azure AD

V hello předchozí a další instance chybné konfigurace, zjistí hello účet Automation hello změní a zobrazuje stav *nekompletní* na hello **účty spustit jako** okně Vlastnosti pro hello účet.

![Nedokončená konfigurace účtu Spustit jako](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Když vyberete účet Spustit jako hello, hello účet **vlastnosti** podokně zobrazí hello následující chybová zpráva:

![Zpráva upozornění o nedokončené konfiguraci účtu Spustit jako](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

Odstranit a znovu vytvořit účet hello můžete rychle vyřešit tyto problémy účet Spustit jako.

## <a name="next-steps"></a>Další kroky
* Další informace o objektech služby najdete v části příliš[objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).
* Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v části příliš[řízení přístupu na základě Role ve službě Azure Automation](automation-role-based-access-control.md).
* Další informace o certifikátech a službám Azure, najdete v části příliš[Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
