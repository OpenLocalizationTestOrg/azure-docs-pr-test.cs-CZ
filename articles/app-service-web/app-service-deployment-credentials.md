---
title: "Přihlašovací údaje nasazení služby Azure App Service | Microsoft Docs"
description: "Naučte se používat přihlašovací údaje pro nasazení služby Azure App Service."
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
ms.openlocfilehash: 86a2cd8ae9f97c606a378452e44eec8941700531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a><span data-ttu-id="f4d8c-103">Konfigurovat přihlašovací údaje nasazení pro službu Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f4d8c-103">Configure deployment credentials for Azure App Service</span></span>
<span data-ttu-id="f4d8c-104">[Aplikační služba Azure](http://go.microsoft.com/fwlink/?LinkId=529714) podporuje dva typy přihlašovací údaje pro [místní nasazení Git](app-service-deploy-local-git.md) a [nasazení FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="f4d8c-104">[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) supports two types of credentials for [local Git deployment](app-service-deploy-local-git.md) and [FTP/S deployment](app-service-deploy-ftp.md).</span></span> <span data-ttu-id="f4d8c-105">To však nejsou stejné, jako přihlašovacích údajů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-105">These are not the same as your Azure Active Directory credentials.</span></span>

* <span data-ttu-id="f4d8c-106">**Přihlašovací údaje individuální**: jednu sadu přihlašovacích údajů pro celý účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-106">**User-level credentials**: one set of credentials for the entire Azure account.</span></span> <span data-ttu-id="f4d8c-107">Slouží k nasazení do služby App Service pro libovolnou aplikaci v žádné předplatné, který má oprávnění k přístupu k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-107">It can be used to deploy to App Service for any app, in any subscription, that the Azure account has permission to access.</span></span> <span data-ttu-id="f4d8c-108">Jedná se o výchozí sadu přihlašovacích údajů, které nakonfigurujete v **App Services** > **&lt;app_name >** > **přihlašovacíúdajenasazení.**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-108">These are the default credentials set that you configure in **App Services** > **&lt;app_name>** > **Deployment credentials**.</span></span> <span data-ttu-id="f4d8c-109">Toto je také výchozí sada, která se zobrazí v grafickém uživatelském rozhraní portálu (například **přehled** a **vlastnosti** vaší aplikace [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span><span class="sxs-lookup"><span data-stu-id="f4d8c-109">This is also the default set that's surfaced in the portal GUI (such as the **Overview** and **Properties** of your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span></span>

    > [!NOTE]
    > <span data-ttu-id="f4d8c-110">Při delegování přístupu k prostředkům Azure prostřednictvím na základě řízení přístupu Role (RBAC) nebo spolusprávce oprávnění každého Azure uživatele, který obdrží přístup k aplikaci můžete použít jejich osobní údaje individuální, dokud je odvolat přístup.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-110">When you delegate access to Azure resources via Role Based Access Control (RBAC) or co-admin permissions, each Azure user that receives access to an app can use his/her personal user-level credentials until access is revoked.</span></span> <span data-ttu-id="f4d8c-111">Tyto přihlašovací údaje nasazení by neměly sdílet s jinými uživateli Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-111">These deployment credentials should not be shared with other Azure users.</span></span>
    >
    >

* <span data-ttu-id="f4d8c-112">**Přihlašovací údaje úrovni aplikace**: jednu sadu přihlašovacích údajů pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-112">**App-level credentials**: one set of credentials for each app.</span></span> <span data-ttu-id="f4d8c-113">Slouží k nasazení této aplikace jenom.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-113">It can be used to deploy to that app only.</span></span> <span data-ttu-id="f4d8c-114">Přihlašovací údaje pro každou aplikaci je generován automaticky při vytváření aplikace a je nalezena v aplikace profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-114">The credentials for each app is generated automatically at app creation, and is found in the app's publish profile.</span></span> <span data-ttu-id="f4d8c-115">Nelze ručně nakonfigurujte pověření, ale můžete je obnovit pro aplikaci, kdykoli.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-115">You cannot manually configure the credentials, but you can reset them for an app anytime.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f4d8c-116">Aby bylo možné poskytnout někdo přístup pro tyto přihlašovací údaje prostřednictvím na základě řízení přístupu Role (RBAC), budete muset udělat, je Přispěvatel nebo vyšší na webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-116">In order to give someone access to these credentials via Role Based Access Control (RBAC), you need to make them contributor or higher on the Web App.</span></span> <span data-ttu-id="f4d8c-117">Čtečky není povoleno publikovat a proto nemá přístup k jejich přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-117">Readers are not allowed to publish, and hence can't access those credentials.</span></span>
    >
    >

## <span data-ttu-id="f4d8c-118"><a name="userscope"></a>Nastavit a resetovat přihlašovací údaje uživatele</span><span class="sxs-lookup"><span data-stu-id="f4d8c-118"><a name="userscope"></a>Set and reset user-level credentials</span></span>

<span data-ttu-id="f4d8c-119">Přihlašovací údaje uživatele můžete nakonfigurovat v jakékoli aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="f4d8c-119">You can configure your user-level credentials in any app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span> <span data-ttu-id="f4d8c-120">Bez ohledu na to v aplikaci, kterou konfigurujete tato pověření, platí pro všechny aplikace a pro všechna předplatná v účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-120">Regardless in which app you configure these credentials, it applies to all apps and for all subscriptions in your Azure account.</span></span> 

<span data-ttu-id="f4d8c-121">Konfigurace přihlašovacích údajů uživatele:</span><span class="sxs-lookup"><span data-stu-id="f4d8c-121">To configure your user-level credentials:</span></span>

1. <span data-ttu-id="f4d8c-122">V [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-122">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Deployment credentials**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f4d8c-123">Na portálu musí mít aspoň jednu aplikaci, než se dostanete k okno přihlašovací údaje nasazení.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-123">In the portal, you must have at least one app before you can access the deployment credentials blade.</span></span> <span data-ttu-id="f4d8c-124">Nicméně s [rozhraní příkazového řádku Azure](app-service-web-app-azure-resource-manager-xplat-cli.md), můžete nakonfigurovat přihlašovací údaje uživatele bez stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-124">However, with the [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), you can configure user-level credentials without an existing app.</span></span>

2. <span data-ttu-id="f4d8c-125">Nakonfigurujte uživatelské jméno a heslo a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-125">Configure the user name and password, and then click **Save**.</span></span>

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

<span data-ttu-id="f4d8c-126">Jakmile jednou nastavíte přihlašovací údaje nasazení, můžete najít *Git* nasazení uživatelské jméno ve vaší aplikaci **přehled**,</span><span class="sxs-lookup"><span data-stu-id="f4d8c-126">Once you have set your deployment credentials, you can find the *Git* deployment username in your app's **Overview**,</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

<span data-ttu-id="f4d8c-127">a a *FTP* nasazení uživatelské jméno ve vaší aplikaci **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-127">and and *FTP* deployment username in your app's **Properties**.</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> <span data-ttu-id="f4d8c-128">Azure nezobrazuje heslo individuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-128">Azure does not show your user-level deployment password.</span></span> <span data-ttu-id="f4d8c-129">Pokud zapomenete heslo, nelze jej načíst.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-129">If you forget the password, you can't retrieve it.</span></span> <span data-ttu-id="f4d8c-130">Však mohou obnovit svoje přihlašovací údaje podle kroků v této části.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-130">However, you can reset your credentials by following the steps in this section.</span></span>
>
>  

## <span data-ttu-id="f4d8c-131"><a name="appscope"></a>Získání a resetovat přihlašovací údaje úrovni aplikace</span><span class="sxs-lookup"><span data-stu-id="f4d8c-131"><a name="appscope"></a>Get and reset app-level credentials</span></span>
<span data-ttu-id="f4d8c-132">Pro každou aplikaci ve službě App Service svoje přihlašovací údaje na úrovni aplikace jsou uložené v souboru XML profilu publikování.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-132">For each app in App Service, its app-level credentials are stored in the XML publish profile.</span></span>

<span data-ttu-id="f4d8c-133">Pokud chcete získat přihlašovací údaje úrovni aplikace:</span><span class="sxs-lookup"><span data-stu-id="f4d8c-133">To get the app-level credentials:</span></span>

1. <span data-ttu-id="f4d8c-134">V [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přehled**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-134">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="f4d8c-135">Klikněte na tlačítko **... Další** > **profilu publikování Get**, a stažení spuštění. Soubor PublishSettings.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-135">Click **...More** > **Get publish profile**, and download starts for a .PublishSettings file.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. <span data-ttu-id="f4d8c-136">Otevřete. Soubor PublishSettings a najděte `<publishProfile>` značky s atributem `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-136">Open the .PublishSettings file and find the `<publishProfile>` tag with the attribute `publishMethod="FTP"`.</span></span> <span data-ttu-id="f4d8c-137">Poté, získat přístup k jeho `userName` a `password` atributy.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-137">Then, get its `userName` and `password` attributes.</span></span>
<span data-ttu-id="f4d8c-138">Jedná se o pověření úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-138">These are the app-level credentials.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    <span data-ttu-id="f4d8c-139">Podobně jako přihlašovací údaje uživatele, uživatelské jméno FTP nasazení je ve formátu `<app_name>\<username>`, a uživatelským jménem nasazení Git je právě `<username>` bez předchozím `<app_name>\`.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-139">Similar to the user-level credentials, the FTP deployment username is in the format of `<app_name>\<username>`, and the Git deployment username is just `<username>` without the preceding `<app_name>\`.</span></span>

<span data-ttu-id="f4d8c-140">Se resetovat přihlašovací údaje úrovni aplikace:</span><span class="sxs-lookup"><span data-stu-id="f4d8c-140">To reset the app-level credentials:</span></span>

1. <span data-ttu-id="f4d8c-141">V [portál Azure](https://portal.azure.com), klikněte na aplikační služby >  **&lt;any_app >** > **přehled**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-141">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="f4d8c-142">Klikněte na tlačítko **... Další** > **profil publikování se resetování**.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-142">Click **...More** > **Reset publish profile**.</span></span> <span data-ttu-id="f4d8c-143">Klikněte na tlačítko **Ano** potvrďte vynulování.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-143">Click **Yes** to confirm the reset.</span></span>

    <span data-ttu-id="f4d8c-144">Resetování akce zruší všechny dříve staženy. Soubory PublishSettings.</span><span class="sxs-lookup"><span data-stu-id="f4d8c-144">The reset action invalidates any previously-downloaded .PublishSettings files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4d8c-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4d8c-145">Next steps</span></span>

<span data-ttu-id="f4d8c-146">Zjistěte, jak používat tyto přihlašovací údaje k nasazení aplikace z [místní Git](app-service-deploy-local-git.md) nebo pomocí [FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="f4d8c-146">Find out how to use these credentials to deploy your app from [local Git](app-service-deploy-local-git.md) or using [FTP/S](app-service-deploy-ftp.md).</span></span>
