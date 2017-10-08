---
title: "aaaUse bitových kopií klienta systému Windows v Azure | Microsoft Docs"
description: "Výhody způsob, jak toouse předplatné sady Visual Studio toodeploy Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 4ac7c3d9872673030f4ea0f0ab38625dd9d9c1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Pomocí klienta systému Windows v Azure pro scénáře vývoje/testování
Můžete použít systém Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování za předpokladu, že máte příslušné předplatné sady Visual Studio (dříve MSDN). Tento článek popisuje hello způsobilosti požadavky pro spuštění klienta systému Windows Azure a použít Dobrý den Image Azure Gallery.

## <a name="subscription-eligibility"></a>Předplatné podmínky
Aktivní sadě Visual Studio Odběratelé (osoby, které jste získali licenci sady Visual Studio předplatného) můžete použít klienta systému Windows pro účely vývoje a testování. Klient systému Windows lze použít na hardware a Azure virtuální počítače běžící na jakýkoli typ předplatného Azure. Klient Windows nemusí být nasazené tooor použít v Azure pro použití v provozním prostředí normální, nebo použít lidmi, kteří nejsou aktivní odběratele Visual Studio.

Pro usnadnění vaší práce jsme provedli některé Image Windows 10 dostupné z Galerie Azure hello v rámci [nabízí vhodné pro vývoj/testování](#eligible-offers). Visual Studio odběratele v rámci libovolného typu nabídky můžete také [adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a pak [nahrát tooAzure](upload-generalized-managed.md). použití Hello zůstane omezené toodev a testovací odběratelům active Visual Studio.

## <a name="eligible-offers"></a>Vhodné nabídky
Následující tabulka Podrobnosti hello Hello nabízejí ID, které jsou způsobilé toodeploy Windows 10 prostřednictvím hello Azure Gallery. Image Hello Windows 10 jsou pouze viditelné toohello následující nabídky. Visual Studio Odběratelé, kteří potřebují toorun klienta systému Windows v různých nabídka typu vyžadovat příliš[adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a [nahrajte tooAzure](upload-generalized-managed.md).

| Název nabídky | Číslo nabídky | Bitové kopie k dispozici klienta |
|:--- |:---:|:---:|
| [Průběžné platby vývoje/testování](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN) odběratele](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional odběratele](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional odběratele](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium with MSDN (benefit)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise odběratele](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise (BizSpark) odběratele](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise pro vývoj/testování](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Zkontrolujte předplatné Azure
Pokud si nejste jisti vaše ID nabídky, můžete ho získat prostřednictvím hello portálu Azure v jednom z těchto způsobů:  

- V okně "Odběry" hello:

  ![Informace ID z hello portálu Azure](./media/client-images/offer-id-azure-portal.png) 

- Nebo klikněte na tlačítko **fakturace** a pak klikněte na ID vašeho předplatného. ID nabídky Hello se zobrazí v okně fakturace hello.

Můžete také zobrazit ID nabídky hello z hello ['odběry, karta](http://account.windowsazure.com/Subscriptions) portálu účtů Azure hello:

![Podrobnosti ID nabídky z portálu účtu Azure hello](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Další kroky
Teď můžete nasadit virtuální počítače pomocí [prostředí PowerShell](quick-create-powershell.md), [šablony Resource Manageru](ps-template.md), nebo [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

