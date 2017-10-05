---
title: Cluster HPC Pack s Azure Active Directory | Microsoft Docs
description: "Zjistěte, jak integrovat clusteru služby HPC Pack 2016 v Azure s Azure Active Directory"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: c5a06a9c810349b1bcce01c7f73563941a5af0ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a><span data-ttu-id="07c57-103">Spravovat cluster služby HPC Pack v Azure pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07c57-103">Manage an HPC Pack cluster in Azure using Azure Active Directory</span></span>
<span data-ttu-id="07c57-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) podporuje integraci se službou [Azure Active Directory](../../active-directory/index.md) (Azure AD) pro správce, kteří nasazení clusteru HPC Pack v Azure.</span><span class="sxs-lookup"><span data-stu-id="07c57-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) supports integration with [Azure Active Directory](../../active-directory/index.md) (Azure AD) for administrators who deploy an HPC Pack cluster in Azure.</span></span>



<span data-ttu-id="07c57-105">Postupujte podle kroků v tomto článku pro následující úlohy vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="07c57-105">Follow the steps in this article for the following high level tasks:</span></span> 
* <span data-ttu-id="07c57-106">Cluster HPC Pack ručně integrate klientovi Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c57-106">Manually integrate your HPC Pack cluster with your Azure AD tenant</span></span>
* <span data-ttu-id="07c57-107">Spravovat a plánování úloh v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="07c57-107">Manage and schedule jobs in your HPC Pack cluster in Azure</span></span> 

<span data-ttu-id="07c57-108">Integrace s Azure AD řešení clusteru prostředí HPC Pack postupuje standardní integrovat dalšími aplikacemi a službami.</span><span class="sxs-lookup"><span data-stu-id="07c57-108">Integrating an HPC Pack cluster solution with Azure AD follows standard steps to integrate other applications and services.</span></span> <span data-ttu-id="07c57-109">Tento článek předpokládá, že jste se seznámili s základní uživatel správy ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c57-109">This article assumes you are familiar with basic user management in Azure AD.</span></span> <span data-ttu-id="07c57-110">Další informace a pozadí, najdete v článku [dokumentaci služby Azure Active Directory](../../active-directory/index.md) a v následující části.</span><span class="sxs-lookup"><span data-stu-id="07c57-110">For more information and background, see the [Azure Active Directory documentation](../../active-directory/index.md) and the following section.</span></span>

## <a name="benefits-of-integration"></a><span data-ttu-id="07c57-111">Výhody integrace</span><span class="sxs-lookup"><span data-stu-id="07c57-111">Benefits of integration</span></span>


<span data-ttu-id="07c57-112">Azure Active Directory (Azure AD) je víceklientské cloudové adresáře a identity management služba, která poskytuje jednotné přihlašování (SSO) přístup k cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="07c57-112">Azure Active Directory (Azure AD) is a multi-tenant cloud-based directory and identity management service that provides single sign-on (SSO) access to cloud solutions.</span></span>

<span data-ttu-id="07c57-113">Integrace clusteru HPC Pack s Azure AD můžete dosáhnout sledovat tyto cíle:</span><span class="sxs-lookup"><span data-stu-id="07c57-113">Integration of an HPC Pack cluster with Azure AD can help you achieve the following goals:</span></span>

* <span data-ttu-id="07c57-114">Odeberte z clusteru HPC Pack tradiční řadič domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="07c57-114">Remove the traditional Active Directory domain controller from the HPC Pack cluster.</span></span> <span data-ttu-id="07c57-115">To může pomoct snížit náklady na údržbu clusteru, pokud to není nezbytné pro své firmy a zrychlit procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="07c57-115">This can help reduce the costs of maintaining the cluster if this is not necessary for your business, and speed-up the deployment process.</span></span>
* <span data-ttu-id="07c57-116">Využívejte následující výhody, které jsou uvedeny ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="07c57-116">Leverage the following benefits that are brought by Azure AD:</span></span>
    *   <span data-ttu-id="07c57-117">Jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="07c57-117">Single sign-on</span></span> 
    *   <span data-ttu-id="07c57-118">Pomocí místní AD identity pro cluster HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="07c57-118">Using a local AD identity for the HPC Pack cluster in Azure</span></span> 

    ![Prostředí Azure Active Directory](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a><span data-ttu-id="07c57-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07c57-120">Prerequisites</span></span>
* <span data-ttu-id="07c57-121">**Cluster HPC Pack 2016 nasazené ve virtuálních počítačích Azure** – pokyny najdete v tématu [nasazení clusteru HPC Pack 2016 v Azure](hpcpack-2016-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="07c57-121">**HPC Pack 2016 cluster deployed in Azure virtual machines** - For steps, see [Deploy an HPC Pack 2016 cluster in Azure](hpcpack-2016-cluster.md).</span></span> <span data-ttu-id="07c57-122">Potřebujete název DNS hlavního uzlu a přihlašovací údaje Správce clusteru k dokončení kroků v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="07c57-122">You need the DNS name of the head node and the credentials of a cluster administrator to complete the steps in this article.</span></span>

  > [!NOTE]
  > <span data-ttu-id="07c57-123">Integrace se službou Azure Active Directory není podporováno ve verzích sady HPC Pack před HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="07c57-123">Azure Active Directory integration is not supported in versions of HPC Pack before HPC Pack 2016.</span></span>



* <span data-ttu-id="07c57-124">**Klientský počítač** -potřebují klientský počítač Windows nebo Windows Server ke spuštění nástroje HPC Pack klienta.</span><span class="sxs-lookup"><span data-stu-id="07c57-124">**Client computer** - You need a Windows or Windows Server client computer to  run HPC Pack client utilities.</span></span> <span data-ttu-id="07c57-125">Pokud chcete používat k odesílání úloh HPC Pack webový portál nebo REST API, můžete použít libovolného klientského počítače podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="07c57-125">If you only want to use the HPC Pack web portal or REST API to submit jobs, you can use any client computer of your choice.</span></span>

* <span data-ttu-id="07c57-126">**HPC Pack klienta nástroje** -nainstalujte nástroje HPC Pack klienta v klientském počítači, pomocí dostupné volné instalační balíček z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="07c57-126">**HPC Pack client utilities** - Install the HPC Pack client utilities on the client computer, using the free installation package available from the Microsoft Download Center.</span></span>


## <a name="step-1-register-the-hpc-cluster-server-with-your-azure-ad-tenant"></a><span data-ttu-id="07c57-127">Krok 1: Zaregistrujte server clusteru HPC s klientovi Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c57-127">Step 1: Register the HPC cluster server with your Azure AD tenant</span></span>
1. <span data-ttu-id="07c57-128">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="07c57-128">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="07c57-129">Klikněte na tlačítko **služby Active Directory** v levé nabídce a pak klikněte na požadovaný adresář ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="07c57-129">Click **Active Directory** in the left menu, and then click the desired directory in your subscription.</span></span> <span data-ttu-id="07c57-130">Musíte mít oprávnění k přístupu k prostředkům v adresáři.</span><span class="sxs-lookup"><span data-stu-id="07c57-130">You must have permission to access resources in the directory.</span></span>
3. <span data-ttu-id="07c57-131">Klikněte na tlačítko **uživatelé**a ověřte, zda uživatelských účtů již vytvořené nebo nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="07c57-131">Click **Users**, and make sure there are user accounts already created or configured.</span></span>
4. <span data-ttu-id="07c57-132">Klikněte na tlačítko **aplikace** > **přidat**a potom klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="07c57-132">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="07c57-133">V průvodci zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="07c57-133">Enter the following information in the wizard:</span></span>
    * <span data-ttu-id="07c57-134">**Název** -HPCPackClusterServer</span><span class="sxs-lookup"><span data-stu-id="07c57-134">**Name** - HPCPackClusterServer</span></span>
    * <span data-ttu-id="07c57-135">**Typ** – vyberte **webové aplikace nebo webové rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="07c57-135">**Type** - Select **Web Application and/or Web API**</span></span>
    * <span data-ttu-id="07c57-136">**Adresa URL přihlašování**– základní adresu URL pro vzorku, který je ve výchozím nastavení`https://hpcserver`</span><span class="sxs-lookup"><span data-stu-id="07c57-136">**Sign-on URL**- The base URL for the sample, which is by default `https://hpcserver`</span></span>
    * <span data-ttu-id="07c57-137">**Identifikátor ID URI aplikace** - `https://<Directory_name>/<application_name>`.</span><span class="sxs-lookup"><span data-stu-id="07c57-137">**App ID URI** - `https://<Directory_name>/<application_name>`.</span></span> <span data-ttu-id="07c57-138">Nahraďte `<Directory_name`> s úplným názvem klientovi Azure AD, například `hpclocal.onmicrosoft.com`a nahraďte `<application_name>` s názvem, který jste vybrali dříve.</span><span class="sxs-lookup"><span data-stu-id="07c57-138">Replace `<Directory_name`> with the full name of your Azure AD tenant, for example, `hpclocal.onmicrosoft.com`, and replace `<application_name>` with the name you chose previously.</span></span>

5. <span data-ttu-id="07c57-139">Po přidání aplikace, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="07c57-139">After the app is added, click **Configure**.</span></span> <span data-ttu-id="07c57-140">Nakonfigurujte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="07c57-140">Configure the following properties:</span></span>
    * <span data-ttu-id="07c57-141">Vyberte **Ano** pro **aplikace je více klientů**</span><span class="sxs-lookup"><span data-stu-id="07c57-141">Select **Yes** for **Application is multi-tenant**</span></span>
    * <span data-ttu-id="07c57-142">Vyberte **Ano** pro **přiřazení uživatelských požadovaná pro přístup k aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="07c57-142">Select **Yes** for **User assignment required to access app**.</span></span>

6. <span data-ttu-id="07c57-143">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07c57-143">Click **Save**.</span></span> <span data-ttu-id="07c57-144">Po dokončení ukládání, klikněte na tlačítko **spravovat Manifest**.</span><span class="sxs-lookup"><span data-stu-id="07c57-144">When saving completes, click **Manage Manifest**.</span></span> <span data-ttu-id="07c57-145">Tato akce stáhne vaší aplikace manifestu JavaScript object notation (JSON) souboru.</span><span class="sxs-lookup"><span data-stu-id="07c57-145">This action downloads your application’s manifest JavaScript object notation (JSON) file.</span></span> <span data-ttu-id="07c57-146">Upravit stažené manifest tím, že se `appRoles` nastavení a přidání roli následující aplikace:</span><span class="sxs-lookup"><span data-stu-id="07c57-146">Edit the downloaded manifest by locating the `appRoles` setting and adding the following application role:</span></span>
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. <span data-ttu-id="07c57-147">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="07c57-147">Save the file.</span></span> <span data-ttu-id="07c57-148">Na portálu, klikněte na tlačítko **spravovat Manifest** > **nahrát Manifest**.</span><span class="sxs-lookup"><span data-stu-id="07c57-148">Then in the portal, click **Manage Manifest** > **Upload Manifest**.</span></span> <span data-ttu-id="07c57-149">Potom můžete nahrát upravená manifestu.</span><span class="sxs-lookup"><span data-stu-id="07c57-149">You can then upload the edited manifest.</span></span>
8. <span data-ttu-id="07c57-150">Klikněte na tlačítko **uživatelé**, vyberte uživatele a pak klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="07c57-150">Click **Users**, select a user, and then click **Assign**.</span></span> <span data-ttu-id="07c57-151">Přiřadíte jeden z dostupných rolí (HpcUsers nebo HpcAdminMirror) pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="07c57-151">Assign one of the available roles (HpcUsers or HpcAdminMirror) to the user.</span></span> <span data-ttu-id="07c57-152">Opakujte tento krok s dalším uživatelům v adresáři.</span><span class="sxs-lookup"><span data-stu-id="07c57-152">Repeat this step with additional users in the directory.</span></span> <span data-ttu-id="07c57-153">Základní informace o uživatelích, clusteru, najdete v části [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="07c57-153">For background information about cluster users, see [Managing Cluster Users](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span></span>

   > [!NOTE] 
   > <span data-ttu-id="07c57-154">Pokud chcete spravovat uživatele, doporučujeme použít okno preview služby Azure Active Directory v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="07c57-154">To manage users, we recommend using the Azure Active Directory preview blade in the [Azure portal](https://portal.azure.com).</span></span>
   >


## <a name="step-2-register-the-hpc-cluster-client-with-your-azure-ad-tenant"></a><span data-ttu-id="07c57-155">Krok 2: Registrace klienta clusteru HPC s klientovi Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c57-155">Step 2: Register the HPC cluster client with your Azure AD tenant</span></span>

1. <span data-ttu-id="07c57-156">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="07c57-156">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="07c57-157">Klikněte na tlačítko **služby Active Directory** v levé nabídce a pak klikněte na požadovaný adresář ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="07c57-157">Click **Active Directory** in the left menu, and then click the desired directory in your subscription.</span></span> <span data-ttu-id="07c57-158">Musíte mít oprávnění k přístupu k prostředkům v adresáři.</span><span class="sxs-lookup"><span data-stu-id="07c57-158">You must have permission to access resources in the directory.</span></span>
3. <span data-ttu-id="07c57-159">Klikněte na tlačítko **aplikace** > **přidat**a potom klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="07c57-159">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="07c57-160">V průvodci zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="07c57-160">Enter the following information in the wizard:</span></span>

    * <span data-ttu-id="07c57-161">**Název** -HPCPackClusterClient</span><span class="sxs-lookup"><span data-stu-id="07c57-161">**Name** - HPCPackClusterClient</span></span>
    * <span data-ttu-id="07c57-162">**Typ** – vyberte **nativní klientskou aplikaci**</span><span class="sxs-lookup"><span data-stu-id="07c57-162">**Type** - Select **Native Client Application**</span></span>
    * <span data-ttu-id="07c57-163">**Identifikátor URI pro přesměrování** - `http://hpcclient`</span><span class="sxs-lookup"><span data-stu-id="07c57-163">**Redirect URI** - `http://hpcclient`</span></span>

4. <span data-ttu-id="07c57-164">Po přidání aplikace, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="07c57-164">After the app is added, click **Configure**.</span></span> <span data-ttu-id="07c57-165">Kopírování **ID klienta** hodnotu a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="07c57-165">Copy the **Client ID** value and save it.</span></span> <span data-ttu-id="07c57-166">Budete potřebovat později při konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="07c57-166">You need this later when configuring your application.</span></span>

5. <span data-ttu-id="07c57-167">V **oprávnění k ostatním aplikacím**, klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="07c57-167">In **Permissions to other applications**, click **Add Application**.</span></span> <span data-ttu-id="07c57-168">Vyhledat a přidat aplikaci HpcPackClusterServer (vytvořený v kroku 1).</span><span class="sxs-lookup"><span data-stu-id="07c57-168">Search and add the  HpcPackClusterServer application (created in Step 1).</span></span>

6. <span data-ttu-id="07c57-169">V **delegovaná oprávnění** rozevíracího seznamu vyberte **přístup HpcClusterServer**.</span><span class="sxs-lookup"><span data-stu-id="07c57-169">In the **Delegated Permissions** dropdown, select **Access HpcClusterServer**.</span></span> <span data-ttu-id="07c57-170">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07c57-170">Then click **Save**.</span></span>


## <a name="step-3-configure-the-hpc-cluster"></a><span data-ttu-id="07c57-171">Krok 3: Konfigurace clusteru HPC</span><span class="sxs-lookup"><span data-stu-id="07c57-171">Step 3: Configure the HPC cluster</span></span>

1. <span data-ttu-id="07c57-172">Připojení k hlavnímu uzlu HPC Pack 2016 v Azure.</span><span class="sxs-lookup"><span data-stu-id="07c57-172">Connect to the HPC Pack 2016 head node in Azure.</span></span>

2. <span data-ttu-id="07c57-173">Spusťte prostředí HPC PowerShell.</span><span class="sxs-lookup"><span data-stu-id="07c57-173">Start HPC PowerShell.</span></span>

3. <span data-ttu-id="07c57-174">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07c57-174">Run the following command:</span></span>

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    <span data-ttu-id="07c57-175">kde</span><span class="sxs-lookup"><span data-stu-id="07c57-175">where</span></span>

    * <span data-ttu-id="07c57-176">`AADTenant`Určuje název klienta Azure AD, jako například`hpclocal.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="07c57-176">`AADTenant` specifies the Azure AD tenant name, such as `hpclocal.onmicrosoft.com`</span></span>
    * <span data-ttu-id="07c57-177">`AADClientAppId`Určuje ID klienta pro aplikaci vytvořili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="07c57-177">`AADClientAppId` specifies the client ID for the app created in Step 2.</span></span>

4. <span data-ttu-id="07c57-178">Restartujte službu HpcSchedulerStateful.</span><span class="sxs-lookup"><span data-stu-id="07c57-178">Restart the HpcSchedulerStateful service.</span></span>

    <span data-ttu-id="07c57-179">V clusteru s více hlavních uzlech můžete spustit následující příkazy prostředí PowerShell z hlavního uzlu přepnout primární repliky pro službu HpcSchedulerStateful:</span><span class="sxs-lookup"><span data-stu-id="07c57-179">In a cluster with multiple head nodes, you can run the following PowerShell commands on the head node to switch the primary replica for the HpcSchedulerStateful service:</span></span>

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-the-client"></a><span data-ttu-id="07c57-180">Krok 4: Správa a odesílání úloh z klienta</span><span class="sxs-lookup"><span data-stu-id="07c57-180">Step 4: Manage and submit jobs from the client</span></span>

<span data-ttu-id="07c57-181">Pokud chcete nainstalovat klienta nástroje HPC Pack ve vašem počítači, stáhněte instalační soubory HPC Pack 2016 (úplná instalace) z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="07c57-181">To install the HPC Pack client utilities on your computer, download the HPC Pack 2016 setup files (full installation) from the Microsoft Download Center.</span></span> <span data-ttu-id="07c57-182">Abyste před zahájením instalace, zvolte možnost instalační program **HPC Pack klienta nástroje**.</span><span class="sxs-lookup"><span data-stu-id="07c57-182">When you begin the installation, choose the setup option for the **HPC Pack client utilities**.</span></span>

<span data-ttu-id="07c57-183">Příprava klientského počítače, nainstalujte certifikát použitý během [nastavení clusteru HPC](hpcpack-2016-cluster.md) v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="07c57-183">To prepare the client computer, install the certificate used during [HPC cluster setup](hpcpack-2016-cluster.md) on the client computer.</span></span> <span data-ttu-id="07c57-184">Použijte standardní postupy správy certifikátu Windows veřejný certifikát, který chcete nainstalovat **certifikáty – aktuální uživatel** > **důvěryhodné kořenové certifikační autority** uložit.</span><span class="sxs-lookup"><span data-stu-id="07c57-184">Use standard Windows certificate management procedures to install the public certificate to the **Certificates – Current user** > **Trusted Root Certification Authorities** store.</span></span> 

<span data-ttu-id="07c57-185">Nyní můžete spustit příkazy HPC Pack nebo pomocí Správce úloh HPC Pack grafickým uživatelským rozhraním odesílat a spravovat úlohy clusteru pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c57-185">You can now run the HPC Pack commands or use the HPC Pack Job manager GUI to submit and manage cluster jobs by using the Azure AD account.</span></span> <span data-ttu-id="07c57-186">Možnosti pro odeslání úlohy najdete v tématu [clusteru HPC odeslání úlohy HPC Pack v Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="07c57-186">For job submission options, see [Submit HPC jobs to an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span></span>

> [!NOTE]
> <span data-ttu-id="07c57-187">Při pokusu o připojení ke clusteru HPC Pack v Azure poprvé, zobrazí se místní windows.</span><span class="sxs-lookup"><span data-stu-id="07c57-187">When you try to connect to the HPC Pack cluster in Azure for the first time, a popup windows appears.</span></span> <span data-ttu-id="07c57-188">Zadejte přihlašovací údaje Azure AD k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="07c57-188">Enter your Azure AD credentials to log in.</span></span> <span data-ttu-id="07c57-189">Token se pak uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="07c57-189">The token is then cached.</span></span> <span data-ttu-id="07c57-190">Novější připojení do clusteru v Azure používat token v mezipaměti, pokud není zaškrtnuté změny v ověřování, nebo v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="07c57-190">Later connections to the cluster in Azure use the cached token unless authentication changes or the cached is cleared.</span></span>
>
  
<span data-ttu-id="07c57-191">Například po dokončení předchozích kroků se můžete dotazovat pro úlohy z lokálního klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="07c57-191">For example, after completing the previous steps, you can query for jobs from an on-premises client as follows:</span></span>

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a><span data-ttu-id="07c57-192">Užitečné rutiny pro odeslání úlohy s integrací služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c57-192">Useful cmdlets for job submission with Azure AD integration</span></span> 

### <a name="manage-the-local-token-cache"></a><span data-ttu-id="07c57-193">Spravovat místní mezipaměti tokenu</span><span class="sxs-lookup"><span data-stu-id="07c57-193">Manage the local token cache</span></span>

<span data-ttu-id="07c57-194">HPC Pack 2016 poskytuje dvě nové rutiny prostředí HPC PowerShell ke správě místního mezipamětí tokenů.</span><span class="sxs-lookup"><span data-stu-id="07c57-194">HPC Pack 2016 provides two new HPC PowerShell cmdlets to manage the local token cache.</span></span> <span data-ttu-id="07c57-195">Tyto rutiny jsou užitečné pro odesílání úloh interaktivně.</span><span class="sxs-lookup"><span data-stu-id="07c57-195">These cmdlets are useful for submitting jobs non-interactively.</span></span> <span data-ttu-id="07c57-196">Podívejte se na následující příklad:</span><span class="sxs-lookup"><span data-stu-id="07c57-196">See the following example:</span></span>

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-the-credentials-for-submitting-jobs-using-the-azure-ad-account"></a><span data-ttu-id="07c57-197">Nastavte přihlašovací údaje pro odesílání úloh pomocí účtu Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c57-197">Set the credentials for submitting jobs using the Azure AD account</span></span> 

<span data-ttu-id="07c57-198">V některých případech můžete chtít spustit úlohu uživatele clusteru prostředí HPC (pro připojené k doméně clusteru HPC, spusťte jako jednu doménu uživatele; pro domény nepřipojená cluster prostředí HPC, spustit jako jeden místní uživatel z hlavního uzlu).</span><span class="sxs-lookup"><span data-stu-id="07c57-198">Sometimes, you may want to run the job under the HPC cluster user (for a domain-joined HPC cluster, run as one domain user; for a non-domain-joined HPC cluster, run as one local user on the head node).</span></span>

1. <span data-ttu-id="07c57-199">Pomocí následujících příkazů nastavit přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="07c57-199">Use the following commands to set the credentials:</span></span>

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. <span data-ttu-id="07c57-200">Potom takto odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="07c57-200">Then submit the job as follows.</span></span> <span data-ttu-id="07c57-201">Úloh se spouští v části $localUser výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="07c57-201">The job/task runs under $localUser on the compute nodes.</span></span>

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   <span data-ttu-id="07c57-202">Pokud `–Credential` není zadaný s `Submit-HpcJob`, úlohy nebo úkolu spouští pod místním uživatelským namapované jako účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c57-202">If `–Credential` is not specified with `Submit-HpcJob`, the job or task runs under a local mapped user as the Azure AD account.</span></span> <span data-ttu-id="07c57-203">(Clusteru HPC vytvoří místní uživatele se stejným názvem jako účet služby Azure AD, který chcete spustit úlohu.)</span><span class="sxs-lookup"><span data-stu-id="07c57-203">(The HPC cluster creates a local user with the same name as the Azure AD account to run the task.)</span></span>
    
3. <span data-ttu-id="07c57-204">Nastavit rozšířená data pro účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c57-204">Set extended data for the Azure AD account.</span></span> <span data-ttu-id="07c57-205">To je užitečné, pokud se používá úlohu MPI na Linuxových uzlů pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07c57-205">This is useful when running an MPI job on Linux nodes using the Azure AD account.</span></span>

   * <span data-ttu-id="07c57-206">Nastavit rozšířená data pro samotné účet Azure AD</span><span class="sxs-lookup"><span data-stu-id="07c57-206">Set extended data for the Azure AD account itself</span></span>

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * <span data-ttu-id="07c57-207">Nastavte Rozšířená data a spusťte jako uživatel clusteru HPC</span><span class="sxs-lookup"><span data-stu-id="07c57-207">Set extended data and run as HPC cluster user</span></span>
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

