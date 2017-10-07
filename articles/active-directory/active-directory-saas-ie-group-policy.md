---
title: "aaaDeploy Azure přístup k panelu rozšíření pro aplikaci Internet Explorer pomocí objektu zásad skupiny | Microsoft Docs"
description: "Jak toouse skupiny zásad toodeploy hello Internet Explorer rozšíření pro portál Moje aplikace hello."
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
ms.openlocfilehash: 926f15950bbe81d2fbfe1b0b856470da40880d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-hello-access-panel-extension-for-internet-explorer-using-group-policy"></a><span data-ttu-id="f2ea9-103">Jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="f2ea9-103">How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy</span></span>
<span data-ttu-id="f2ea9-104">Tento kurz ukazuje, jak tooremotely zásad skupiny toouse instalovat hello přístupový Panel rozšíření pro Internet Explorer na počítačích uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-104">This tutorial shows how toouse group policy tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span> <span data-ttu-id="f2ea9-105">Toto rozšíření je pro Internet Explorer uživatelů, kteří potřebují toosign do aplikace, které jsou nakonfigurované pomocí [založené na heslech jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="f2ea9-105">This extension is required for Internet Explorer users who need toosign into apps that are configured using [password-based single sign-on](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span></span>

<span data-ttu-id="f2ea9-106">Doporučuje se, že správci automatizovat nasazení hello tohoto rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-106">It is recommended that admins automate hello deployment of this extension.</span></span> <span data-ttu-id="f2ea9-107">Jinak uživatelé mají toodownload a nainstalovat hello rozšíření, která je chyba náchylné k chybám toouser a musí mít oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-107">Otherwise, users have toodownload and install hello extension themselves, which is prone toouser error and requires administrator permissions.</span></span> <span data-ttu-id="f2ea9-108">Tento kurz se zaměřuje na jednu z metod automatizaci nasazení softwaru pomocí zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-108">This tutorial covers one method of automating software deployments by using group policy.</span></span> [<span data-ttu-id="f2ea9-109">Další informace o zásadách skupiny.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-109">Learn more about group policy.</span></span>](https://technet.microsoft.com/windowsserver/bb310732.aspx)

<span data-ttu-id="f2ea9-110">Hello rozšíření přístupový Panel je také k dispozici pro [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) a [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), ani jedno, které vyžadují tooinstall oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-110">hello Access Panel extension is also available for [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) and [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), neither of which require administrator permissions tooinstall.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2ea9-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f2ea9-111">Prerequisites</span></span>
* <span data-ttu-id="f2ea9-112">Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a připojení uživatelů počítačů tooyour domény.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-112">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>
* <span data-ttu-id="f2ea9-113">Musíte mít hello "Upravit nastavení" oprávnění tooedit hello objekt zásad skupiny (GPO).</span><span class="sxs-lookup"><span data-stu-id="f2ea9-113">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="f2ea9-114">Ve výchozím nastavení, toto oprávnění mají členové hello následující skupiny zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-114">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> [<span data-ttu-id="f2ea9-115">Další informace</span><span class="sxs-lookup"><span data-stu-id="f2ea9-115">Learn more.</span></span>](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-hello-distribution-point"></a><span data-ttu-id="f2ea9-116">Krok 1: Vytvoření hello distribučního bodu</span><span class="sxs-lookup"><span data-stu-id="f2ea9-116">Step 1: Create hello Distribution Point</span></span>
<span data-ttu-id="f2ea9-117">Nejprve musíte umístit hello instalační balíček na síťové umístění, které jsou přístupné hello počítače, u nějž chcete tooremotely instalace hello rozšíření na.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-117">First, you must place hello installer package on a network location that can be accessed by hello machines that you wish tooremotely install hello extension on.</span></span> <span data-ttu-id="f2ea9-118">toodo, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f2ea9-118">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="f2ea9-119">Přihlaste se jako správce serveru toohello</span><span class="sxs-lookup"><span data-stu-id="f2ea9-119">Log on toohello server as an administrator</span></span>
2. <span data-ttu-id="f2ea9-120">V hello **správce serveru** okně přejděte příliš**soubory a služby úložiště**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-120">In hello **Server Manager** window, go too**Files and Storage Services**.</span></span>
   
    ![Otevřené soubory a služby úložiště](./media/active-directory-saas-ie-group-policy/files-services.png)
3. <span data-ttu-id="f2ea9-122">Přejděte toohello **sdílené složky** kartě. Pak klikněte na tlačítko **úlohy** > **Nová sdílená složka...**</span><span class="sxs-lookup"><span data-stu-id="f2ea9-122">Go toohello **Shares** tab. Then click **Tasks** > **New Share...**</span></span>
   
    ![Otevřené soubory a služby úložiště](./media/active-directory-saas-ie-group-policy/shares.png)
4. <span data-ttu-id="f2ea9-124">Dokončení hello **Průvodce vytvořením nové sdílené složky** a tooensure sadu oprávnění, aby byla přístupná z vašich uživatelů počítačů.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-124">Complete hello **New Share Wizard** and set permissions tooensure that it can be accessed from your users' machines.</span></span> [<span data-ttu-id="f2ea9-125">Další informace o sdílených složek.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-125">Learn more about shares.</span></span>](https://technet.microsoft.com/library/cc753175.aspx)
5. <span data-ttu-id="f2ea9-126">Stáhněte si následující balíček Instalační služby systému Windows (soubor .msi) hello: [Extension.msi Panel přístupu](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span><span class="sxs-lookup"><span data-stu-id="f2ea9-126">Download hello following Microsoft Windows Installer package (.msi file): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span></span>
6. <span data-ttu-id="f2ea9-127">Zkopírujte hello instalační program balíčku tooa požadovaného umístění na sdílené složky hello.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-127">Copy hello installer package tooa desired location on hello share.</span></span>
   
    ![Zkopírujte sdílenou toohello hello .msi.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. <span data-ttu-id="f2ea9-129">Ověřte, zda klientské počítače jsou možné tooaccess hello instalační balíček ze sdílené složky hello.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-129">Verify that your client machines are able tooaccess hello installer package from hello share.</span></span> 

## <a name="step-2-create-hello-group-policy-object"></a><span data-ttu-id="f2ea9-130">Krok 2: Vytvoření hello objektu zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="f2ea9-130">Step 2: Create hello Group Policy Object</span></span>
1. <span data-ttu-id="f2ea9-131">Přihlaste se na toohello serveru, který je hostitelem vaší instalace služby Active Directory Domain Services (AD DS).</span><span class="sxs-lookup"><span data-stu-id="f2ea9-131">Log on toohello server that hosts your Active Directory Domain Services (AD DS) installation.</span></span>
2. <span data-ttu-id="f2ea9-132">Hello správce serveru, přejděte v příliš**nástroje** > **Správa zásad skupiny**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-132">In hello Server Manager, go too**Tools** > **Group Policy Management**.</span></span>
   
    ![Přejděte tooTools > Managment zásad skupiny](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. <span data-ttu-id="f2ea9-134">V levém podokně hello hello **Správa zásad skupiny** okně zobrazení hierarchie organizační jednotky (OU) a určit, ve které oboru chcete zásady skupiny tooapply hello.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-134">In hello left pane of hello **Group Policy Management** window, view your Organizational Unit (OU) hierarchy and determine at which scope you would like tooapply hello group policy.</span></span> <span data-ttu-id="f2ea9-135">Například se můžete rozhodnout toopick malé tooa toodeploy organizační jednotky několik uživatelů pro testování, nebo vyberete nejvyšší úrovni organizační jednotky toodeploy tooyour celé organizace.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-135">For instance, you may decide toopick a small OU toodeploy tooa few users for testing, or you may pick a top-level OU toodeploy tooyour entire organization.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f2ea9-136">Pokud by jako toocreate nebo upravit organizačních jednotkách (OU), přepněte zpět toohello správce serveru a přejděte příliš**nástroje** > **Active Directory Users and Computers**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-136">If you would like toocreate or edit your Organization Units (OUs), switch back toohello Server Manager and go too**Tools** > **Active Directory Users and Computers**.</span></span>
   > 
   > 
4. <span data-ttu-id="f2ea9-137">Jakmile vyberete organizační jednotku, pravým tlačítkem myši a vyberte **vytvořit objekt zásad skupiny v této doméně a propojit jej sem...**</span><span class="sxs-lookup"><span data-stu-id="f2ea9-137">Once you have selected an OU, right-click it and select **Create a GPO in this domain, and Link it here...**</span></span>
   
    ![Vytvořit nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. <span data-ttu-id="f2ea9-139">V hello **nový objekt zásad skupiny** řádku, zadejte název pro hello nového objektu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-139">In hello **New GPO** prompt, type in a name for hello new Group Policy Object.</span></span>
   
    ![Název hello nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. <span data-ttu-id="f2ea9-141">Hello klikněte pravým tlačítkem na objekt zásad skupiny, který jste vytvořili a vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-141">Right-click hello Group Policy Object that you created, and select **Edit**.</span></span>
   
    ![Upravit hello nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-hello-installation-package"></a><span data-ttu-id="f2ea9-143">Krok 3: Přiřaďte hello instalační balíček</span><span class="sxs-lookup"><span data-stu-id="f2ea9-143">Step 3: Assign hello Installation Package</span></span>
1. <span data-ttu-id="f2ea9-144">Určete, jestli chcete rozšíření hello toodeploy na základě **konfigurace počítače** nebo **konfigurace uživatele**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-144">Determine whether you would like toodeploy hello extension based on **Computer Configuration** or **User Configuration**.</span></span> <span data-ttu-id="f2ea9-145">Při použití [konfigurace počítače](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), rozšíření hello je nainstalovaná na počítači hello bez ohledu na to, které uživatelé přihlásit tooit.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-145">When using [computer configuration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), hello extension is installed on hello computer regardless of which users log on tooit.</span></span> <span data-ttu-id="f2ea9-146">S [konfigurace uživatele](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), uživatelé mají hello rozšíření nainstalovat pro ně bez ohledu na počítače, které se přihlašují.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-146">With [user configuration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), users have hello extension installed for them regardless of which computers they log on to.</span></span>
2. <span data-ttu-id="f2ea9-147">V levém podokně hello hello **Editor správy zásad skupiny** okně přejděte tooeither hello následující cesty ke složce zadat, v závislosti na tom, jaký typ konfigurace jste zvolili:</span><span class="sxs-lookup"><span data-stu-id="f2ea9-147">In hello left pane of hello **Group Policy Management Editor** window, go tooeither of hello following folder paths, depending on which type of configuration you chose:</span></span>
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. <span data-ttu-id="f2ea9-148">Klikněte pravým tlačítkem na **instalace softwaru**, pak vyberte **nový** > **balíčku...**</span><span class="sxs-lookup"><span data-stu-id="f2ea9-148">Right-click **Software installation**, then select **New** > **Package...**</span></span>
   
    ![Vytvořit nový balíček pro instalaci softwaru](./media/active-directory-saas-ie-group-policy/new-package.png)
4. <span data-ttu-id="f2ea9-150">Přejděte toohello sdílená složka, která obsahuje balíček instalačního programu hello z [krok 1: Vytvoření distribučního bodu hello](#step-1-create-the-distribution-point), vyberte soubor MSI hello a klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-150">Go toohello shared folder that contains hello installer package from [Step 1: Create hello Distribution Point](#step-1-create-the-distribution-point), select hello .msi file, and click **Open**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="f2ea9-151">Pokud sdílená složka hello je umístěná na tento stejný server, ověřte, zda přistupujete hello .msi prostřednictvím cesta k souboru hello sítě, nikoli hello místní cesta.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-151">If hello share is located on this same server, verify that you are accessing hello .msi through hello network file path, rather than hello local file path.</span></span>
   > 
   > 
   
    ![Vyberte hello instalační balíček ze sdílené složky hello.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. <span data-ttu-id="f2ea9-153">V hello **nasazení softwaru** výzvy, vyberte **přiřazeno** pro metodu nasazení.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-153">In hello **Deploy Software** prompt, select **Assigned** for your deployment method.</span></span> <span data-ttu-id="f2ea9-154">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-154">Then click **OK**.</span></span>
   
    ![Vyberte přiřazeno a potom klikněte na tlačítko OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

<span data-ttu-id="f2ea9-156">Hello rozšíření je nyní nasazené toohello organizační jednotku, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-156">hello extension is now deployed toohello OU that you selected.</span></span> [<span data-ttu-id="f2ea9-157">Další informace o instalace softwaru zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-157">Learn more about Group Policy Software Installation.</span></span>](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-hello-extension-for-internet-explorer"></a><span data-ttu-id="f2ea9-158">Krok 4: Povolit automatické hello rozšíření pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="f2ea9-158">Step 4: Auto-Enable hello Extension for Internet Explorer</span></span>
<span data-ttu-id="f2ea9-159">Kromě toho instalačního programu hello toorunning, každou příponu pro Internet Explorer musí být explicitně povoleno před použitím.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-159">In addition toorunning hello installer, every extension for Internet Explorer must be explicitly enabled before it can be used.</span></span> <span data-ttu-id="f2ea9-160">Hello postupujte podle kroků tooenable hello rozšíření panely přístup pomocí zásad skupiny:</span><span class="sxs-lookup"><span data-stu-id="f2ea9-160">Follow hello steps below tooenable hello Access Panel Extension using group policy:</span></span>

1. <span data-ttu-id="f2ea9-161">V hello **Editor správy zásad skupiny** přejděte tooeither hello následující cesty, v závislosti na tom, jaký typ konfigurace, který jste zvolili v okně [krok 3: přiřaďte hello instalační balíček](#step-3-assign-the-installation-package):</span><span class="sxs-lookup"><span data-stu-id="f2ea9-161">In hello **Group Policy Management Editor** window, go tooeither of hello following paths, depending on which type of configuration you chose in [Step 3: Assign hello Installation Package](#step-3-assign-the-installation-package):</span></span>
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. <span data-ttu-id="f2ea9-162">Klikněte pravým tlačítkem na **seznam doplňků**a vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-162">Right-click **Add-on List**, and select **Edit**.</span></span>
    <span data-ttu-id="f2ea9-163">![Seznam rozšíření upravte.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span><span class="sxs-lookup"><span data-stu-id="f2ea9-163">![Edit Add-on List.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span></span>
3. <span data-ttu-id="f2ea9-164">V hello **seznam doplňků** vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-164">In hello **Add-on List** window, select **Enabled**.</span></span> <span data-ttu-id="f2ea9-165">Potom v části hello **možnosti** klikněte na tlačítko **zobrazit...** .</span><span class="sxs-lookup"><span data-stu-id="f2ea9-165">Then, under hello **Options** section, click **Show...**.</span></span>
   
    ![Klikněte na položku Povolit a pak klikněte na zobrazit...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. <span data-ttu-id="f2ea9-167">V hello **zobrazit obsah** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f2ea9-167">In hello **Show Contents** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="f2ea9-168">Pro první sloupec hello (hello **název hodnoty** pole), zkopírujte a vložte hello následující ID třídy:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span><span class="sxs-lookup"><span data-stu-id="f2ea9-168">For hello first column (hello **Value Name** field), copy and paste hello following Class ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span></span>
   2. <span data-ttu-id="f2ea9-169">Pro druhý sloupec hello (hello **hodnotu** pole), zadejte hello následující hodnotu:`1`</span><span class="sxs-lookup"><span data-stu-id="f2ea9-169">For hello second column (hello **Value** field), type in hello following value: `1`</span></span>
   3. <span data-ttu-id="f2ea9-170">Klikněte na tlačítko **OK** tooclose hello **zobrazit obsah** okna.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-170">Click **OK** tooclose hello **Show Contents** window.</span></span>
      
      ![Vyplňte hodnoty hello jak je uvedeno výše.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. <span data-ttu-id="f2ea9-172">Klikněte na tlačítko **OK** tooapply změny a zavřít hello **seznam doplňků** okno.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-172">Click **OK** tooapply your changes and close hello **Add-on List** window.</span></span>

<span data-ttu-id="f2ea9-173">Hello rozšíření by měl nyní povoleno pro hello počítačů v hello vybrané organizační jednotce.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-173">hello extension should now be enabled for hello machines in hello selected OU.</span></span> [<span data-ttu-id="f2ea9-174">Další informace o používání tooenable zásad skupiny nebo zakázat rozšíření prohlížeče Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-174">Learn more about using group policy tooenable or disable Internet Explorer add-ons.</span></span>](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a><span data-ttu-id="f2ea9-175">Krok 5 (volitelné): zakázat "Zapamatovat heslo" řádku</span><span class="sxs-lookup"><span data-stu-id="f2ea9-175">Step 5 (Optional): Disable "Remember Password" Prompt</span></span>
<span data-ttu-id="f2ea9-176">Při mohou uživatelé přihlášení toowebsites pomocí hello rozšíření přístup k panelu, Internet Explorer následující hello zobrazit výzva s dotazem "by vám jako toostore heslo?"</span><span class="sxs-lookup"><span data-stu-id="f2ea9-176">When users sign-in toowebsites using hello Access Panel Extension, Internet Explorer may show hello following prompt asking "Would you like toostore your password?"</span></span>

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

<span data-ttu-id="f2ea9-177">Pokud chcete uživatelům z výzva zobrazila tooprevent, potom postupujte podle kroků hello tooprevent automatické dokončování z zapamatování hesla:</span><span class="sxs-lookup"><span data-stu-id="f2ea9-177">If you wish tooprevent your users from seeing this prompt, then follow hello steps below tooprevent auto-complete from remembering passwords:</span></span>

1. <span data-ttu-id="f2ea9-178">V hello **Editor správy zásad skupiny** okno, přejděte toohello cestu uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-178">In hello **Group Policy Management Editor** window, go toohello path listed below.</span></span> <span data-ttu-id="f2ea9-179">Toto nastavení konfigurace je k dispozici v části pouze **konfigurace uživatele**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-179">This configuration setting is only available under **User Configuration**.</span></span>
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. <span data-ttu-id="f2ea9-180">Najít nastavení hello s názvem **zapnout hello funkce automatického dokončování pro uživatelská jména a hesla ve formulářích**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-180">Find hello setting named **Turn on hello auto-complete feature for user names and passwords on forms**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f2ea9-181">Předchozí verze služby Active Directory může zobrazit seznam toto nastavení s názvem hello **zakázat automatické dokončování toosave hesla**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-181">Previous versions of Active Directory may list this setting with hello name **Do not allow auto-complete toosave passwords**.</span></span> <span data-ttu-id="f2ea9-182">Hello konfigurace tohoto nastavení se liší od hello nastavení popsané v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-182">hello configuration for that setting differs from hello setting described in this tutorial.</span></span>
   > 
   > 
   
    ![Nezapomeňte toolook pro to, v části Nastavení uživatele.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. <span data-ttu-id="f2ea9-184">Klikněte pravým tlačítkem na hello výše nastavení a vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-184">Right click hello above setting, and select **Edit**.</span></span>
4. <span data-ttu-id="f2ea9-185">V okně hello s názvem **zapnout hello funkce automatického dokončování pro uživatelská jména a hesla ve formulářích**, vyberte **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-185">In hello window titled **Turn on hello auto-complete feature for user names and passwords on forms**, select **Disabled**.</span></span>
   
    ![Vyberte zakázat](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. <span data-ttu-id="f2ea9-187">Klikněte na tlačítko **OK** tooapply tyto změny a zavřít hello okno.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-187">Click **OK** tooapply these changes and close hello window.</span></span>

<span data-ttu-id="f2ea9-188">Uživatelé budou už možné toostore přihlašovacích údajů nebo použít automatické dokončování tooaccess dříve uložené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-188">Users will no longer be able toostore their credentials or use auto-complete tooaccess previously stored credentials.</span></span> <span data-ttu-id="f2ea9-189">Však tato zásada Povolit uživatelům toocontinue toouse automatické dokončování pro jiné typy polí formuláře, například pole hledání.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-189">However, this policy does allow users toocontinue toouse auto-complete for other types of form fields, such as search fields.</span></span>

> [!WARNING]
> <span data-ttu-id="f2ea9-190">Pokud po uživatele vybrali toostore některé přihlašovací údaje, je tato zásada povolena, bude tato zásada *není* vymazat hello přihlašovací údaje, které již byly uloženy.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-190">If this policy is enabled after users have chosen toostore some credentials, this policy will *not* clear hello credentials that have already been stored.</span></span>
> 
> 

## <a name="step-6-testing-hello-deployment"></a><span data-ttu-id="f2ea9-191">Krok 6: Testování hello nasazení</span><span class="sxs-lookup"><span data-stu-id="f2ea9-191">Step 6: Testing hello Deployment</span></span>
<span data-ttu-id="f2ea9-192">Pokud hello rozšíření nasazení proběhlo úspěšně, postupujte podle kroků hello níže tooverify:</span><span class="sxs-lookup"><span data-stu-id="f2ea9-192">Follow hello steps below tooverify if hello extension deployment was successful:</span></span>

1. <span data-ttu-id="f2ea9-193">Pokud jste nasadili pomocí **konfigurace počítače**, přihlaste se klientský počítač, který patří toohello organizační jednotku, kterou jste vybrali v [krok 2: vytvoření objektu zásad skupiny hello](#step-2-create-the-group-policy-object).</span><span class="sxs-lookup"><span data-stu-id="f2ea9-193">If you deployed using **Computer Configuration**, sign into a client machine that belongs toohello OU that you selected in [Step 2: Create hello Group Policy Object](#step-2-create-the-group-policy-object).</span></span> <span data-ttu-id="f2ea9-194">Pokud jste nasadili pomocí **konfigurace uživatele**, ujistěte se, že toosign v jako uživatel, který patří toothat organizační jednotky.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-194">If you deployed using **User Configuration**, make sure toosign in as a user who belongs toothat OU.</span></span>
2. <span data-ttu-id="f2ea9-195">Může trvat několik sign in pro zásady skupiny hello změny toofully aktualizace se tento počítač.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-195">It may take a couple sign ins for hello group policy changes toofully update with this machine.</span></span> <span data-ttu-id="f2ea9-196">tooforce hello aktualizace, otevřete **příkazového řádku** okno a spuštění hello následující příkaz:`gpupdate /force`</span><span class="sxs-lookup"><span data-stu-id="f2ea9-196">tooforce hello update, open a **Command Prompt** window and run hello following command: `gpupdate /force`</span></span>
3. <span data-ttu-id="f2ea9-197">Je nutné restartovat počítač hello pro místní tootake instalace hello.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-197">You must restart hello machine for hello installation tootake place.</span></span> <span data-ttu-id="f2ea9-198">Spuštění může trvat výrazně déle než obvykle při rozšíření hello nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-198">Bootup may take significantly more time than usual while hello extension installs.</span></span>
4. <span data-ttu-id="f2ea9-199">Po restartování počítače, otevřete **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-199">After restarting, open **Internet Explorer**.</span></span> <span data-ttu-id="f2ea9-200">V pravém horním rohu hello hello okna, klikněte na tlačítko **nástroje** (ozubené kolečko ikona hello) a potom vyberte **spravovat doplňky**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-200">On hello upper-right corner of hello window, click **Tools** (hello gear icon), and then select **Manage add-ons**.</span></span>
   
    ![Přejděte tooTools > Spravovat doplňky](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. <span data-ttu-id="f2ea9-202">V hello **spravovat doplňky** okno, ověřte, že hello **rozšíření přístup k panelu** byla nainstalována a že jeho **stav** byl nastaven příliš**povoleno**.</span><span class="sxs-lookup"><span data-stu-id="f2ea9-202">In hello **Manage Add-ons** window, verify that hello **Access Panel Extension** has been installed and that its **Status** has been set too**Enabled**.</span></span>
   
    ![Ověřte, že hello rozšíření přístup k panelu je nainstalovaný a povolený.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a><span data-ttu-id="f2ea9-204">Související články</span><span class="sxs-lookup"><span data-stu-id="f2ea9-204">Related Articles</span></span>
* [<span data-ttu-id="f2ea9-205">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f2ea9-205">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="f2ea9-206">Přístup k aplikaci a jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f2ea9-206">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="f2ea9-207">Řešení potíží s hello rozšíření panely přístup pro prohlížeč Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="f2ea9-207">Troubleshooting hello Access Panel Extension for Internet Explorer</span></span>](active-directory-saas-ie-troubleshooting.md)

