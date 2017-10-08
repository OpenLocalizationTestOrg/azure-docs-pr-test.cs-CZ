---
title: aaaHPC Pack cluster s Azure Active Directory | Microsoft Docs
description: "Zjistěte, jak toointegrate HPC Pack 2016 cluster v Azure s Azure Active Directory"
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
ms.openlocfilehash: 0806e544a468e27ca0567e18c55554811584fbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a><span data-ttu-id="386bd-103">Spravovat cluster služby HPC Pack v Azure pomocí Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="386bd-103">Manage an HPC Pack cluster in Azure using Azure Active Directory</span></span>
<span data-ttu-id="386bd-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) podporuje integraci se službou [Azure Active Directory](../../active-directory/index.md) (Azure AD) pro správce, kteří nasazení clusteru HPC Pack v Azure.</span><span class="sxs-lookup"><span data-stu-id="386bd-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) supports integration with [Azure Active Directory](../../active-directory/index.md) (Azure AD) for administrators who deploy an HPC Pack cluster in Azure.</span></span>



<span data-ttu-id="386bd-105">Postupujte podle kroků hello v tomto článku hello následující úlohy vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="386bd-105">Follow hello steps in this article for hello following high level tasks:</span></span> 
* <span data-ttu-id="386bd-106">Cluster HPC Pack ručně integrate klientovi Azure AD</span><span class="sxs-lookup"><span data-stu-id="386bd-106">Manually integrate your HPC Pack cluster with your Azure AD tenant</span></span>
* <span data-ttu-id="386bd-107">Spravovat a plánování úloh v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="386bd-107">Manage and schedule jobs in your HPC Pack cluster in Azure</span></span> 

<span data-ttu-id="386bd-108">Integrace s Azure AD řešení clusteru prostředí HPC Pack následuje toointegrate standardní kroky, ostatní aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="386bd-108">Integrating an HPC Pack cluster solution with Azure AD follows standard steps toointegrate other applications and services.</span></span> <span data-ttu-id="386bd-109">Tento článek předpokládá, že jste se seznámili s základní uživatel správy ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="386bd-109">This article assumes you are familiar with basic user management in Azure AD.</span></span> <span data-ttu-id="386bd-110">Další informace a pozadí, najdete v části hello [dokumentaci služby Azure Active Directory](../../active-directory/index.md) a hello následující části.</span><span class="sxs-lookup"><span data-stu-id="386bd-110">For more information and background, see hello [Azure Active Directory documentation](../../active-directory/index.md) and hello following section.</span></span>

## <a name="benefits-of-integration"></a><span data-ttu-id="386bd-111">Výhody integrace</span><span class="sxs-lookup"><span data-stu-id="386bd-111">Benefits of integration</span></span>


<span data-ttu-id="386bd-112">Azure Active Directory (Azure AD) je víceklientské cloudové adresáře a identity management služba, která poskytuje jednotné přihlašování (SSO) přístup toocloud řešení.</span><span class="sxs-lookup"><span data-stu-id="386bd-112">Azure Active Directory (Azure AD) is a multi-tenant cloud-based directory and identity management service that provides single sign-on (SSO) access toocloud solutions.</span></span>

<span data-ttu-id="386bd-113">Integrace clusteru HPC Pack s Azure AD můžete dosáhnout hello následující cíle:</span><span class="sxs-lookup"><span data-stu-id="386bd-113">Integration of an HPC Pack cluster with Azure AD can help you achieve hello following goals:</span></span>

* <span data-ttu-id="386bd-114">Odeberte z clusteru HPC Pack hello hello tradiční řadič domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="386bd-114">Remove hello traditional Active Directory domain controller from hello HPC Pack cluster.</span></span> <span data-ttu-id="386bd-115">To může pomoct snížit náklady na hello Správa clusteru hello, pokud to není nezbytné pro své firmy a proces nasazení hello zrychlit.</span><span class="sxs-lookup"><span data-stu-id="386bd-115">This can help reduce hello costs of maintaining hello cluster if this is not necessary for your business, and speed-up hello deployment process.</span></span>
* <span data-ttu-id="386bd-116">Využít hello následující výhody, které jsou uvedeny ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="386bd-116">Leverage hello following benefits that are brought by Azure AD:</span></span>
    *   <span data-ttu-id="386bd-117">Jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="386bd-117">Single sign-on</span></span> 
    *   <span data-ttu-id="386bd-118">Pomocí místní AD identity pro cluster HPC Pack hello v Azure</span><span class="sxs-lookup"><span data-stu-id="386bd-118">Using a local AD identity for hello HPC Pack cluster in Azure</span></span> 

    ![Prostředí Azure Active Directory](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a><span data-ttu-id="386bd-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="386bd-120">Prerequisites</span></span>
* <span data-ttu-id="386bd-121">**Cluster HPC Pack 2016 nasazené ve virtuálních počítačích Azure** – pokyny najdete v tématu [nasazení clusteru HPC Pack 2016 v Azure](hpcpack-2016-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="386bd-121">**HPC Pack 2016 cluster deployed in Azure virtual machines** - For steps, see [Deploy an HPC Pack 2016 cluster in Azure](hpcpack-2016-cluster.md).</span></span> <span data-ttu-id="386bd-122">Potřebujete název DNS hello hello hlavního uzlu a hello přihlašovací údaje Správce clusteru dokončit hello kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="386bd-122">You need hello DNS name of hello head node and hello credentials of a cluster administrator to complete hello steps in this article.</span></span>

  > [!NOTE]
  > <span data-ttu-id="386bd-123">Integrace se službou Azure Active Directory není podporováno ve verzích sady HPC Pack před HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="386bd-123">Azure Active Directory integration is not supported in versions of HPC Pack before HPC Pack 2016.</span></span>



* <span data-ttu-id="386bd-124">**Klientský počítač** -je nutné Windows nebo Windows Server klientské počítače příliš spustit HPC Pack klienta nástroje.</span><span class="sxs-lookup"><span data-stu-id="386bd-124">**Client computer** - You need a Windows or Windows Server client computer too run HPC Pack client utilities.</span></span> <span data-ttu-id="386bd-125">Pokud chcete pouze toouse hello HPC Pack webový portál nebo REST API toosubmit úloh, můžete použít libovolného klientského počítače podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="386bd-125">If you only want toouse hello HPC Pack web portal or REST API toosubmit jobs, you can use any client computer of your choice.</span></span>

* <span data-ttu-id="386bd-126">**HPC Pack klienta nástroje** -nainstalujte na hello klientského počítače, pomocí hello volné instalačního balíčku k dispozici z hello Microsoft Download Center hello HPC Pack klienta nástroje.</span><span class="sxs-lookup"><span data-stu-id="386bd-126">**HPC Pack client utilities** - Install hello HPC Pack client utilities on hello client computer, using hello free installation package available from hello Microsoft Download Center.</span></span>


## <a name="step-1-register-hello-hpc-cluster-server-with-your-azure-ad-tenant"></a><span data-ttu-id="386bd-127">Krok 1: Zaregistrujte server clusteru HPC hello se klientovi služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="386bd-127">Step 1: Register hello HPC cluster server with your Azure AD tenant</span></span>
1. <span data-ttu-id="386bd-128">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="386bd-128">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="386bd-129">Klikněte na tlačítko **služby Active Directory** v levé nabídce text hello a potom klikněte na hello požadovaného adresáře v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="386bd-129">Click **Active Directory** in hello left menu, and then click hello desired directory in your subscription.</span></span> <span data-ttu-id="386bd-130">V adresáři hello musí mít oprávnění tooaccess prostředky.</span><span class="sxs-lookup"><span data-stu-id="386bd-130">You must have permission tooaccess resources in hello directory.</span></span>
3. <span data-ttu-id="386bd-131">Klikněte na tlačítko **uživatelé**a ověřte, zda uživatelských účtů již vytvořené nebo nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="386bd-131">Click **Users**, and make sure there are user accounts already created or configured.</span></span>
4. <span data-ttu-id="386bd-132">Klikněte na tlačítko **aplikace** > **přidat**a potom klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="386bd-132">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="386bd-133">Zadejte následující informace v Průvodci hello hello:</span><span class="sxs-lookup"><span data-stu-id="386bd-133">Enter hello following information in hello wizard:</span></span>
    * <span data-ttu-id="386bd-134">**Název** -HPCPackClusterServer</span><span class="sxs-lookup"><span data-stu-id="386bd-134">**Name** - HPCPackClusterServer</span></span>
    * <span data-ttu-id="386bd-135">**Typ** – vyberte **webové aplikace nebo webové rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="386bd-135">**Type** - Select **Web Application and/or Web API**</span></span>
    * <span data-ttu-id="386bd-136">**Adresa URL přihlašování**– hello základní adresu URL pro hello vzorku, který je ve výchozím nastavení`https://hpcserver`</span><span class="sxs-lookup"><span data-stu-id="386bd-136">**Sign-on URL**- hello base URL for hello sample, which is by default `https://hpcserver`</span></span>
    * <span data-ttu-id="386bd-137">**Identifikátor ID URI aplikace** - `https://<Directory_name>/<application_name>`.</span><span class="sxs-lookup"><span data-stu-id="386bd-137">**App ID URI** - `https://<Directory_name>/<application_name>`.</span></span> <span data-ttu-id="386bd-138">Nahraďte `<Directory_name`> s hello celý název klienta služby Azure AD například `hpclocal.onmicrosoft.com`a nahraďte `<application_name>` s názvem hello dříve vybrané.</span><span class="sxs-lookup"><span data-stu-id="386bd-138">Replace `<Directory_name`> with hello full name of your Azure AD tenant, for example, `hpclocal.onmicrosoft.com`, and replace `<application_name>` with hello name you chose previously.</span></span>

5. <span data-ttu-id="386bd-139">Po přidání aplikace hello klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="386bd-139">After hello app is added, click **Configure**.</span></span> <span data-ttu-id="386bd-140">Nakonfigurujte hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="386bd-140">Configure hello following properties:</span></span>
    * <span data-ttu-id="386bd-141">Vyberte **Ano** pro **aplikace je více klientů**</span><span class="sxs-lookup"><span data-stu-id="386bd-141">Select **Yes** for **Application is multi-tenant**</span></span>
    * <span data-ttu-id="386bd-142">Vyberte **Ano** pro **uživatele přiřazení požadované tooaccess aplikace**.</span><span class="sxs-lookup"><span data-stu-id="386bd-142">Select **Yes** for **User assignment required tooaccess app**.</span></span>

6. <span data-ttu-id="386bd-143">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="386bd-143">Click **Save**.</span></span> <span data-ttu-id="386bd-144">Po dokončení ukládání, klikněte na tlačítko **spravovat Manifest**.</span><span class="sxs-lookup"><span data-stu-id="386bd-144">When saving completes, click **Manage Manifest**.</span></span> <span data-ttu-id="386bd-145">Tato akce stáhne vaší aplikace manifestu JavaScript object notation (JSON) souboru.</span><span class="sxs-lookup"><span data-stu-id="386bd-145">This action downloads your application’s manifest JavaScript object notation (JSON) file.</span></span> <span data-ttu-id="386bd-146">Upravit hello stáhnout manifest vyhledáním hello `appRoles` nastavení a přidání hello následující aplikační role:</span><span class="sxs-lookup"><span data-stu-id="386bd-146">Edit hello downloaded manifest by locating hello `appRoles` setting and adding hello following application role:</span></span>
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
7. <span data-ttu-id="386bd-147">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="386bd-147">Save hello file.</span></span> <span data-ttu-id="386bd-148">Hello portálu, klikněte na tlačítko **spravovat Manifest** > **nahrát Manifest**.</span><span class="sxs-lookup"><span data-stu-id="386bd-148">Then in hello portal, click **Manage Manifest** > **Upload Manifest**.</span></span> <span data-ttu-id="386bd-149">Potom můžete nahrát hello upravená manifestu.</span><span class="sxs-lookup"><span data-stu-id="386bd-149">You can then upload hello edited manifest.</span></span>
8. <span data-ttu-id="386bd-150">Klikněte na tlačítko **uživatelé**, vyberte uživatele a pak klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="386bd-150">Click **Users**, select a user, and then click **Assign**.</span></span> <span data-ttu-id="386bd-151">Přiřadíte jeden hello dostupných rolí (HpcUsers nebo HpcAdminMirror) toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="386bd-151">Assign one of hello available roles (HpcUsers or HpcAdminMirror) toohello user.</span></span> <span data-ttu-id="386bd-152">Opakujte tento krok s dalším uživatelům v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="386bd-152">Repeat this step with additional users in hello directory.</span></span> <span data-ttu-id="386bd-153">Základní informace o uživatelích, clusteru, najdete v části [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="386bd-153">For background information about cluster users, see [Managing Cluster Users](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span></span>

   > [!NOTE] 
   > <span data-ttu-id="386bd-154">toomanage uživatele, doporučujeme použít okno preview služby Azure Active Directory hello v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="386bd-154">toomanage users, we recommend using hello Azure Active Directory preview blade in hello [Azure portal](https://portal.azure.com).</span></span>
   >


## <a name="step-2-register-hello-hpc-cluster-client-with-your-azure-ad-tenant"></a><span data-ttu-id="386bd-155">Krok 2: Registrace klienta clusteru HPC hello se klientovi služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="386bd-155">Step 2: Register hello HPC cluster client with your Azure AD tenant</span></span>

1. <span data-ttu-id="386bd-156">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="386bd-156">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="386bd-157">Klikněte na tlačítko **služby Active Directory** v levé nabídce text hello a potom klikněte na hello požadovaného adresáře v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="386bd-157">Click **Active Directory** in hello left menu, and then click hello desired directory in your subscription.</span></span> <span data-ttu-id="386bd-158">V adresáři hello musí mít oprávnění tooaccess prostředky.</span><span class="sxs-lookup"><span data-stu-id="386bd-158">You must have permission tooaccess resources in hello directory.</span></span>
3. <span data-ttu-id="386bd-159">Klikněte na tlačítko **aplikace** > **přidat**a potom klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="386bd-159">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="386bd-160">Zadejte následující informace v Průvodci hello hello:</span><span class="sxs-lookup"><span data-stu-id="386bd-160">Enter hello following information in hello wizard:</span></span>

    * <span data-ttu-id="386bd-161">**Název** -HPCPackClusterClient</span><span class="sxs-lookup"><span data-stu-id="386bd-161">**Name** - HPCPackClusterClient</span></span>
    * <span data-ttu-id="386bd-162">**Typ** – vyberte **nativní klientskou aplikaci**</span><span class="sxs-lookup"><span data-stu-id="386bd-162">**Type** - Select **Native Client Application**</span></span>
    * <span data-ttu-id="386bd-163">**Identifikátor URI pro přesměrování** - `http://hpcclient`</span><span class="sxs-lookup"><span data-stu-id="386bd-163">**Redirect URI** - `http://hpcclient`</span></span>

4. <span data-ttu-id="386bd-164">Po přidání aplikace hello klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="386bd-164">After hello app is added, click **Configure**.</span></span> <span data-ttu-id="386bd-165">Kopírování hello **ID klienta** hodnotu a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="386bd-165">Copy hello **Client ID** value and save it.</span></span> <span data-ttu-id="386bd-166">Budete potřebovat později při konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="386bd-166">You need this later when configuring your application.</span></span>

5. <span data-ttu-id="386bd-167">V **oprávnění tooother aplikace**, klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="386bd-167">In **Permissions tooother applications**, click **Add Application**.</span></span> <span data-ttu-id="386bd-168">Vyhledat a přidat aplikaci HpcPackClusterServer hello (vytvořený v kroku 1).</span><span class="sxs-lookup"><span data-stu-id="386bd-168">Search and add hello  HpcPackClusterServer application (created in Step 1).</span></span>

6. <span data-ttu-id="386bd-169">V hello **delegovaná oprávnění** rozevíracího seznamu vyberte **přístup HpcClusterServer**.</span><span class="sxs-lookup"><span data-stu-id="386bd-169">In hello **Delegated Permissions** dropdown, select **Access HpcClusterServer**.</span></span> <span data-ttu-id="386bd-170">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="386bd-170">Then click **Save**.</span></span>


## <a name="step-3-configure-hello-hpc-cluster"></a><span data-ttu-id="386bd-171">Krok 3: Konfigurace clusteru HPC hello</span><span class="sxs-lookup"><span data-stu-id="386bd-171">Step 3: Configure hello HPC cluster</span></span>

1. <span data-ttu-id="386bd-172">Připojte toohello HPC Pack 2016 hlavního uzlu v Azure.</span><span class="sxs-lookup"><span data-stu-id="386bd-172">Connect toohello HPC Pack 2016 head node in Azure.</span></span>

2. <span data-ttu-id="386bd-173">Spusťte prostředí HPC PowerShell.</span><span class="sxs-lookup"><span data-stu-id="386bd-173">Start HPC PowerShell.</span></span>

3. <span data-ttu-id="386bd-174">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="386bd-174">Run hello following command:</span></span>

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    <span data-ttu-id="386bd-175">kde</span><span class="sxs-lookup"><span data-stu-id="386bd-175">where</span></span>

    * <span data-ttu-id="386bd-176">`AADTenant`Určuje název klienta Azure AD hello, jako například`hpclocal.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="386bd-176">`AADTenant` specifies hello Azure AD tenant name, such as `hpclocal.onmicrosoft.com`</span></span>
    * <span data-ttu-id="386bd-177">`AADClientAppId`Určuje ID hello klienta pro aplikaci hello vytvořili v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="386bd-177">`AADClientAppId` specifies hello client ID for hello app created in Step 2.</span></span>

4. <span data-ttu-id="386bd-178">Restartujte službu HpcSchedulerStateful hello.</span><span class="sxs-lookup"><span data-stu-id="386bd-178">Restart hello HpcSchedulerStateful service.</span></span>

    <span data-ttu-id="386bd-179">V clusteru s více hlavních uzlech můžete spustit následující příkazy prostředí PowerShell na hello hlavního uzlu tooswitch hello primární repliky pro službu HpcSchedulerStateful hello hello:</span><span class="sxs-lookup"><span data-stu-id="386bd-179">In a cluster with multiple head nodes, you can run hello following PowerShell commands on hello head node tooswitch hello primary replica for hello HpcSchedulerStateful service:</span></span>

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-hello-client"></a><span data-ttu-id="386bd-180">Krok 4: Správa a odesílání úloh z klienta hello</span><span class="sxs-lookup"><span data-stu-id="386bd-180">Step 4: Manage and submit jobs from hello client</span></span>

<span data-ttu-id="386bd-181">tooinstall hello HPC Pack klienta nástroje do počítače stáhnout instalační soubory HPC Pack 2016 (úplná instalace) z hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="386bd-181">tooinstall hello HPC Pack client utilities on your computer, download the HPC Pack 2016 setup files (full installation) from hello Microsoft Download Center.</span></span> <span data-ttu-id="386bd-182">Když zahájíte instalaci hello, zvolte možnost instalace hello pro hello **HPC Pack klienta nástroje**.</span><span class="sxs-lookup"><span data-stu-id="386bd-182">When you begin hello installation, choose hello setup option for hello **HPC Pack client utilities**.</span></span>

<span data-ttu-id="386bd-183">tooprepare hello klientský počítač, nainstalujte certifikát hello používá během [nastavení clusteru HPC](hpcpack-2016-cluster.md) na klientském počítači hello.</span><span class="sxs-lookup"><span data-stu-id="386bd-183">tooprepare hello client computer, install hello certificate used during [HPC cluster setup](hpcpack-2016-cluster.md) on hello client computer.</span></span> <span data-ttu-id="386bd-184">Standardní Windows pomocí certifikátu správy postupy tooinstall hello veřejný certifikát toohello **certifikáty – aktuální uživatel** > **důvěryhodné kořenové certifikační autority** uložit.</span><span class="sxs-lookup"><span data-stu-id="386bd-184">Use standard Windows certificate management procedures tooinstall hello public certificate toohello **Certificates – Current user** > **Trusted Root Certification Authorities** store.</span></span> 

<span data-ttu-id="386bd-185">Teď můžete spustit příkazy hello HPC Pack nebo použít hello grafického uživatelského rozhraní Správce úloh HPC Pack toosubmit a spravovat úlohy clusteru pomocí účtu hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="386bd-185">You can now run hello HPC Pack commands or use hello HPC Pack Job manager GUI toosubmit and manage cluster jobs by using hello Azure AD account.</span></span> <span data-ttu-id="386bd-186">Možnosti pro odeslání úlohy najdete v tématu [tooan HPC odeslání úlohy HPC Pack clusteru v Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="386bd-186">For job submission options, see [Submit HPC jobs tooan HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span></span>

> [!NOTE]
> <span data-ttu-id="386bd-187">Když zkusíte tooconnect clusteru toohello HPC Pack v Azure pro hello poprvé, zobrazí se místní windows.</span><span class="sxs-lookup"><span data-stu-id="386bd-187">When you try tooconnect toohello HPC Pack cluster in Azure for hello first time, a popup windows appears.</span></span> <span data-ttu-id="386bd-188">Zadejte vaše přihlašovací údaje toolog Azure AD v.</span><span class="sxs-lookup"><span data-stu-id="386bd-188">Enter your Azure AD credentials toolog in.</span></span> <span data-ttu-id="386bd-189">Hello token se pak uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="386bd-189">hello token is then cached.</span></span> <span data-ttu-id="386bd-190">Novější clusteru toohello připojení v Azure pomocí hello do mezipaměti token, pokud není zaškrtnuté změny ověřování nebo hello do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="386bd-190">Later connections toohello cluster in Azure use hello cached token unless authentication changes or hello cached is cleared.</span></span>
>
  
<span data-ttu-id="386bd-191">Například po dokončení předchozích kroků hello se můžete dotazovat pro úlohy z lokálního klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="386bd-191">For example, after completing hello previous steps, you can query for jobs from an on-premises client as follows:</span></span>

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a><span data-ttu-id="386bd-192">Užitečné rutiny pro odeslání úlohy s integrací služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="386bd-192">Useful cmdlets for job submission with Azure AD integration</span></span> 

### <a name="manage-hello-local-token-cache"></a><span data-ttu-id="386bd-193">Spravovat místní mezipaměti tokenu hello</span><span class="sxs-lookup"><span data-stu-id="386bd-193">Manage hello local token cache</span></span>

<span data-ttu-id="386bd-194">HPC Pack 2016 poskytuje dvě nové rutiny prostředí HPC PowerShell toomanage hello místní tokenu mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="386bd-194">HPC Pack 2016 provides two new HPC PowerShell cmdlets toomanage hello local token cache.</span></span> <span data-ttu-id="386bd-195">Tyto rutiny jsou užitečné pro odesílání úloh interaktivně.</span><span class="sxs-lookup"><span data-stu-id="386bd-195">These cmdlets are useful for submitting jobs non-interactively.</span></span> <span data-ttu-id="386bd-196">Viz následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="386bd-196">See hello following example:</span></span>

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-hello-credentials-for-submitting-jobs-using-hello-azure-ad-account"></a><span data-ttu-id="386bd-197">Nastavte hello přihlašovací údaje pro odesílání úloh, pomocí účtu hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="386bd-197">Set hello credentials for submitting jobs using hello Azure AD account</span></span> 

<span data-ttu-id="386bd-198">V některých případech můžete chtít úlohy hello toorun pod uživatelským clusteru HPC hello (pro připojené k doméně clusteru HPC, spustit jako uživatele domény jeden; pro domény nepřipojená clusteru HPC, spustit jako jeden místní uživatel hello hlavního uzlu).</span><span class="sxs-lookup"><span data-stu-id="386bd-198">Sometimes, you may want toorun hello job under hello HPC cluster user (for a domain-joined HPC cluster, run as one domain user; for a non-domain-joined HPC cluster, run as one local user on hello head node).</span></span>

1. <span data-ttu-id="386bd-199">Použijte následující příkazy tooset hello pověření hello:</span><span class="sxs-lookup"><span data-stu-id="386bd-199">Use hello following commands tooset hello credentials:</span></span>

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. <span data-ttu-id="386bd-200">Pak odešlete úlohu hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="386bd-200">Then submit hello job as follows.</span></span> <span data-ttu-id="386bd-201">Hello úkol se spustí v části $localUser hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="386bd-201">hello job/task runs under $localUser on hello compute nodes.</span></span>

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   <span data-ttu-id="386bd-202">Pokud `–Credential` není zadaný s `Submit-HpcJob`, hello úlohy nebo úkolu spouští pod místním uživatelským namapované jako hello účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="386bd-202">If `–Credential` is not specified with `Submit-HpcJob`, hello job or task runs under a local mapped user as hello Azure AD account.</span></span> <span data-ttu-id="386bd-203">(clusteru HPC hello vytvoří místního uživatele s hello stejný název jako hello úloh hello toorun účet Azure AD.)</span><span class="sxs-lookup"><span data-stu-id="386bd-203">(hello HPC cluster creates a local user with hello same name as hello Azure AD account toorun hello task.)</span></span>
    
3. <span data-ttu-id="386bd-204">Nastavit rozšířená data pro hello účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="386bd-204">Set extended data for hello Azure AD account.</span></span> <span data-ttu-id="386bd-205">To je užitečné, pokud se používá úlohu MPI na Linuxových uzlů pomocí účtu hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="386bd-205">This is useful when running an MPI job on Linux nodes using hello Azure AD account.</span></span>

   * <span data-ttu-id="386bd-206">Nastavit rozšířená data pro hello přímo účet Azure AD</span><span class="sxs-lookup"><span data-stu-id="386bd-206">Set extended data for hello Azure AD account itself</span></span>

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * <span data-ttu-id="386bd-207">Nastavte Rozšířená data a spusťte jako uživatel clusteru HPC</span><span class="sxs-lookup"><span data-stu-id="386bd-207">Set extended data and run as HPC cluster user</span></span>
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

