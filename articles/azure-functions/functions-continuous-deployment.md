---
title: "aaaContinuous nasazení pro Azure Functions | Microsoft Docs"
description: "Průběžné nasazování vlastnosti služby Azure App Service toopublish použijte Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 28c44f737dad3feab3cf54f7dd42b6a978d0617e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-for-azure-functions"></a>Průběžné nasazování se službou Azure Functions
Azure Functions umožňuje snadno toodeploy funkce aplikace pomocí průběžnou integraci služby App Service. Funkce se integruje s BitBucket, Dropbox, Githubu a Visual Studio Team Services (VSTS). To umožňuje pracovní kde kód funkce aktualizace provedené pomocí jedné z těchto tooAzure nasazení aktivační událost integrovaných služeb. Pokud jste nové funkce tooAzure, začínat [přehled Azure Functions](functions-overview.md).

Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často. To také umožňuje udržovat zdrojového kódu na váš kód funkce. Hello následující nasazení zdroje jsou aktuálně podporovány:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Externího úložiště (Git nebo Mercurial)
* [Místní úložiště Git](../app-service-web/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

Nasazení jsou nakonfigurované na základě aplikací na funkce. Po povolení průběžné nasazování kód toofunction přístup k portálu hello je nastaven příliš*jen pro čtení*.

## <a name="continuous-deployment-requirements"></a>Průběžné nasazování požadavky

Váš zdroj nasazení nakonfigurované a váš kód funkce musí mít v zdroj nasazení hello před nastavit průběžné nasazování. V nasazení aplikace danou funkci jednotlivé funkce žije v podadresáři s názvem, kde je název adresáře hello hello název funkce hello.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a>Nastavení nepřetržitého nasazování
Použijte tento postup tooconfigure průběžné nasazování pro existující aplikaci funkce. Tyto kroky ukazují integrace s úložišti GitHub, ale podobným způsobem platí pro Visual Studio Team Services nebo jiné služby pro nasazení.

1. V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **možnosti nasazení**. 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Potom v hello **nasazení** okně klikněte na tlačítko **instalační program**.
 
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. V hello **zdroj nasazení** okně klikněte na tlačítko **vybrat zdroj**, potom vyplňte hello informace pro váš zdroj vybraný nasazení a klikněte na **OK**.
   
    ![Vyberte zdroj nasazení.](./media/functions-continuous-deployment/choose-deployment-source.png)

Po dokončení konfigurace průběžné nasazování, všechny změny souboru ve zdroji nasazení jsou zkopírovaný toohello funkce aplikace a nasazení plnou verzi webu se aktivuje. Hello lokality je znovu nasazena, když jsou aktualizovány soubory ve zdroji hello.

## <a name="deployment-options"></a>Možnosti nasazení

Hello Následují některé typické nasazení scénáře:

- [Vytvořit pracovní nasazení](#staging)
- [Přesunutí existující funkce toocontinuous nasazení](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Vytvořit pracovní nasazení

Funkce aplikace ještě nepodporuje nasazovací sloty. Samostatná pracovní a provozní nasazení však můžete spravovat pomocí nepřetržité integrace.

Hello tooconfigure procesu a pracovat s pracovní nasazení obecně vypadat třeba takto:

1. Vaše předplatné, jeden pro hello produkčním kódu a jeden pro pracovní vytvořte dvě funkce aplikace. 

2. Pokud jste ještě nemáte, vytvořte zdroj nasazení. Tento příklad používá [Githubu].

3. Pro funkce aplikace produkční dokončení hello předchozí kroky **nastavit průběžné nasazování** a sadu hello nasazení větve toohello hlavní větve ve vašem úložišti GitHub.
   
    ![Vyberte větev nasazení](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Tento krok opakujte pro hello pracovní funkce aplikace, ale zvolte hello pracovní větev místo ve vašem úložišti GitHub. Pokud vaše zdrojová nasazení nepodporuje vytvoření větve, použijte jinou složku.
    
5. Proveďte aktualizace tooyour kódu v hello pracovní větev nebo složku a potom ověřte, že tyto změny se projeví v hello pracovní nasazení.

6. Po testování sloučit změny z hello pracovní větve do hlavní větve hello. Tato sloučení aktivuje nasazení toohello produkční funkce aplikace. Pokud vaše zdrojová nasazení nepodporuje větví, přepište hello souborů ve složce produkční hello hello soubory z hello pracovní složku.

<a name="existing"></a>
### <a name="move-existing-functions-toocontinuous-deployment"></a>Přesunutí existující funkce toocontinuous nasazení
Pokud máte existující funkce, které jste vytvořili a udržovat hello portálu, je nutné toodownload stávající funkce soubory kódu pomocí protokol FTP nebo hello místní úložiště Git před průběžné nasazování můžete nastavit, jak je popsáno výše. To provedete v hello nastavení služby App Service pro vaši aplikaci funkce. Po stažení jsou vaše soubory, můžete je načíst zdroj tooyour vybrali průběžné nasazování.

> [!NOTE]
> Po dokončení konfigurace průběžnou integraci, už nebudete moct tooedit vaší zdrojové soubory hello funkce portálu.

- [Postupy: konfigurace přihlašovací údaje nasazení.](#credentials)
- [Postupy: stažení souborů pomocí protokolu FTP](#downftp)
- [Postupy: stahovat soubory s použitím hello místní úložiště Git](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Postupy: konfigurace přihlašovací údaje nasazení.
Před soubory si můžete stáhnout z funkce aplikace pomocí protokol FTP nebo místní úložiště Git, je nutné nakonfigurovat lokalitu hello tooaccess přihlašovací údaje. Přihlašovací údaje jsou nastavené na úrovni aplikace hello funkce. Použijte následující kroky přihlašovací údaje nasazení tooset v hello portál Azure hello:

1. V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **přihlašovací údaje nasazení**.
   
    ![Nastavte přihlašovací údaje pro místní nasazení](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Zadejte uživatelské jméno a heslo a pak klikněte na **Uložit**. Nyní můžete tyto přihlašovací údaje tooaccess funkce aplikaci z protokol FTP nebo hello integrované úložiště Git.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Postupy: stažení souborů pomocí protokolu FTP

1. V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **vlastnosti**, zkopírujte hodnoty hello **uživatel FTP/nasazení**, **Název hostitele FTP**, a **název hostitele FTPS**.  

    **Uživatel FTP/nasazení** musí zadat, jak je uvedeno v hello portál, včetně názvu aplikace hello, tooprovide správný kontext hello FTP serveru.
   
    ![Získat informace o nasazení](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Z vašeho klienta FTP hello připojení informací, které jste shromáždili tooconnect tooyour aplikace a stáhnout hello zdrojové soubory pro funkce.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Postupy: stahovat soubory s použitím místní úložiště Git

1. V aplikaci funkce v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **funkce** a **možnosti nasazení**. 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Potom v hello **nasazení** okně klikněte na tlačítko **instalační program**.
 
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. V hello **zdroj nasazení** okně klikněte na tlačítko **úložiště místní Git** a pak klikněte na **OK**.

3. V **funkce**, klikněte na tlačítko **vlastnosti** a poznamenejte si hodnotu hello adresu URL pro Git. 
   
    ![Nastavit průběžné nasazování.](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Klon hello úložiště na místním počítači pomocí Git využívající příkazový řádek nebo vaše oblíbené nástroje Git. Hello příkaz Git clone vypadá takto:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Načtení souborů z vaší funkce aplikace toohello klonu v místním počítači, jako hello následující ukázka:
   
        git pull origin master
   
    Pokud požadovaný, zadejte vaše [nakonfigurovat přihlašovací údaje nasazení](#credentials).  

[Githubu]: https://github.com/
