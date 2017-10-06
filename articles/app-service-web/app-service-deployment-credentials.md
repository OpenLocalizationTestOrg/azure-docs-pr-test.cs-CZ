---
title: "aaaAzure přihlašovací údaje nasazení pro aplikaci služby | Microsoft Docs"
description: "Zjistěte, jak toouse hello přihlašovací údaje pro nasazení služby Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: d6f9f5cc1b62a17c42643266f4c9490f827c63f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Konfigurovat přihlašovací údaje nasazení pro službu Azure App Service
[Aplikační služba Azure](http://go.microsoft.com/fwlink/?LinkId=529714) podporuje dva typy přihlašovací údaje pro [místní nasazení Git](app-service-deploy-local-git.md) a [nasazení FTP/S](app-service-deploy-ftp.md). Tyto nejsou hello stejné jako přihlašovacích údajů Azure Active Directory.

* **Přihlašovací údaje individuální**: jednu sadu pověření pro účet Azure celý hello. Použít toodeploy tooApp služba může být pro všechny aplikace v libovolné předplatné, které hello účet Azure má tooaccess oprávnění. Jedná se o hello výchozí přihlašovací údaje sadu, kterou nakonfigurujete v **App Services** > **&lt;app_name >** > **přihlašovacíúdajenasazení.**. To je také hello výchozí sadu, která se zobrazí v portálu hello grafickým uživatelským rozhraním (například hello **přehled** a **vlastnosti** vaší aplikace [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Při delegování přístup k prostředkům tooAzure prostřednictvím na základě řízení přístupu Role (RBAC) nebo spolusprávce oprávnění, můžete každý Azure uživatele, který obdrží aplikace access tooan jejich osobní údaje individuální používat, dokud je odvolat přístup. Tyto přihlašovací údaje nasazení by neměly sdílet s jinými uživateli Azure.
    >
    >

* **Přihlašovací údaje úrovni aplikace**: jednu sadu přihlašovacích údajů pro každou aplikaci. Dá se použít toodeploy toothat pouze aplikace. Hello přihlašovací údaje pro každou aplikaci je generován automaticky při vytváření aplikace a je nalezena v aplikaci hello profilu publikování. Nelze ručně nakonfigurovat hello pověření, ale můžete je obnovit pro aplikaci, kdykoli.

    > [!NOTE]
    > V pořadí toogive někdo přístupu k pověřením toothese prostřednictvím na základě řízení přístupu Role (RBAC), musíte toomake je Přispěvatel nebo vyšší na hello webové aplikace. Čtečky nejsou povoleny toopublish a proto nemůžete tyto přihlašovací údaje.
    >
    >

## <a name="userscope"></a>Nastavit a resetovat přihlašovací údaje uživatele

Přihlašovací údaje uživatele můžete nakonfigurovat v jakékoli aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources). Bez ohledu na to v aplikaci, kterou konfigurujete tato pověření, použije tooall aplikace a pro všechna předplatná v Azure, váš účet. 

tooconfigure přihlašovacích údajů uživatele:

1. V hello [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přihlašovací údaje nasazení**.

    > [!NOTE]
    > Hello portálu musí mít aspoň jednu aplikaci, než se dostanete k hello okno přihlašovací údaje nasazení. Nicméně s hello [rozhraní příkazového řádku Azure](app-service-web-app-azure-resource-manager-xplat-cli.md), můžete nakonfigurovat přihlašovací údaje uživatele bez stávající aplikace.

2. Nakonfigurujte hello uživatelské jméno a heslo a potom klikněte na **Uložit**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Jakmile jednou nastavíte přihlašovací údaje nasazení, můžete najít hello *Git* nasazení uživatelské jméno ve vaší aplikaci **přehled**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

a a *FTP* nasazení uživatelské jméno ve vaší aplikaci **vlastnosti**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure nezobrazuje heslo individuální nasazení. Pokud zapomenete heslo hello, nelze jej načíst. Však mohou obnovit svoje přihlašovací údaje podle následujících kroků hello v této části.
>
>  

## <a name="appscope"></a>Získání a resetovat přihlašovací údaje úrovni aplikace
Pro každou aplikaci ve službě App Service svoje přihlašovací údaje na úrovni aplikace jsou uloženy v hello XML profilu publikování.

tooget hello úrovni aplikace přihlašovací údaje:

1. V hello [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přehled**.

2. Klikněte na tlačítko **... Další** > **profilu publikování Get**, a stažení spuštění. Soubor PublishSettings.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Otevřete hello. Soubor PublishSettings a najde hello `<publishProfile>` značky s atributem hello `publishMethod="FTP"`. Poté, získat přístup k jeho `userName` a `password` atributy.
Toto jsou přihlašovací údaje hello úrovni aplikace.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Podobně jako přihlašovací údaje toohello individuální nasazení hello FTP uživatelské jméno je ve formátu hello `<app_name>\<username>`, a uživatelské jméno nasazení Git hello je právě `<username>` bez hello předchozí `<app_name>\`.

tooreset hello úrovni aplikace přihlašovací údaje:

1. V hello [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přehled**.

2. Klikněte na tlačítko **... Další** > **profil publikování se resetování**. Klikněte na tlačítko **Ano** tooconfirm hello resetovat.

    Hello resetování akce zruší všechny dříve staženy. Soubory PublishSettings.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toouse tyto přihlašovací údaje toodeploy aplikaci z [místní Git](app-service-deploy-local-git.md) nebo pomocí [FTP/S](app-service-deploy-ftp.md).
