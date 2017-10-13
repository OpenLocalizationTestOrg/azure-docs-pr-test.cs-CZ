---
title: "Použít Image klientů Windows ve službě Azure | Microsoft Docs"
description: "Jak používat výhody předplatné sady Visual Studio k nasazení systému Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování"
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
ms.openlocfilehash: 207a6562965b4913416bd4dbf3eb132b42938dc9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Pomocí klienta systému Windows v Azure pro scénáře vývoje/testování
Můžete použít systém Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování za předpokladu, že máte příslušné předplatné sady Visual Studio (dříve MSDN). Tento článek popisuje požadavky na podmínky pro spuštěné klienta systému Windows Azure a použít Azure Galerie obrázků.

## <a name="subscription-eligibility"></a>Předplatné podmínky
Aktivní sadě Visual Studio Odběratelé (osoby, které jste získali licenci sady Visual Studio předplatného) můžete použít klienta systému Windows pro účely vývoje a testování. Klient systému Windows lze použít na hardware a Azure virtuální počítače běžící na jakýkoli typ předplatného Azure. Klienta Windows nemusí nasazené nebo použít v Azure pro použití v provozním prostředí normální nebo použít lidmi, kteří nejsou aktivní odběratele Visual Studio.

Pro usnadnění vaší práce jsme provedli některé Image Windows 10 dostupné z Galerie Azure v rámci [nabízí vhodné pro vývoj/testování](#eligible-offers). Visual Studio odběratele v rámci libovolného typu nabídky můžete také [adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a pak [nahrát do Azure](upload-generalized-managed.md). Použití zůstane omezená na vývoj nebo testování odběratelům active Visual Studio.

## <a name="eligible-offers"></a>Vhodné nabídky
V následující tabulce jsou nabídku ID, které jsou způsobilé k nasazení Windows 10 prostřednictvím Azure Gallery. Bitové kopie systému Windows 10 viditelné pouze pro následující nabídky. Visual Studio Odběratelé, kteří potřebují ke spuštění klienta systému Windows v různých nabídka typu vyžadují, abyste [adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a [pak nahrajte do Azure](upload-generalized-managed.md).

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
Pokud si nejste jisti vaše ID nabídky, můžete ho získat prostřednictvím portálu Azure v jednom z těchto způsobů:  

- V okně 'Odběry':

  ![Informace ID z portálu Azure](./media/client-images/offer-id-azure-portal.png) 

- Nebo klikněte na tlačítko **fakturace** a pak klikněte na ID vašeho předplatného. ID nabídky se zobrazí v okně fakturace.

Můžete také zobrazit ID nabídky z ['odběry, karta](http://account.windowsazure.com/Subscriptions) portálu účtů Azure:

![Informace ID z portálu účtu služby Azure](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Další kroky
Teď můžete nasadit virtuální počítače pomocí [prostředí PowerShell](quick-create-powershell.md), [šablony Resource Manageru](ps-template.md), nebo [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

