---
title: "aaaDeploy SAP S nebo 4HANA nebo BW/4HANA ve virtuálním počítači Azure | Microsoft Docs"
description: "Nasazení SAP S nebo 4HANA nebo BW/4HANA ve virtuálním počítači Azure"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 7e57f7daa7667b7c7dbcb86f6892a54e4295b74c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>Nasazení SAP S nebo 4HANA nebo BW/4HANA v Azure
Tento článek popisuje, jak toodeploy S nebo 4HANA v Azure pomocí hello SAP cloudu zařízení knihoven (SAP CAL) 3.0. toodeploy jiných řešení na základě SAP HANA, jako je například BW/4HANA, postupujte podle hello stejný postup.

> [!NOTE]
Další informace o hello SAP CAL, přejděte toohello [knihovny zařízení cloudu SAP](https://cal.sap.com/) webu. SAP má také blog o hello [SAP cloudu zařízení knihovna 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).

> [!NOTE]
Jako 29. květen 2017, můžete pomocí modelu nasazení Azure Resource Manager hello kromě model nasazení classic méně než upřednostňovaný toohello toodeploy hello SAP CAL. Doporučujeme použít hello nového modelu nasazení Resource Manager a modelu nasazení classic hello ignorovat.

## <a name="step-by-step-process-toodeploy-hello-solution"></a>Podrobný postup toodeploy hello řešení

Hello následující pořadí snímky obrazovky ukazuje, jak toodeploy S nebo 4HANA v Azure pomocí hello SAP CAL. proces Hello funguje hello stejným způsobem jako u jiných řešení, jako je například BW/4HANA.

Hello **řešení** stránky jsou uvedeny některé hello řešení založená na SAP CAL HANA dostupné v Azure. **SAP S nebo 4HANA 1610 FPS01, Fully-Activated zařízení** se v prostředním řádku hello:

![Řešení CAL SAP](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-hello-sap-cal"></a>Vytvoření účtu na hello SAP CAL
1. toosign v toohello SAP CAL pro hello poprvé, použijte S SAP-uživatele nebo jiný uživatel zaregistrována SAP. Poté definujte SAP CAL účtu, který je používán hello SAP CAL toodeploy zařízení v Azure. V definici hello účet budete muset:

    a. Vyberte model nasazení hello v Azure (Resource Manager nebo classic).

    b. Zadejte předplatné Azure. Účet SAP CAL lze přiřadit pouze tooone předplatné. Pokud potřebujete více než jedno předplatné, je třeba toocreate jiný účet SAP CAL.

    c. Dejte hello SAP CAL oprávnění toodeploy do vašeho předplatného Azure.

    > [!NOTE]
    Hello následující kroky ukazují, jak toocreate je SAP CAL účet pro nasazení Resource Manager. Pokud již máte účet SAP CAL, který je propojené toohello modelu nasazení classic, můžete *potřebovat* toofollow tyto kroky toocreate nový účet SAP CAL. nový účet SAP CAL Hello musí toodeploy v modelu Resource Manager hello.

2. Vytvořte nový účet SAP CAL. Hello **účty** stránka zobrazuje tři možnosti pro Azure: 

    a. **Microsoft Azure (klasický)** je hello modelu nasazení classic a již není upřednostňovaný.

    b. **Microsoft Azure** je hello nového modelu nasazení Resource Manager.

    c. **Windows Azure je provozována společností 21Vianet** je možnost v Číně, který používá model nasazení classic hello.

    Vyberte toodeploy v modelu Resource Manager hello **Microsoft Azure**.

    ![Podrobnosti účtu CAL SAP](./media/cal-s4h/s4h-pic-2a.png)

3. Zadejte hello Azure **ID předplatného** , naleznete na portálu Azure hello.

   ![Účty CAL SAP](./media/cal-s4h/s4h-pic3c.png)

4. definované tooauthorize hello SAP CAL toodeploy do hello předplatné Azure, klikněte na tlačítko **Authorize**. hello záložce prohlížeče se zobrazí následující stránka Hello:

   ![Internet Explorer cloudových služeb přihlášení](./media/cal-s4h/s4h-pic4c.png)

5. Pokud je uveden více než jeden uživatel, zvolte účet Microsoft hello, který je propojený toobe hello spolusprávce hello předplatné Azure, které jste vybrali. hello záložce prohlížeče se zobrazí následující stránka Hello:

   ![Internet Explorer cloudové služby potvrzení](./media/cal-s4h/s4h-pic5a.png)

6. Klikněte na tlačítko **přijmout**. Pokud autorizace hello je úspěšné, hello Definice SAP CAL účtu se zobrazí znovu. Po krátkou dobu zprávu potvrdí, že proces autorizace hello bylo úspěšné.

7. tooassign hello nově vytvořený uživatel tooyour účet SAP CAL, zadejte vaše **ID uživatele** v hello textového pole na hello správné a klikněte na tlačítko **přidat**.

   ![Přidružení toouser účtu](./media/cal-s4h/s4h-pic8a.png)

8. Klikněte na tlačítko tooassociate vašeho účtu pomocí hello uživatele, že používáte toosign toohello SAP CAL, **zkontrolujte**. 
 
9. Klikněte na tlačítko toocreate hello přidružení mezi uživateli a hello nově vytvořený účet SAP CAL, **vytvořit**.

   ![Přidružení účtu uživatele tooSAP Kalendáře](./media/cal-s4h/s4h-pic9b.png)

Úspěšně jste vytvořili účet SAP CAL, aby bylo možné:

- Pomocí modelu nasazení Resource Manager hello.
- Nasazení systémů SAP do vašeho předplatného Azure.

Nyní můžete začít S nebo 4HANA toodeploy do předplatného uživatele v Azure.

> [!NOTE]
Než budete pokračovat, zjistit, zda máte Azure základní kvóty pro virtuální počítače Azure H-Series. V okamžiku hello hello SAP CAL používá toodeploy H-Series virtuální počítače Azure některé řešení založená na SAP HANA hello. Vaše předplatné Azure nemusí mít žádné H-Series základní kvóty pro H-Series. Pokud ano, bude pravděpodobně nutné toocontact podporu Azure tooget kvótu alespoň 16 jader H-Series.

> [!NOTE]
Když nasadíte řešení v Azure v hello SAP CAL, můžete zjistit, které můžete zvolit pouze jedna oblast Azure. toodeploy do Azure oblastí, než hello jeden navrhovaná službou hello SAP CAL, musíte toopurchase odběru Kalendáře ze SAP. Také může musíte tooopen zprávu s SAP toohave vašeho účtu povoleno toodeliver CAL do Azure oblastí, než ty, které jsou původně navržený hello.

### <a name="deploy-a-solution"></a>Nasazení řešení

Umožňuje nasadit řešení z hello **řešení** stránku hello SAP CAL. Hello SAP CAL má dva toodeploy pořadí:

- Základní pořadí, které používá jednu stránku toodefine hello systému toobe nasazení
- Pokročilé pořadí, který vám dává některé možnosti na velikosti virtuálních počítačů 

Ukážeme hello základní cesta toodeployment sem.

1. Na hello **Podrobnosti účtu** stránky, potřebujete:

    a. Vyberte účet SAP CAL. (Použít účet, který je přidružený toodeploy pomocí modelu nasazení Resource Manager hello.)

    b. Zadejte instance **název**.

    c. Vyberte Azure **oblast**. Hello SAP CAL navrhuje oblast. Pokud potřebujete jinou oblast Azure a nemáte předplatné SAP CAL, potřebujete předplatné tooorder licence CAL s SAP.

    d. Zadejte hlavní **heslo** pro řešení hello osm nebo devět znaků. Hello heslo se používá pro hello správci hello různé součásti.

   ![SAP CAL Basic režim: Vytvoření Instance](./media/cal-s4h/s4h-pic10a.png)

2. Klikněte na tlačítko **vytvořit**a v hello se zprávou, klikněte na **OK**.

   ![SAP CAL podporované velikosti virtuálních počítačů](./media/cal-s4h/s4h-pic10b.png)

3. V hello **privátní klíč** dialogové okno, klikněte na tlačítko **úložiště** toostore hello privátní klíč v hello SAP CAL. Klikněte na tlačítko toouse ochrana heslem pro hello privátní klíč, **Stáhnout**. 

   ![SAP CAL privátní klíč](./media/cal-s4h/s4h-pic10c.png)

4. Čtení hello SAP CAL **upozornění** zpráva a klikněte na tlačítko **OK**.

   ![Upozornění CAL SAP](./media/cal-s4h/s4h-pic10d.png)

    Nyní hello nasazení probíhá. Po určité době, v závislosti na složitosti hello řešení (hello, SAP CAL poskytuje odhad) a velikost hello hello stav se zobrazuje jako aktivní a připravena k použití.

5. toofind hello virtuální počítače shromážděné pomocí hello další související prostředky v jedné skupině prostředků, přejděte toohello portálu Azure: 

   ![Objekty SAP CAL nasazené v hello nového portálu.](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. Na portálu hello SAP CAL, hello stav se zobrazí jako **Active**. tooconnect toohello řešení, klikněte na tlačítko **Connect**. Různé možnosti tooconnect toohello různé součásti jsou nasazeny v rámci tohoto řešení.

   ![Instance SAP Kalendáře](./media/cal-s4h/active_solution.png)

7. Než budete moct použít jeden z hello možnosti tooconnect toohello nasazené systémy, klikněte na tlačítko **– Příručka Začínáme**. 

   ![Připojit toohello Instance](./media/cal-s4h/connect_to_solution.png)

    Hello dokumentace názvy hello uživatelů pro každou z metod hello připojení. Hello hesla pro uživatele, se nastavují toohello hlavní heslo, které jste definovali od začátku hello hello procesu nasazení. Hello dokumentace, jsou uvedena jiných více funkční uživatelů s jejich hesla, které můžete použít toosign v toohello nasazené systému. 

    Například pokud použijete hello SAP grafickým uživatelským rozhraním, který je předinstalován v počítači vzdálené plochy Windows hello, hello S/4 systému může vypadat například takto:

   ![SM50 v hello předinstalována SAP grafického uživatelského rozhraní](./media/cal-s4h/gui_sm50.png)

    Nebo pokud používáte hello DBACockpit, hello instance může vypadat například takto:

   ![SM50 v hello DBACockpit SAP grafického uživatelského rozhraní](./media/cal-s4h/dbacockpit.png)

V rámci několik hodin je v pořádku zařízení SAP S/4 nasazené v Azure.

Pokud jste si zakoupili předplatné SAP CAL, SAP plně podporuje nasazení prostřednictvím hello SAP CAL na platformě Azure. fronta podporu Hello je BC. VCM CAL.







