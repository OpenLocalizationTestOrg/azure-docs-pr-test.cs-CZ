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
# <a name="configure-deployment-credentials-for-azure-app-service"></a><span data-ttu-id="bbfa2-103">Konfigurovat přihlašovací údaje nasazení pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bbfa2-103">Configure deployment credentials for Azure App Service</span></span>
<span data-ttu-id="bbfa2-104">[Aplikační služba Azure](http://go.microsoft.com/fwlink/?LinkId=529714) podporuje dva typy přihlašovací údaje pro [místní nasazení Git](app-service-deploy-local-git.md) a [nasazení FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="bbfa2-104">[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) supports two types of credentials for [local Git deployment](app-service-deploy-local-git.md) and [FTP/S deployment](app-service-deploy-ftp.md).</span></span> <span data-ttu-id="bbfa2-105">Tyto nejsou hello stejné jako přihlašovacích údajů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-105">These are not hello same as your Azure Active Directory credentials.</span></span>

* <span data-ttu-id="bbfa2-106">**Přihlašovací údaje individuální**: jednu sadu pověření pro účet Azure celý hello.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-106">**User-level credentials**: one set of credentials for hello entire Azure account.</span></span> <span data-ttu-id="bbfa2-107">Použít toodeploy tooApp služba může být pro všechny aplikace v libovolné předplatné, které hello účet Azure má tooaccess oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-107">It can be used toodeploy tooApp Service for any app, in any subscription, that hello Azure account has permission tooaccess.</span></span> <span data-ttu-id="bbfa2-108">Jedná se o hello výchozí přihlašovací údaje sadu, kterou nakonfigurujete v **App Services** > **&lt;app_name >** > **přihlašovacíúdajenasazení.**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-108">These are hello default credentials set that you configure in **App Services** > **&lt;app_name>** > **Deployment credentials**.</span></span> <span data-ttu-id="bbfa2-109">To je také hello výchozí sadu, která se zobrazí v portálu hello grafickým uživatelským rozhraním (například hello **přehled** a **vlastnosti** vaší aplikace [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span><span class="sxs-lookup"><span data-stu-id="bbfa2-109">This is also hello default set that's surfaced in hello portal GUI (such as hello **Overview** and **Properties** of your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span></span>

    > [!NOTE]
    > <span data-ttu-id="bbfa2-110">Při delegování přístup k prostředkům tooAzure prostřednictvím na základě řízení přístupu Role (RBAC) nebo spolusprávce oprávnění, můžete každý Azure uživatele, který obdrží aplikace access tooan jejich osobní údaje individuální používat, dokud je odvolat přístup.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-110">When you delegate access tooAzure resources via Role Based Access Control (RBAC) or co-admin permissions, each Azure user that receives access tooan app can use his/her personal user-level credentials until access is revoked.</span></span> <span data-ttu-id="bbfa2-111">Tyto přihlašovací údaje nasazení by neměly sdílet s jinými uživateli Azure.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-111">These deployment credentials should not be shared with other Azure users.</span></span>
    >
    >

* <span data-ttu-id="bbfa2-112">**Přihlašovací údaje úrovni aplikace**: jednu sadu přihlašovacích údajů pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-112">**App-level credentials**: one set of credentials for each app.</span></span> <span data-ttu-id="bbfa2-113">Dá se použít toodeploy toothat pouze aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-113">It can be used toodeploy toothat app only.</span></span> <span data-ttu-id="bbfa2-114">Hello přihlašovací údaje pro každou aplikaci je generován automaticky při vytváření aplikace a je nalezena v aplikaci hello profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-114">hello credentials for each app is generated automatically at app creation, and is found in hello app's publish profile.</span></span> <span data-ttu-id="bbfa2-115">Nelze ručně nakonfigurovat hello pověření, ale můžete je obnovit pro aplikaci, kdykoli.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-115">You cannot manually configure hello credentials, but you can reset them for an app anytime.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bbfa2-116">V pořadí toogive někdo přístupu k pověřením toothese prostřednictvím na základě řízení přístupu Role (RBAC), musíte toomake je Přispěvatel nebo vyšší na hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-116">In order toogive someone access toothese credentials via Role Based Access Control (RBAC), you need toomake them contributor or higher on hello Web App.</span></span> <span data-ttu-id="bbfa2-117">Čtečky nejsou povoleny toopublish a proto nemůžete tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-117">Readers are not allowed toopublish, and hence can't access those credentials.</span></span>
    >
    >

## <span data-ttu-id="bbfa2-118"><a name="userscope"></a>Nastavit a resetovat přihlašovací údaje uživatele</span><span class="sxs-lookup"><span data-stu-id="bbfa2-118"><a name="userscope"></a>Set and reset user-level credentials</span></span>

<span data-ttu-id="bbfa2-119">Přihlašovací údaje uživatele můžete nakonfigurovat v jakékoli aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="bbfa2-119">You can configure your user-level credentials in any app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span> <span data-ttu-id="bbfa2-120">Bez ohledu na to v aplikaci, kterou konfigurujete tato pověření, použije tooall aplikace a pro všechna předplatná v Azure, váš účet.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-120">Regardless in which app you configure these credentials, it applies tooall apps and for all subscriptions in your Azure account.</span></span> 

<span data-ttu-id="bbfa2-121">tooconfigure přihlašovacích údajů uživatele:</span><span class="sxs-lookup"><span data-stu-id="bbfa2-121">tooconfigure your user-level credentials:</span></span>

1. <span data-ttu-id="bbfa2-122">V hello [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-122">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Deployment credentials**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bbfa2-123">Hello portálu musí mít aspoň jednu aplikaci, než se dostanete k hello okno přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-123">In hello portal, you must have at least one app before you can access hello deployment credentials blade.</span></span> <span data-ttu-id="bbfa2-124">Nicméně s hello [rozhraní příkazového řádku Azure](app-service-web-app-azure-resource-manager-xplat-cli.md), můžete nakonfigurovat přihlašovací údaje uživatele bez stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-124">However, with hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), you can configure user-level credentials without an existing app.</span></span>

2. <span data-ttu-id="bbfa2-125">Nakonfigurujte hello uživatelské jméno a heslo a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-125">Configure hello user name and password, and then click **Save**.</span></span>

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

<span data-ttu-id="bbfa2-126">Jakmile jednou nastavíte přihlašovací údaje nasazení, můžete najít hello *Git* nasazení uživatelské jméno ve vaší aplikaci **přehled**,</span><span class="sxs-lookup"><span data-stu-id="bbfa2-126">Once you have set your deployment credentials, you can find hello *Git* deployment username in your app's **Overview**,</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

<span data-ttu-id="bbfa2-127">a a *FTP* nasazení uživatelské jméno ve vaší aplikaci **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-127">and and *FTP* deployment username in your app's **Properties**.</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> <span data-ttu-id="bbfa2-128">Azure nezobrazuje heslo individuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-128">Azure does not show your user-level deployment password.</span></span> <span data-ttu-id="bbfa2-129">Pokud zapomenete heslo hello, nelze jej načíst.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-129">If you forget hello password, you can't retrieve it.</span></span> <span data-ttu-id="bbfa2-130">Však mohou obnovit svoje přihlašovací údaje podle následujících kroků hello v této části.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-130">However, you can reset your credentials by following hello steps in this section.</span></span>
>
>  

## <span data-ttu-id="bbfa2-131"><a name="appscope"></a>Získání a resetovat přihlašovací údaje úrovni aplikace</span><span class="sxs-lookup"><span data-stu-id="bbfa2-131"><a name="appscope"></a>Get and reset app-level credentials</span></span>
<span data-ttu-id="bbfa2-132">Pro každou aplikaci ve službě App Service svoje přihlašovací údaje na úrovni aplikace jsou uloženy v hello XML profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-132">For each app in App Service, its app-level credentials are stored in hello XML publish profile.</span></span>

<span data-ttu-id="bbfa2-133">tooget hello úrovni aplikace přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="bbfa2-133">tooget hello app-level credentials:</span></span>

1. <span data-ttu-id="bbfa2-134">V hello [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přehled**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-134">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="bbfa2-135">Klikněte na tlačítko **... Další** > **profilu publikování Get**, a stažení spuštění. Soubor PublishSettings.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-135">Click **...More** > **Get publish profile**, and download starts for a .PublishSettings file.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. <span data-ttu-id="bbfa2-136">Otevřete hello. Soubor PublishSettings a najde hello `<publishProfile>` značky s atributem hello `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-136">Open hello .PublishSettings file and find hello `<publishProfile>` tag with hello attribute `publishMethod="FTP"`.</span></span> <span data-ttu-id="bbfa2-137">Poté, získat přístup k jeho `userName` a `password` atributy.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-137">Then, get its `userName` and `password` attributes.</span></span>
<span data-ttu-id="bbfa2-138">Toto jsou přihlašovací údaje hello úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-138">These are hello app-level credentials.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    <span data-ttu-id="bbfa2-139">Podobně jako přihlašovací údaje toohello individuální nasazení hello FTP uživatelské jméno je ve formátu hello `<app_name>\<username>`, a uživatelské jméno nasazení Git hello je právě `<username>` bez hello předchozí `<app_name>\`.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-139">Similar toohello user-level credentials, hello FTP deployment username is in hello format of `<app_name>\<username>`, and hello Git deployment username is just `<username>` without hello preceding `<app_name>\`.</span></span>

<span data-ttu-id="bbfa2-140">tooreset hello úrovni aplikace přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="bbfa2-140">tooreset hello app-level credentials:</span></span>

1. <span data-ttu-id="bbfa2-141">V hello [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přehled**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-141">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="bbfa2-142">Klikněte na tlačítko **... Další** > **profil publikování se resetování**.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-142">Click **...More** > **Reset publish profile**.</span></span> <span data-ttu-id="bbfa2-143">Klikněte na tlačítko **Ano** tooconfirm hello resetovat.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-143">Click **Yes** tooconfirm hello reset.</span></span>

    <span data-ttu-id="bbfa2-144">Hello resetování akce zruší všechny dříve staženy. Soubory PublishSettings.</span><span class="sxs-lookup"><span data-stu-id="bbfa2-144">hello reset action invalidates any previously-downloaded .PublishSettings files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbfa2-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbfa2-145">Next steps</span></span>

<span data-ttu-id="bbfa2-146">Zjistěte, jak toouse tyto přihlašovací údaje toodeploy aplikaci z [místní Git](app-service-deploy-local-git.md) nebo pomocí [FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="bbfa2-146">Find out how toouse these credentials toodeploy your app from [local Git](app-service-deploy-local-git.md) or using [FTP/S](app-service-deploy-ftp.md).</span></span>
