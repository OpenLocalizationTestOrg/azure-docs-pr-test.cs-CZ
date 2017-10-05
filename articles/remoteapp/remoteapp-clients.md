---
title: "Přístup k aplikacím z libovolného zařízení | Microsoft Docs"
description: "Zjistěte, klientů, kteří jsou podporovány pro Azure RemoteApp a jak získat přístup k vaší aplikace."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: fb7bd17d-7aa8-43fd-9278-f96e0e9308e4
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 10a5be6251765b59fac92a33120cedcf8091a677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a>Přístup k aplikacím ve službě Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Jedním z beauties Azure RemoteApp je, že vám přístup k aplikacím, z libovolného zařízení. I lépe můžete začít pracovat na jednom zařízení a potom bezproblémově přechod do druhé zařízení a vyzvednutí vpravo, kde jste přestali. Abyste mohli začít budete muset stáhnout příslušnou klienta pro vaše zařízení a přihlaste se ke službě.

V tomto tématu: přečtěte aktuálně podporovaných klientů a jak je stáhnout před I ukazují, jak se přihlásit k Remoteappu z každé z klientů.

## <a name="supported-clients"></a>Podporovaní klienti
Mají přístup ke vzdálené aplikace RemoteApp pomocí následujícího postupu, pokud zařízení s jedním z těchto operačních systémů:

* Windows 10 
* Windows 8.1
* Windows 8
* Windows 7 Service Pack 1
* Windows Phone 8.1
* iOS
* Mac OS X
* Android

 Co tenké klienty? Jsou podporovány následující tenké klienty systému Windows Embedded:

* Windows Embedded Standard 7
* Windows Embedded 8 Standard
* Windows Embedded 8.1 Industry Pro
* Windows 10 IoT Enterprise

## <a name="downloading-the-client"></a>Stahování klienta
Bez ohledu na to, jakou platformu používáte klienta potřebujete přístup k vzdálené aplikace RemoteApp najdete na [stažení klienta vzdálené plochy](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) stránky.

Kliknutím na jiné odkazy buď přímo spustí stahování klienta nebo budou odeslány že klientovi stažení stránky na webu app store pro platformu. Nainstalujte klienta podle pokynů na obrazovce.

Po instalaci klienta na vašem zařízení a jeho spuštění, přejít na odpovídající části se dozvíte, jak se přihlásit k Remoteappu z tohoto klienta.

## <a name="android"></a>Android
Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu Google Play, můžete ji najít v seznamu aplikací v rámci **vzdálené plochy**.

1. Spuštění aplikace přináší můžete pro prázdný Center připojení, pokud jste již dosud používali aplikaci. Chcete-li začít s Azure Remoteappem, klepněte na tlačítko Přidat **"" +""** a klepněte na **Azure RemoteApp**.    
   
     ![Prázdný připojení Center](./media/remoteapp-clients/Android1.png)
2. Musíte se přihlásit pomocí e-mailovou adresu k přístupu ke službě. Klepněte na **Začínáme**.
   
    ![Přihlaste se řádku](./media/remoteapp-clients/Android2.png)
3. Na další stránce zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**. To zahájí proces přihlášení pomocí služby Azure Active Directory.
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/Android3.png)
4. Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (dříve nazývané "LiveID") a ID organizace. Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi. Pokud jste, vyberte pozvánky, kterým důvěřujete a klepněte na **provádí**.    
   
    ![Stránka pozvánek](./media/remoteapp-clients/Android4.png)
5. Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení. Klepněte na jednu z aplikací k jej začít používat.
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/Android5.png)
6. Pokud jste pozvánku ještě stále můžete vyzkoušet na služby. Chcete-li tak učinit, klepněte na **přejít na bezplatné zkušební verze** po zobrazení výzvy.
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/Android6.png)
7. Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a>iOS
Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu s aplikacemi, můžete ji najít v seznamu aplikací v rámci **klientovi služby Vzdálená plocha**.

1. Spuštění aplikace přináší můžete pro prázdný Center připojení, pokud jste již dosud používali aplikaci. Chcete-li začít s Azure Remoteappem, klepněte na tlačítko Přidat **"" +""** a klepněte na **přidat Azure RemoteApp**.
   
    ![Prázdný připojení Center](./media/remoteapp-clients/IOS1.png)
2. Je nutné se přihlásit pomocí e-mailovou adresu k přístupu ke službě, ke spuštění tohoto procesu zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**.
   
    ![Přihlaste se řádku](./media/remoteapp-clients/picture1.png)
3. Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace. Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi. Pokud jste, vyberte pozvánky, kterým důvěřujete a klepněte na **provádí**.
   
    ![Stránka pozvánek](./media/remoteapp-clients/IOS3.png)
4. Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení. Klepněte na jednu z aplikací a spustíte jej začít používat.
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/IOS4.png)
5. Pokud jste pozvánku ještě stále můžete vyzkoušet na služby. Chcete-li tak učinit, klepněte na **přejít na bezplatné zkušební verze** po zobrazení výzvy.
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/IOS5.png)
6. Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a>Mac OS X
Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu s aplikacemi, můžete ji najít v seznamu aplikací v rámci **Vzdálená plocha od Microsoftu**.

1. Spuštění aplikace přináší můžete pro prázdný Center připojení, pokud jste již dosud používali aplikaci. Chcete-li začít s Azure RemoteApp, klikněte na tlačítko **Azure RemoteApp** tlačítko.
   
    ![Prázdný připojení Center](./media/remoteapp-clients/Mac1.png)
2. Je nutné se přihlásit pomocí e-mailovou adresu k přístupu ke službě, ke spuštění procesu, klepněte na **Začínáme**.
   
    ![Přihlaste se řádku](./media/remoteapp-clients/Mac2.png)
3. Na další stránce zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**. To začne přihlašovací proces pomocí služby Azure Active Directory.
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/picture2.png)
4. Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace. Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi. Pokud jste, vyberte pozvánky, kterým důvěřujete a zavřete dialogové okno.
   
    ![Stránka pozvánek](./media/remoteapp-clients/Mac4.png)
5. Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení. Dvakrát klikněte na jednu z aplikací a spustíte jej začít používat.
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/Mac5.png)
6. Pokud jste pozvánku ještě stále můžete vyzkoušet na služby. Chcete-li to provést, klikněte na tlačítko **přejít na bezplatné zkušební verze** po zobrazení výzvy.
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/Mac6.png)
7. Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a>Windows (s výjimkou Windows Phone všechny podporované verze)
Klient spustí automaticky po dokončení instalace, ale pokud budete potřebovat pro přístup k ní později se nachází v seznamu aplikací pod názvem **Azure RemoteApp**.

1. Spuštění klienta, první stránka, kterou vidíte hřívací zařízení vás vítá do Azure Remoteappu. Chcete-li pokračovat, klikněte na **Začínáme**.
   
    ![Úvodní stránka klientovi Azure Remoteappu](./media/remoteapp-clients/Windows1.png)
2. Na další stránku spustí přihlašovací v procesu pro Azure RemoteApp pomocí služby Azure Active Directory. Tento proces by měla vypadat povědomě, pokud jste použili služby společnosti Microsoft v minulosti. Začněte tím, že zadáte vaše **e-mailová adresa** a klikněte na tlačítko **pokračovat**.
   
    ![První výzva služby Azure Active Directory](./media/remoteapp-clients/Windows2.png)
3. Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace. Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi. Pokud jste, vyberte pozvánky, kterým důvěřujete a klikněte na tlačítko **provádí**.
   
    ![Stránka pozvánek klientovi Azure Remoteappu](./media/remoteapp-clients/Windows3.png)
4. Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení. Dvakrát klikněte na jednu z aplikací a spustíte jej začít používat.
   
    ![Připojení středu klientovi Azure Remoteappu](./media/remoteapp-clients/Windows4.png)
5. Pokud žádná vám poslal ještě pozvánku, nemusíte si dělat starosti My jsme vám zahrnutých! Stále budete mít přístup ke kolekci ukázku, můžete otestovat na službu.
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a>Windows Phone 8.1
Po instalaci aplikace Vzdálená plocha společnosti Microsoft z obchodu Windows Phone 8.1, můžete ji najít v seznamu aplikací v rámci **vzdálené plochy**.

1. Spuštění aplikace přináší můžete přímo pro prázdný Center připojení, pokud jste již dosud používali aplikaci. Chcete-li začít s Azure Remoteappem, klepněte na tlačítko Přidat **"" +""** v dolní části obrazovky.
   
    ![Prázdný připojení Center](./media/remoteapp-clients/WinPhone1.png)
2. Potom klepněte na **Azure RemoteApp**.
   
    ![Přidat stránku položek](./media/remoteapp-clients/WinPhone2.png)
3. Je nutné se přihlásit pomocí e-mailovou adresu k přístupu ke službě, ke spuštění procesu, klepněte na **připojit**.
   
    ![Přihlaste se řádku](./media/remoteapp-clients/WinPhone3.png)
4. Na další stránce zadejte vaše **e-mailová adresa** a klepněte na **pokračovat**. To začne přihlašovací proces pomocí služby Azure Active Directory.
   
    ![První stránka Azure Active Directory](./media/remoteapp-clients/WinPhone4.png)
5. Postupujte podle pokynů na obrazovce se přihlásit pomocí účtu Microsoft (LiveID) a ID organizace. Po přihlášení může zobrazí se stránka výpis všech pozvánky, kterou jste obdrželi. Pokud jste, vyberte pozvánky, kterým důvěřujete a klepněte na **Uložit**.
   
    ![Stránka pozvánek](./media/remoteapp-clients/WinPhone5.png)
6. Po přijetí vaší pozvánky, seznam aplikací, které máte přístup k stáhne do zařízení a dostupné v centru připojení. Klepněte na jednu z aplikací a spustíte jej začít používat.
   
    ![Centrum připojení s informačního kanálu](./media/remoteapp-clients/WinPhone6.png)
7. Pokud jste pozvánku ještě stále můžete vyzkoušet na služby. Chcete-li tak učinit, klepněte na **Ano** po zobrazení výzvy.
   
    ![Ukázkový dotaz kanálu](./media/remoteapp-clients/WinPhone7.png)
8. Tím získáte přístup k základní sadu aplikací, které vám pomůžou začít s Remoteappem.
   
    ![Ukázkový kanál pro Azure RemoteApp](./media/remoteapp-clients/WinPhone8.png)

