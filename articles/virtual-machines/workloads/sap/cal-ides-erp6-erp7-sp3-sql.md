---
title: "aaaDeploy SAP SP3 EHP7 integrovaného vývojového prostředí pro SAP ERP 6.0 v Azure | Microsoft Docs"
description: "Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 na Azure"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 26d88c7b48a91d35602464c4f89ca7a30502c4b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a>Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 na Azure
Tento článek popisuje, jak toodeploy SAP integrovaného vývojového prostředí systému SQL Server a operační systém Windows hello na Azure prostřednictvím hello SAP cloudu zařízení knihoven (SAP CAL) 3.0. snímky obrazovky Hello zobrazit podrobný postup hello. toodeploy jiné řešení, postupujte podle stejných kroků hello.

toostart s hello CAL SAP, přejděte toohello [knihovny zařízení cloudu SAP](https://cal.sap.com/) webu. SAP má také blog o hello nové [SAP cloudu zařízení knihovna 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 

> [!NOTE]
Jako 29. květen 2017, můžete pomocí modelu nasazení Azure Resource Manager hello kromě model nasazení classic méně než upřednostňovaný toohello toodeploy hello SAP CAL. Doporučujeme použít hello nového modelu nasazení Resource Manager a modelu nasazení classic hello ignorovat.

Pokud jste již vytvořili SAP CAL účet, který používá hello klasického modelu, *potřebujete toocreate jiný účet SAP CAL*. Tento účet musí tooexclusively nasadit do Azure pomocí modelu Resource Manager hello.

Po přihlášení toohello SAP CAL první stránku hello obvykle vás toohello **řešení** stránky. řešení Hello nabízené na hello SAP CAL jsou vytrvale zvýšení, tak může být nutné tooscroll odlišují hello řešení toofind, které chcete. Hello zvýrazněná integrovaného vývojového prostředí systému Windows SAP řešení, které je k dispozici výhradně na Azure ukazuje procesu nasazení hello:

![Řešení CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-hello-sap-cal"></a>Vytvoření účtu na hello SAP CAL
1. toosign v toohello SAP CAL pro hello poprvé, použijte S SAP-uživatele nebo jiný uživatel zaregistrována SAP. Poté definujte SAP CAL účtu, který je používán hello SAP CAL toodeploy zařízení v Azure. V definici hello účet budete muset:

    a. Vyberte model nasazení hello v Azure (Resource Manager nebo classic).

    b. Zadejte předplatné Azure. Účet SAP CAL lze přiřadit pouze tooone předplatné. Pokud potřebujete více než jedno předplatné, je třeba toocreate jiný účet SAP CAL.
    
    c. Dejte hello SAP CAL oprávnění toodeploy do vašeho předplatného Azure.

    > [!NOTE]
    Hello následující kroky ukazují, jak toocreate je SAP CAL účet pro nasazení Resource Manager. Pokud již máte účet SAP CAL, který je propojené toohello modelu nasazení classic, můžete *potřebovat* toofollow tyto kroky toocreate nový účet SAP CAL. nový účet SAP CAL Hello musí toodeploy v modelu Resource Manager hello.

2. toocreate nové CAL SAP účet, hello **účty** stránky zobrazí dvě možnosti pro Azure: 

    a. **Microsoft Azure (klasický)** je hello modelu nasazení classic a již není upřednostňovaný.

    b. **Microsoft Azure** je hello nového modelu nasazení Resource Manager.

    ![Účty CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    Vyberte toodeploy v modelu Resource Manager hello **Microsoft Azure**.

    ![Účty CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. Zadejte hello Azure **ID předplatného** , naleznete na portálu Azure hello. 

    ![ID předplatného CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. definované tooauthorize hello SAP CAL toodeploy do hello předplatné Azure, klikněte na tlačítko **Authorize**. hello záložce prohlížeče se zobrazí následující stránka Hello:

    ![Internet Explorer cloudových služeb přihlášení](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. Pokud je uveden více než jeden uživatel, zvolte účet Microsoft hello, který je propojený toobe hello spolusprávce hello předplatné Azure, které jste vybrali. hello záložce prohlížeče se zobrazí následující stránka Hello:

    ![Internet Explorer cloudové služby potvrzení](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. Klikněte na tlačítko **přijmout**. Pokud autorizace hello je úspěšné, hello Definice SAP CAL účtu se zobrazí znovu. Po krátkou dobu zprávu potvrdí, že proces autorizace hello bylo úspěšné.

7. tooassign hello nově vytvořený uživatel tooyour účet SAP CAL, zadejte vaše **ID uživatele** v hello textového pole na hello správné a klikněte na tlačítko **přidat**. 

    ![Přidružení toouser účtu](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. Klikněte na tlačítko tooassociate vašeho účtu pomocí hello uživatele, že používáte toosign toohello SAP CAL, **zkontrolujte**. 

9. Klikněte na tlačítko toocreate hello přidružení mezi uživateli a hello nově vytvořený účet SAP CAL, **vytvořit**.

    ![Tooaccount přidružení uživatele](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

Úspěšně jste vytvořili účet SAP CAL, aby bylo možné:

- Pomocí modelu nasazení Resource Manager hello.
- Nasazení systémů SAP do vašeho předplatného Azure.

> [!NOTE]
Před nasazením řešení integrovaného vývojového prostředí SAP hello založené na systému Windows a SQL Server, bude pravděpodobně nutné toosign pro předplatné SAP CAL. Jinak se může hello řešení nezobrazí jako **uzamčen** na stránce Přehled hello.

### <a name="deploy-a-solution"></a>Nasazení řešení
1. Když nastavíte účet SAP CAL, vyberte **hello řešení SAP integrovaného vývojového prostředí v systémech Windows a SQL Server** řešení. Klikněte na tlačítko **vytvořit instanci**a potvrďte podmínky použití a podmínky hello. 

2. Na hello **základní režim: Vytvořte instanci** stránky, potřebujete:

    a. Zadejte instance **název**.

    b. Vyberte Azure **oblast**. Můžete potřebovat SAP CAL tooget předplatného nabízí několika oblastmi Azure.

    c.  Zadejte hlavní hello **heslo** hello řešení, jak je znázorněno:

    ![SAP CAL Basic režim: Vytvoření Instance](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. Klikněte na možnost **Vytvořit**. Po určité době, v závislosti na složitosti hello řešení (hello, SAP CAL poskytuje odhad) a velikost hello hello stav se zobrazuje jako aktivní a připravena k použití: 

    ![Instance SAP Kalendáře](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. Skupina prostředků hello toofind a všechny její objekty, které byly vytvořeny hello SAP CAL, přejděte toohello portálu Azure. virtuální počítač Hello naleznete počínaje hello stejný název, který byl zadán v hello SAP CAL instance.

    ![Objekty skupiny prostředků](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. Na portálu SAP CAL hello, přejděte instancí toohello nasazení a klikněte na tlačítko **Connect**. Hello následující automaticky otevírané okno se zobrazí: 

    ![Připojit toohello Instance](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. Než budete moct použít jeden z hello možnosti tooconnect toohello nasazené systémy, klikněte na tlačítko **– Příručka Začínáme**. Hello dokumentace názvy hello uživatelů pro každou z metod hello připojení. Hello hesla pro uživatele, se nastavují toohello hlavní heslo, které jste definovali od začátku hello hello procesu nasazení. Hello dokumentace, jsou uvedena jiných více funkční uživatelů s jejich hesla, které můžete použít toosign v toohello nasazené systému.

    ![Vítejte v dokumentaci k SAP](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

V rámci několik hodin je v pořádku systému integrovaného vývojového prostředí SAP nasazené v Azure.

Pokud jste si zakoupili předplatné SAP CAL, SAP plně podporuje nasazení prostřednictvím hello SAP CAL na platformě Azure. fronta podporu Hello je BC. VCM CAL.

