---
title: "Nasazení rozšíření Panel přístupu Azure pro aplikaci Internet Explorer pomocí objektu zásad skupiny | Microsoft Docs"
description: "Postup nasazení aplikace Internet Explorer rozšíření pro portál Moje aplikace pomocí zásad skupiny."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b402ae326ab34ec71ad9de966e22be00045fee3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a><span data-ttu-id="76d3e-103">Postup nasazení rozšíření Panel přístupu pro Internet Explorer pomocí zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="76d3e-103">How to Deploy the Access Panel Extension for Internet Explorer using Group Policy</span></span>
<span data-ttu-id="76d3e-104">Tento kurz ukazuje, jak provést vzdálenou instalaci rozšíření Panel přístupu pro Internet Explorer na počítačích uživatelů pomocí zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="76d3e-104">This tutorial shows how to use group policy to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span> <span data-ttu-id="76d3e-105">Toto rozšíření je pro Internet Explorer uživatelů, kteří potřebují pro přihlášení do aplikace, které jsou nakonfigurované pomocí [založené na heslech jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="76d3e-105">This extension is required for Internet Explorer users who need to sign into apps that are configured using [password-based single sign-on](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span></span>

<span data-ttu-id="76d3e-106">Doporučujeme, aby správci automatizovat nasazení tohoto rozšíření.</span><span class="sxs-lookup"><span data-stu-id="76d3e-106">It is recommended that admins automate the deployment of this extension.</span></span> <span data-ttu-id="76d3e-107">Uživatelé, jinak hodnota mít ke stažení a instalaci rozšíření sami, který je pravděpodobnost chyby uživatele a musí mít oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="76d3e-107">Otherwise, users have to download and install the extension themselves, which is prone to user error and requires administrator permissions.</span></span> <span data-ttu-id="76d3e-108">Tento kurz se zaměřuje na jednu z metod automatizaci nasazení softwaru pomocí zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="76d3e-108">This tutorial covers one method of automating software deployments by using group policy.</span></span> [<span data-ttu-id="76d3e-109">Další informace o zásadách skupiny.</span><span class="sxs-lookup"><span data-stu-id="76d3e-109">Learn more about group policy.</span></span>](https://technet.microsoft.com/windowsserver/bb310732.aspx)

<span data-ttu-id="76d3e-110">Rozšíření přístupový Panel je také k dispozici pro [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) a [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), ani jedno, které vyžadují oprávnění správce pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="76d3e-110">The Access Panel extension is also available for [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) and [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), neither of which require administrator permissions to install.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76d3e-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76d3e-111">Prerequisites</span></span>
* <span data-ttu-id="76d3e-112">Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a vaši uživatelé počítačů se připojili k vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="76d3e-112">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>
* <span data-ttu-id="76d3e-113">Musíte mít oprávnění "Upravit nastavení" k úpravě objektu zásad skupiny (GPO).</span><span class="sxs-lookup"><span data-stu-id="76d3e-113">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="76d3e-114">Ve výchozím nastavení, toto oprávnění mají členy z těchto skupin zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners.</span><span class="sxs-lookup"><span data-stu-id="76d3e-114">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> [<span data-ttu-id="76d3e-115">Další informace</span><span class="sxs-lookup"><span data-stu-id="76d3e-115">Learn more.</span></span>](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a><span data-ttu-id="76d3e-116">Krok 1: Vytvoření distribučního bodu</span><span class="sxs-lookup"><span data-stu-id="76d3e-116">Step 1: Create the Distribution Point</span></span>
<span data-ttu-id="76d3e-117">Balíček Instalační služby musíte nejprve umístit na umístění v síti, která je přístupná na počítače, které chcete vzdáleně nainstalovat rozšíření na.</span><span class="sxs-lookup"><span data-stu-id="76d3e-117">First, you must place the installer package on a network location that can be accessed by the machines that you wish to remotely install the extension on.</span></span> <span data-ttu-id="76d3e-118">Chcete-li to provést, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="76d3e-118">To do this, follow these steps:</span></span>

1. <span data-ttu-id="76d3e-119">Přihlaste se k serveru jako správce</span><span class="sxs-lookup"><span data-stu-id="76d3e-119">Log on to the server as an administrator</span></span>
2. <span data-ttu-id="76d3e-120">V **správce serveru** okno, přejděte na **soubory a služby úložiště**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-120">In the **Server Manager** window, go to **Files and Storage Services**.</span></span>
   
    ![Otevřené soubory a služby úložiště](./media/active-directory-saas-ie-group-policy/files-services.png)
3. <span data-ttu-id="76d3e-122">Přejděte na **sdílené složky** kartě. Pak klikněte na tlačítko **úlohy** > **Nová sdílená složka...**</span><span class="sxs-lookup"><span data-stu-id="76d3e-122">Go to the **Shares** tab. Then click **Tasks** > **New Share...**</span></span>
   
    ![Otevřené soubory a služby úložiště](./media/active-directory-saas-ie-group-policy/shares.png)
4. <span data-ttu-id="76d3e-124">Dokončení **Průvodce vytvořením nové sdílené složky** a nastavit oprávnění a ujistěte se, že byla přístupná z vašich uživatelů počítačů.</span><span class="sxs-lookup"><span data-stu-id="76d3e-124">Complete the **New Share Wizard** and set permissions to ensure that it can be accessed from your users' machines.</span></span> [<span data-ttu-id="76d3e-125">Další informace o sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="76d3e-125">Learn more about shares.</span></span>](https://technet.microsoft.com/library/cc753175.aspx)
5. <span data-ttu-id="76d3e-126">Stáhněte si následující balíček Instalační služby systému Windows (soubor MSI): [Extension.msi Panel přístupu](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span><span class="sxs-lookup"><span data-stu-id="76d3e-126">Download the following Microsoft Windows Installer package (.msi file): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span></span>
6. <span data-ttu-id="76d3e-127">Zkopírujte instalační balíček do požadovaného umístění na sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="76d3e-127">Copy the installer package to a desired location on the share.</span></span>
   
    ![Zkopírujte soubor .msi ke sdílené složce.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. <span data-ttu-id="76d3e-129">Ověřte, zda jsou klientské počítače moct přistupovat ze sdílené složky je balíček Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="76d3e-129">Verify that your client machines are able to access the installer package from the share.</span></span> 

## <a name="step-2-create-the-group-policy-object"></a><span data-ttu-id="76d3e-130">Krok 2: Vytvoření objektu zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="76d3e-130">Step 2: Create the Group Policy Object</span></span>
1. <span data-ttu-id="76d3e-131">Přihlaste se k serveru, který je hostitelem vaší instalace služby Active Directory Domain Services (AD DS).</span><span class="sxs-lookup"><span data-stu-id="76d3e-131">Log on to the server that hosts your Active Directory Domain Services (AD DS) installation.</span></span>
2. <span data-ttu-id="76d3e-132">Ve Správci serveru přejděte na **nástroje** > **Správa zásad skupiny**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-132">In the Server Manager, go to **Tools** > **Group Policy Management**.</span></span>
   
    ![Přejděte na Nástroje > Managment zásad skupiny](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. <span data-ttu-id="76d3e-134">V levém podokně **Správa zásad skupiny** okně zobrazení hierarchie organizační jednotky (OU) a zjistit, v oboru, který chcete použít zásady skupiny.</span><span class="sxs-lookup"><span data-stu-id="76d3e-134">In the left pane of the **Group Policy Management** window, view your Organizational Unit (OU) hierarchy and determine at which scope you would like to apply the group policy.</span></span> <span data-ttu-id="76d3e-135">Například můžete rozhodnout pro vyberte malé organizační jednotky k nasazení na několik uživatelů pro testování, nebo vyberete nejvyšší úrovně organizační jednotky k nasazení do celé organizace.</span><span class="sxs-lookup"><span data-stu-id="76d3e-135">For instance, you may decide to pick a small OU to deploy to a few users for testing, or you may pick a top-level OU to deploy to your entire organization.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="76d3e-136">Pokud chcete vytvořit nebo upravit organizačních jednotkách (OU), přepněte zpět do správce serveru a přejděte k **nástroje** > **Active Directory Users and Computers**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-136">If you would like to create or edit your Organization Units (OUs), switch back to the Server Manager and go to **Tools** > **Active Directory Users and Computers**.</span></span>
   > 
   > 
4. <span data-ttu-id="76d3e-137">Jakmile vyberete organizační jednotku, pravým tlačítkem myši a vyberte **vytvořit objekt zásad skupiny v této doméně a propojit jej sem...**</span><span class="sxs-lookup"><span data-stu-id="76d3e-137">Once you have selected an OU, right-click it and select **Create a GPO in this domain, and Link it here...**</span></span>
   
    ![Vytvořit nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. <span data-ttu-id="76d3e-139">V **nový objekt zásad skupiny** řádku, zadejte název pro nový objekt zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="76d3e-139">In the **New GPO** prompt, type in a name for the new Group Policy Object.</span></span>
   
    ![Název nového objektu zásad skupiny](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. <span data-ttu-id="76d3e-141">Klikněte pravým tlačítkem na objekt zásad skupiny, který jste vytvořili a vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-141">Right-click the Group Policy Object that you created, and select **Edit**.</span></span>
   
    ![Upravit nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-the-installation-package"></a><span data-ttu-id="76d3e-143">Krok 3: Přiřaďte instalačního balíčku</span><span class="sxs-lookup"><span data-stu-id="76d3e-143">Step 3: Assign the Installation Package</span></span>
1. <span data-ttu-id="76d3e-144">Určete, jestli chcete nasadit rozšíření na základě **konfigurace počítače** nebo **konfigurace uživatele**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-144">Determine whether you would like to deploy the extension based on **Computer Configuration** or **User Configuration**.</span></span> <span data-ttu-id="76d3e-145">Při použití [konfigurace počítače](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), rozšíření je nainstalován v počítači bez ohledu na to, které je uživatelům přihlášení.</span><span class="sxs-lookup"><span data-stu-id="76d3e-145">When using [computer configuration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), the extension is installed on the computer regardless of which users log on to it.</span></span> <span data-ttu-id="76d3e-146">S [konfigurace uživatele](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), uživatelé mají rozšíření nainstalovat pro ně bez ohledu na počítače, které se přihlašují.</span><span class="sxs-lookup"><span data-stu-id="76d3e-146">With [user configuration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), users have the extension installed for them regardless of which computers they log on to.</span></span>
2. <span data-ttu-id="76d3e-147">V levém podokně **Editor správy zásad skupiny** okno, přejděte na jednu z následujících cestách složku, v závislosti na tom, jaký typ konfigurace jste zvolili:</span><span class="sxs-lookup"><span data-stu-id="76d3e-147">In the left pane of the **Group Policy Management Editor** window, go to either of the following folder paths, depending on which type of configuration you chose:</span></span>
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. <span data-ttu-id="76d3e-148">Klikněte pravým tlačítkem na **instalace softwaru**, pak vyberte **nový** > **balíčku...**</span><span class="sxs-lookup"><span data-stu-id="76d3e-148">Right-click **Software installation**, then select **New** > **Package...**</span></span>
   
    ![Vytvořit nový balíček pro instalaci softwaru](./media/active-directory-saas-ie-group-policy/new-package.png)
4. <span data-ttu-id="76d3e-150">Přejděte do sdílené složky, která obsahuje balíček Instalační služby z [krok 1: Vytvoření distribučního bodu](#step-1-create-the-distribution-point), vyberte soubor MSI a klikněte na **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-150">Go to the shared folder that contains the installer package from [Step 1: Create the Distribution Point](#step-1-create-the-distribution-point), select the .msi file, and click **Open**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="76d3e-151">Pokud je na tomto serveru stejné sdílené složky, ověřte, že přístupu k souboru MSI prostřednictvím sítě cesta k souboru, nikoli cestu k místnímu souboru.</span><span class="sxs-lookup"><span data-stu-id="76d3e-151">If the share is located on this same server, verify that you are accessing the .msi through the network file path, rather than the local file path.</span></span>
   > 
   > 
   
    ![Vyberte instalační balíček ze sdílené složky.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. <span data-ttu-id="76d3e-153">V **nasazení softwaru** výzvy, vyberte **přiřazeno** pro metodu nasazení.</span><span class="sxs-lookup"><span data-stu-id="76d3e-153">In the **Deploy Software** prompt, select **Assigned** for your deployment method.</span></span> <span data-ttu-id="76d3e-154">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-154">Then click **OK**.</span></span>
   
    ![Vyberte přiřazeno a potom klikněte na tlačítko OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

<span data-ttu-id="76d3e-156">Rozšíření je nyní nasadit na organizační jednotku, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="76d3e-156">The extension is now deployed to the OU that you selected.</span></span> [<span data-ttu-id="76d3e-157">Další informace o instalace softwaru zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="76d3e-157">Learn more about Group Policy Software Installation.</span></span>](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a><span data-ttu-id="76d3e-158">Krok 4: Automatické povolení rozšíření pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="76d3e-158">Step 4: Auto-Enable the Extension for Internet Explorer</span></span>
<span data-ttu-id="76d3e-159">Kromě spuštění Instalační program, každou příponu pro Internet Explorer musí být explicitně povoleno před použitím.</span><span class="sxs-lookup"><span data-stu-id="76d3e-159">In addition to running the installer, every extension for Internet Explorer must be explicitly enabled before it can be used.</span></span> <span data-ttu-id="76d3e-160">Použijte následující postup k povolení rozšíření Panel přístupu pomocí zásad skupiny:</span><span class="sxs-lookup"><span data-stu-id="76d3e-160">Follow the steps below to enable the Access Panel Extension using group policy:</span></span>

1. <span data-ttu-id="76d3e-161">V **Editor správy zásad skupiny** okno, přejděte na jednu z následujících cestách, v závislosti na tom, jaký typ konfigurace, který jste zvolili v [krok 3: přiřaďte instalační balíček](#step-3-assign-the-installation-package):</span><span class="sxs-lookup"><span data-stu-id="76d3e-161">In the **Group Policy Management Editor** window, go to either of the following paths, depending on which type of configuration you chose in [Step 3: Assign the Installation Package](#step-3-assign-the-installation-package):</span></span>
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. <span data-ttu-id="76d3e-162">Klikněte pravým tlačítkem na **seznam doplňků**a vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-162">Right-click **Add-on List**, and select **Edit**.</span></span>
    <span data-ttu-id="76d3e-163">![Seznam rozšíření upravte.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span><span class="sxs-lookup"><span data-stu-id="76d3e-163">![Edit Add-on List.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span></span>
3. <span data-ttu-id="76d3e-164">V **seznam doplňků** vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-164">In the **Add-on List** window, select **Enabled**.</span></span> <span data-ttu-id="76d3e-165">Potom v části **možnosti** klikněte na tlačítko **zobrazit...** .</span><span class="sxs-lookup"><span data-stu-id="76d3e-165">Then, under the **Options** section, click **Show...**.</span></span>
   
    ![Klikněte na položku Povolit a pak klikněte na zobrazit...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. <span data-ttu-id="76d3e-167">V **zobrazit obsah** okna, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="76d3e-167">In the **Show Contents** window, perform the following steps:</span></span>
   
   1. <span data-ttu-id="76d3e-168">První sloupec ( **název hodnoty** pole), zkopírujte a vložte následující ID třídy:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span><span class="sxs-lookup"><span data-stu-id="76d3e-168">For the first column (the **Value Name** field), copy and paste the following Class ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span></span>
   2. <span data-ttu-id="76d3e-169">Pro druhý sloupec ( **hodnotu** pole), zadejte následující hodnotu:`1`</span><span class="sxs-lookup"><span data-stu-id="76d3e-169">For the second column (the **Value** field), type in the following value: `1`</span></span>
   3. <span data-ttu-id="76d3e-170">Klikněte na tlačítko **OK** zavřete **zobrazit obsah** okna.</span><span class="sxs-lookup"><span data-stu-id="76d3e-170">Click **OK** to close the **Show Contents** window.</span></span>
      
      ![Vyplňte hodnoty, jak je uvedeno výše.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. <span data-ttu-id="76d3e-172">Klikněte na tlačítko **OK** použít změny a zavřete **seznam doplňků** okno.</span><span class="sxs-lookup"><span data-stu-id="76d3e-172">Click **OK** to apply your changes and close the **Add-on List** window.</span></span>

<span data-ttu-id="76d3e-173">Rozšíření by měl nyní povolena pro počítače ve vybrané organizační jednotce.</span><span class="sxs-lookup"><span data-stu-id="76d3e-173">The extension should now be enabled for the machines in the selected OU.</span></span> [<span data-ttu-id="76d3e-174">Další informace o použití zásad skupiny k povolení nebo zakázání doplňky aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="76d3e-174">Learn more about using group policy to enable or disable Internet Explorer add-ons.</span></span>](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a><span data-ttu-id="76d3e-175">Krok 5 (volitelné): zakázat "Zapamatovat heslo" řádku</span><span class="sxs-lookup"><span data-stu-id="76d3e-175">Step 5 (Optional): Disable "Remember Password" Prompt</span></span>
<span data-ttu-id="76d3e-176">Když uživatelé přihlášení k weby s využitím rozšíření přístup k panelu, Internet Explorer může zobrazit následující výzva s dotazem, "Chcete uložit heslo?"</span><span class="sxs-lookup"><span data-stu-id="76d3e-176">When users sign-in to websites using the Access Panel Extension, Internet Explorer may show the following prompt asking "Would you like to store your password?"</span></span>

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

<span data-ttu-id="76d3e-177">Pokud chcete zabránit uživatelům v zobrazení tuto výzvu, postupujte podle následujících kroků a zabránit automatické dokončování z zapamatování hesla:</span><span class="sxs-lookup"><span data-stu-id="76d3e-177">If you wish to prevent your users from seeing this prompt, then follow the steps below to prevent auto-complete from remembering passwords:</span></span>

1. <span data-ttu-id="76d3e-178">V **Editor správy zásad skupiny** okno, přejděte k cestě uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="76d3e-178">In the **Group Policy Management Editor** window, go to the path listed below.</span></span> <span data-ttu-id="76d3e-179">Toto nastavení konfigurace je k dispozici v části pouze **konfigurace uživatele**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-179">This configuration setting is only available under **User Configuration**.</span></span>
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. <span data-ttu-id="76d3e-180">Najít nastavení s názvem **zapnout funkci automatického dokončování pro uživatelská jména a hesla ve formulářích**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-180">Find the setting named **Turn on the auto-complete feature for user names and passwords on forms**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="76d3e-181">Předchozí verze služby Active Directory může zobrazit seznam toto nastavení s názvem **zakázat automatické dokončování pro ukládání hesel**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-181">Previous versions of Active Directory may list this setting with the name **Do not allow auto-complete to save passwords**.</span></span> <span data-ttu-id="76d3e-182">Konfigurace tohoto nastavení se liší od nastavení popsané v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="76d3e-182">The configuration for that setting differs from the setting described in this tutorial.</span></span>
   > 
   > 
   
    ![Mějte na paměti, a to Hledat v části Nastavení uživatele.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. <span data-ttu-id="76d3e-184">Klikněte pravým tlačítkem na výše uvedené nastavení a vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-184">Right click the above setting, and select **Edit**.</span></span>
4. <span data-ttu-id="76d3e-185">V okně s názvem **zapnout funkci automatického dokončování pro uživatelská jména a hesla ve formulářích**, vyberte **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-185">In the window titled **Turn on the auto-complete feature for user names and passwords on forms**, select **Disabled**.</span></span>
   
    ![Vyberte zakázat](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. <span data-ttu-id="76d3e-187">Klikněte na tlačítko **OK** tyto změny a zavřete toto okno.</span><span class="sxs-lookup"><span data-stu-id="76d3e-187">Click **OK** to apply these changes and close the window.</span></span>

<span data-ttu-id="76d3e-188">Uživatelé už nebude moct ukládání jejich přihlašovacích údajů nebo jejich použití automatického dokončování pro přístup k dříve uložené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="76d3e-188">Users will no longer be able to store their credentials or use auto-complete to access previously stored credentials.</span></span> <span data-ttu-id="76d3e-189">Ale tyto zásady umožňují uživatelům nadále používat automatické dokončování pro jiné typy polí formuláře, například pole hledání.</span><span class="sxs-lookup"><span data-stu-id="76d3e-189">However, this policy does allow users to continue to use auto-complete for other types of form fields, such as search fields.</span></span>

> [!WARNING]
> <span data-ttu-id="76d3e-190">Pokud je tato zásada povolena po uživatelé jste se rozhodli uložit některé přihlašovací údaje, zásady budou *není* vymazat přihlašovací údaje, které již byly uloženy.</span><span class="sxs-lookup"><span data-stu-id="76d3e-190">If this policy is enabled after users have chosen to store some credentials, this policy will *not* clear the credentials that have already been stored.</span></span>
> 
> 

## <a name="step-6-testing-the-deployment"></a><span data-ttu-id="76d3e-191">Krok 6: Testování nasazení</span><span class="sxs-lookup"><span data-stu-id="76d3e-191">Step 6: Testing the Deployment</span></span>
<span data-ttu-id="76d3e-192">Použijte následující postup ověření, pokud rozšíření nasazení bylo úspěšné:</span><span class="sxs-lookup"><span data-stu-id="76d3e-192">Follow the steps below to verify if the extension deployment was successful:</span></span>

1. <span data-ttu-id="76d3e-193">Pokud jste nasadili pomocí **konfigurace počítače**, přihlaste se klientský počítač, který patří do organizační jednotky, kterou jste vybrali v [krok 2: vytvoření objektu zásad skupiny](#step-2-create-the-group-policy-object).</span><span class="sxs-lookup"><span data-stu-id="76d3e-193">If you deployed using **Computer Configuration**, sign into a client machine that belongs to the OU that you selected in [Step 2: Create the Group Policy Object](#step-2-create-the-group-policy-object).</span></span> <span data-ttu-id="76d3e-194">Pokud jste nasadili pomocí **konfigurace uživatele**, zajistěte, aby se přihlásit jako uživatel, který patří do dané organizační jednotky.</span><span class="sxs-lookup"><span data-stu-id="76d3e-194">If you deployed using **User Configuration**, make sure to sign in as a user who belongs to that OU.</span></span>
2. <span data-ttu-id="76d3e-195">To může trvat několik sign in pro zásady skupiny se změní na plně aktualizovat pomocí tohoto počítače.</span><span class="sxs-lookup"><span data-stu-id="76d3e-195">It may take a couple sign ins for the group policy changes to fully update with this machine.</span></span> <span data-ttu-id="76d3e-196">Chcete-li vynutit aktualizaci, otevřete **příkazového řádku** okno a spusťte následující příkaz:`gpupdate /force`</span><span class="sxs-lookup"><span data-stu-id="76d3e-196">To force the update, open a **Command Prompt** window and run the following command: `gpupdate /force`</span></span>
3. <span data-ttu-id="76d3e-197">Je nutné restartovat počítač pro instalaci proběhla.</span><span class="sxs-lookup"><span data-stu-id="76d3e-197">You must restart the machine for the installation to take place.</span></span> <span data-ttu-id="76d3e-198">Spuštění může trvat výrazně déle než obvykle při rozšíření nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="76d3e-198">Bootup may take significantly more time than usual while the extension installs.</span></span>
4. <span data-ttu-id="76d3e-199">Po restartování počítače, otevřete **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-199">After restarting, open **Internet Explorer**.</span></span> <span data-ttu-id="76d3e-200">V pravém horním rohu okna, klikněte na tlačítko **nástroje** (ozubené kolečko ikonu) a potom vyberte **spravovat doplňky**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-200">On the upper-right corner of the window, click **Tools** (the gear icon), and then select **Manage add-ons**.</span></span>
   
    ![Přejděte na Nástroje > Správa doplňků](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. <span data-ttu-id="76d3e-202">V **spravovat doplňky** okno, ověřte, zda **rozšíření přístup k panelu** byla nainstalována a že jeho **stav** byla nastavena na **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="76d3e-202">In the **Manage Add-ons** window, verify that the **Access Panel Extension** has been installed and that its **Status** has been set to **Enabled**.</span></span>
   
    ![Ověřte, zda rozšíření přístup k panelu je nainstalován a povolen.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a><span data-ttu-id="76d3e-204">Související články</span><span class="sxs-lookup"><span data-stu-id="76d3e-204">Related Articles</span></span>
* [<span data-ttu-id="76d3e-205">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="76d3e-205">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="76d3e-206">Přístup k aplikaci a jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="76d3e-206">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="76d3e-207">Řešení potíží s příponou Panel přístupu pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="76d3e-207">Troubleshooting the Access Panel Extension for Internet Explorer</span></span>](active-directory-saas-ie-troubleshooting.md)

