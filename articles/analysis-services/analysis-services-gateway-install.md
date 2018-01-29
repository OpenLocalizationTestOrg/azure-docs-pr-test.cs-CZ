---
title: "Nainstalovat bránu dat místní | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat bránu místní data."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 10/30/2017
ms.author: owend
ms.openlocfilehash: 9ba36c4cf932cab2e449b12a307639184feab049
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Nainstalujte a nakonfigurujte bránu místní data
Místní brána dat je potřeba při jeden nebo více serverů ve stejné oblasti Azure Analysis Services připojení ke zdrojům dat v místě. Další informace o bráně najdete v tématu [místní brána dat](analysis-services-gateway.md).

## <a name="prerequisites"></a>Požadavky
**Minimální požadavky:**

* Rozhraní .NET 4.5 framework
* 64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)

**Doporučujeme:**

* 8 jader procesoru
* 8 GB paměti
* 64bitová verze systému Windows 2012 R2 (nebo novější)

**Důležité informace:**

* Během instalace, při registraci brány na Azure je vybrán výchozí oblasti pro vaše předplatné. Můžete použít v jiné oblasti. Pokud máte servery ve více než jedné oblasti, je nutné nainstalovat bránu pro každou oblast. 
* Brána nemůže být nainstalována na řadiči domény.
* V jednom počítači lze nainstalovat pouze jedna brána.
* Bránu nainstalujte na počítač, který zůstává na a není přejít do režimu spánku.
* Neinstalujte bránu v počítači se bezdrátově připojený k síti. Výkon může být snížena.
* Přihlaste se k Azure pomocí účtu ve službě Azure AD pro stejné [klienta](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) jako předplatné registrujete brána. Azure B2B účty (Host) nejsou podporována při instalaci a registraci brány.


## <a name="download"></a>Stahování
 [Stáhněte bránu](https://aka.ms/azureasgateway)

## <a name="install"></a>Instalace

1. Spusťte instalační program.

2. Vyberte umístění, přijměte podmínky a pak klikněte na tlačítko **nainstalovat**.

   ![Nainstalujte umístění a licenční podmínky](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Přihlaste se k Azure. Účet musí být v vašeho klienta Azure Active Directory. Tento účet slouží pro správce brány. Azure B2B účty (Host) nejsou podporována při instalaci a registraci brány.

   ![Přihlášení k Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Pokud se přihlásíte pomocí účtu domény, je namapována na váš účet organizace v Azure AD. Váš účet organizace se používá jako správce brány.

## <a name="register"></a>Registrace
Chcete-li vytvořit bránu prostředků v Azure, je nutné zaregistrovat místní instance, kterou jste nainstalovali s cloudové službě brány. 

1.  Vyberte **registrace nové brány na tomto počítači**.

    ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Zadejte název a obnovení klíče pro bránu. Ve výchozím nastavení brána používá výchozí oblasti vašeho předplatného. Pokud je nutné vybrat jiné oblasti, vyberte **změnu oblast**.

    > [!IMPORTANT]
    > Obnovovací klíč uložte na bezpečném místě. Obnovovací klíč se vyžaduje v daném pořadí k převzetí, migraci nebo obnovení brány. 

   ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Vytvořte prostředek Azure brány
Po instalaci a zaregistrovat bránu, musíte vytvořit prostředek brány ve vašem předplatném Azure. Přihlaste se k Azure pomocí stejného účtu, který jste použili při registraci brány.

1. Na portálu Azure, klikněte na tlačítko **vytvoření nové služby** > **Enterprise integrace** > **místní brána dat** > **vytvořit**.

   ![Vytvořte prostředek brány](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. V **vytvořit připojení bránu**, zadejte tato nastavení:

    * **Název**: Zadejte název pro prostředek brány. 

    * **Předplatné**: Vyberte předplatné Azure, které chcete přidružit k prostředku brány. 
   
      Výchozí předplatné je založena na účet Azure, který jste použili k přihlášení.

    * **Skupina prostředků**: Vytvořte skupinu prostředků, nebo vyberte existující.

    * **Umístění**: Vyberte oblast zaregistrovat bránu v.

    * **Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte bránu zaregistrován. 

    Když jste hotovi, klikněte na tlačítko **vytvořit**.

## <a name="connect-servers"></a>Připojte servery k prostředku brány

1. V přehledu vaší služby Azure Analysis Services serveru, klikněte na tlačítko **místní brána dat**.

   ![Připojení serveru k bráně](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. V **vyberte bránu místní Data připojit**, vyberte prostředek vaší brány a pak klikněte na tlačítko **připojit vybrané brány**.

   ![Připojení serveru k prostředku brány](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Pokud vaše brána v seznamu nezobrazí, je váš server pravděpodobně že není ve stejné oblasti jako oblast jste zadali při registraci brány. 

A je to. Pokud potřebujete otevřít porty nebo provést žádné řešení potíží, nezapomeňte se podívat [místní brána dat](analysis-services-gateway.md).

## <a name="next-steps"></a>Další kroky
* [Správa služby Analysis Services](analysis-services-manage.md)   
* [Získání dat z Azure Analysis Services](analysis-services-connect.md)
