---
title: "Brána dat místní aaaInstall | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace brány místní data."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/22/2017
ms.author: owend
ms.openlocfilehash: e2878bf765c82910d452ae2cdd9264a343ec1990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Nainstalujte a nakonfigurujte bránu místní data
Místní brána dat je potřeba při jeden nebo více serverů Azure Analysis Services v hello stejné oblasti připojení zdroje dat tooon místní. toolearn Další informace o bráně hello, najdete v části [místní brána dat](analysis-services-gateway.md).

## <a name="prerequisites"></a>Požadavky
**Minimální požadavky:**

* Rozhraní .NET 4.5 framework
* 64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)

**Doporučujeme:**

* 8 jader procesoru
* 8 GB paměti
* 64bitová verze systému Windows 2012 R2 (nebo novější)

**Důležité informace:**

* Během instalace, při registraci brány na Azure je vybrat hello výchozí oblast pro vaše předplatné. Můžete použít v jiné oblasti. Pokud máte servery ve více než jedné oblasti, je nutné nainstalovat bránu pro každou oblast. 
* Brána Hello nemůže být nainstalována na řadiči domény.
* V jednom počítači lze nainstalovat pouze jedna brána.
* Hello bránu nainstalujte na počítač, který zůstává na a nepřekračuje toosleep.
* Neinstalujte hello brány v síti bezdrátově připojených tooyour počítače. Výkon může být snížena.


## <a name="download"></a>Stahování
 [Stáhnout hello brány](https://aka.ms/azureasgateway)

## <a name="install"></a>Instalace

1. Spusťte instalační program.

2. Vyberte umístění, přijměte podmínky hello a pak klikněte na tlačítko **nainstalovat**.

   ![Nainstalujte umístění a licenční podmínky](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Vyberte **místní brána dat (doporučeno)**. Azure Analysis Services nepodporuje osobní režimu.

   ![Vyberte typ brány](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. Zadejte účtu toosign tooAzure. Hello účet musí být v vašeho klienta Azure Active Directory. Tento účet slouží pro hello Správce brány. 

   ![Zadejte účtu toosign v tooAzure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Pokud se přihlásíte pomocí účtu domény, bude namapované tooyour účet organizace v Azure AD. Váš účet organizace se použije jako správce brány hello hello.

## <a name="register"></a>Registrace
V pořadí toocreate brány prostředků v Azure je nutné zaregistrovat hello místní instance, které jste nainstalovali s hello cloudové službě brány. 

1.  Vyberte **registrace nové brány na tomto počítači**.

    ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Zadejte název a obnovení klíče pro bránu. Ve výchozím nastavení brána hello používá výchozí oblasti vašeho předplatného. Pokud potřebujete tooselect v jiné oblasti, vyberte **změnu oblast**.

   ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Vytvořte prostředek Azure brány
Po instalaci a zaregistrovat bránu, musíte toocreate brány prostředků ve vašem předplatném Azure. Přihlášení tooAzure s hello stejný účet, které jste použili při registraci brány hello.

1. Na portálu Azure, klikněte na tlačítko **vytvoření nové služby** > **Enterprise integrace** > **místní brána dat** > **vytvořit**.

   ![Vytvořte prostředek brány](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. V **vytvořit připojení bránu**, zadejte tato nastavení:

    * **Název**: Zadejte název pro prostředek brány. 

    * **Předplatné**: Vyberte hello tooassociate předplatného Azure s vaší brány prostředků. 
    Tento odběr musí být hello jsou vaše servery v rámci stejného předplatného.
   
      výchozí předplatné Hello je založena na hello účet Azure, který jste použili toosign v.

    * **Skupina prostředků**: Vytvořte skupinu prostředků, nebo vyberte existující.

    * **Umístění**: zaregistrovat bránu v oblasti vyberte hello.

    * **Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte hello brána registrovaná. 

    Když jste hotovi, klikněte na tlačítko **vytvořit**.

## <a name="connect-servers"></a>Připojte prostředek brány toohello servery

1. V přehledu vaší služby Azure Analysis Services serveru, klikněte na tlačítko **místní brána dat**.

   ![Připojit server toogateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. V **vyberte místní brána dat tooconnect**, vyberte prostředek vaší brány a pak klikněte na tlačítko **připojit vybrané brány**.

   ![Připojte server toogateway prostředek](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Pokud vaše brána není uvedené v seznamu hello, váš server není pravděpodobné ve stejné oblasti jako hello oblasti, které jste zadali při registraci brány hello hello. 

A je to. Pokud třeba tooopen porty nebo provést žádné řešení potíží s, nebude se toocheck se [místní brána dat](analysis-services-gateway.md).

## <a name="next-steps"></a>Další kroky
* [Správa služby Analysis Services](analysis-services-manage.md)   
* [Získání dat z Azure Analysis Services](analysis-services-connect.md)
