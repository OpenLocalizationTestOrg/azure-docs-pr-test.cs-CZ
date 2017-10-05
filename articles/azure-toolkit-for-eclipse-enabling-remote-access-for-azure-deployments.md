---
title: "Povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse"
description: "Postup povolení vzdáleného přístupu pro Azure nasazení pomocí sady nástrojů pro Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a><span data-ttu-id="e119d-103">Povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="e119d-103">Enabling Remote Access for Azure Deployments in Eclipse</span></span>
<span data-ttu-id="e119d-104">Pomoc při řešení potíží s vaše nasazení, můžete povolit a používat vzdáleného přístupu pro připojení k virtuálnímu počítači hostování vašeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="e119d-104">To help troubleshoot your deployments, you may enable and use Remote Access to connect to the virtual machine hosting your deployment.</span></span> <span data-ttu-id="e119d-105">Funkce vzdáleného přístupu závisí na protokol RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="e119d-105">The Remote Access functionality relies on the Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="e119d-106">Vzdálený přístup můžete nakonfigurovat pro vaše nasazení po publikovali do Azure, nebo pokud používáte Eclipse s operačním systémem Windows, můžete nakonfigurovat vzdálený přístup před publikováním do Azure.</span><span class="sxs-lookup"><span data-stu-id="e119d-106">You can configure Remote Access for your deployment after you have published it to Azure, or if you are using Eclipse with a Windows operating system, you can configure Remote Access before you publish to Azure.</span></span> <span data-ttu-id="e119d-107">Všimněte si, že budete potřebovat klient vzdálené plochy, který je kompatibilní s operačním systémem bylo před připojením k vašeho nasazení virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="e119d-107">Note that you will need a remote desktop client that is compatible with your operating system in order to connect to your deployment's virtual machine in Azure.</span></span>

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a><span data-ttu-id="e119d-108">Postup povolení vzdáleného přístupu, před nasazením do Azure</span><span class="sxs-lookup"><span data-stu-id="e119d-108">How to enable Remote Access before you deploy to Azure</span></span>
> [!NOTE]
> <span data-ttu-id="e119d-109">Postup povolení vzdáleného přístupu, před nasazením aplikace do Azure, budete muset používat prostředí Eclipse v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e119d-109">To enable Remote Access before you deploy your application to Azure, you need to be running Eclipse on Windows.</span></span>
> 
> 

<span data-ttu-id="e119d-110">Na následujícím obrázku **vzdáleného přístupu** dialogové okno Vlastnosti používaným pro povolení vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="e119d-110">The following image shows the **Remote Access** properties dialog used to enable remote access.</span></span>

![][ic719494]

<span data-ttu-id="e119d-111">Existují dva způsoby, jak zobrazit **vzdáleného přístupu** dialogové okno Vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="e119d-111">There are two ways to display the **Remote Access** properties dialog:</span></span>

* <span data-ttu-id="e119d-112">Klikněte na tlačítko **Upřesnit** na odkaz v **vzdáleného přístupu** části **publikovat do Azure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e119d-112">Click the **Advanced** link in the **Remote Access** section of the **Publish to Azure** dialog.</span></span>

* <span data-ttu-id="e119d-113">Otevřete **vlastnosti** dialogové okno Azure projektu.</span><span class="sxs-lookup"><span data-stu-id="e119d-113">Open the **Properties** dialog of your Azure project.</span></span>

<span data-ttu-id="e119d-114">Když vytvoříte nový projekt nasazení Azure, projekt nebude mít vzdálený přístup ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="e119d-114">When you create a new Azure deployment project, the project will not have Remote Access enabled by default.</span></span> <span data-ttu-id="e119d-115">Však můžete snadno povolit vzdáleného přístupu tak, že zadáte uživatelské jméno a heslo **publikovat do Azure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e119d-115">However, you can easily enable remote access by specifying the user name and password in the **Publish to Azure** dialog.</span></span> <span data-ttu-id="e119d-116">Heslo vzdáleného přístupu je zašifrována pomocí certifikátů X.509.</span><span class="sxs-lookup"><span data-stu-id="e119d-116">The Remote Access password is encrypted using X.509 certificates.</span></span> <span data-ttu-id="e119d-117">Pokud nepoužijete zadejte svůj vlastní certifikát šifrování spoléhá na certifikát podepsaný svým držitelem, které jsou součástí modulu plug-in Azure pro Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e119d-117">If you do not use provide your own certificate, the encryption relies on a self-signed certificate shipped with the Azure Plugin for Eclipse.</span></span> <span data-ttu-id="e119d-118">Tento certifikát podepsaný držitelem je v **cert** složce projektu Azure uložená i jako soubor certifikátu veřejného (SampleRemoteAccessPublic.cer) a jako Personal Information Exchange (PFX) soubor certifikátu () SampleRemoteAccessPrivate.pfx).</span><span class="sxs-lookup"><span data-stu-id="e119d-118">This self-signed certificate is in the **cert** folder of your Azure project, stored both as a public certificate file (SampleRemoteAccessPublic.cer) and as a Personal Information Exchange (PFX) certificate file (SampleRemoteAccessPrivate.pfx).</span></span> <span data-ttu-id="e119d-119">Ta obsahuje soukromý klíč pro certifikát, a má výchozí heslo **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="e119d-119">The latter contains the private key for the certificate, and it has a default password, **Password1**.</span></span> <span data-ttu-id="e119d-120">Vzhledem k tomu, že je toto heslo veřejné znalostní báze, výchozí certifikát by měl použít pouze pro účely, není pro produkční nasazení učení.</span><span class="sxs-lookup"><span data-stu-id="e119d-120">However, since this password is public knowledge, the default certificate should be used only for learning purposes, not for a production deployment.</span></span> <span data-ttu-id="e119d-121">Takže než pro učení účely, pokud chcete povolit vzdálené relace pro vaše nasazení, měli byste kliknout na **Upřesnit** na odkaz v **publikovat do Azure** dialogovém okně můžete zadat vlastní certifikát.</span><span class="sxs-lookup"><span data-stu-id="e119d-121">So other than for learning purposes, when you want to enabled remote sessions for your deployments, you should click the **Advanced** link in the **Publish to Azure** dialog to specify your own certificate.</span></span> <span data-ttu-id="e119d-122">Všimněte si, že budete muset nahrát verze PFX certifikátu k vaší hostované služby v rámci portálu pro správu Azure tak, že Azure může dešifrovat heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="e119d-122">Note that you'll need to upload the PFX version of the certificate to your hosted service within the Azure Management Portal, so that Azure can decrypt the user password.</span></span>

<span data-ttu-id="e119d-123">Zbývající část tohoto kurzu se dozvíte, jak pro povolení vzdáleného přístupu pro projekt nasazení Azure, která byla původně vytvořena s vzdálený přístup zakázán.</span><span class="sxs-lookup"><span data-stu-id="e119d-123">The remainder of the tutorial shows you how to enable remote access for an Azure deployment project that was initially created with remote access disabled.</span></span> <span data-ttu-id="e119d-124">Pro účely tohoto kurzu vytvoříme nový certifikát podepsaný svým držitelem a jeho soubor .pfx, bude mít heslo podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="e119d-124">For purposes of this tutorial, we'll create a new self-signed certificate, and its .pfx file will have a password of your choice.</span></span> <span data-ttu-id="e119d-125">Máte také možnost použít certifikát vydaný certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="e119d-125">You also have the option of using a certificate issued by a certificate authority.</span></span>

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a><span data-ttu-id="e119d-126">Postup povolení vzdáleného přístupu, když nasadíte do Azure</span><span class="sxs-lookup"><span data-stu-id="e119d-126">How to enable Remote Access after you have deployed to Azure</span></span>
<span data-ttu-id="e119d-127">Postup povolení vzdáleného přístupu, když nasadíte do Azure, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="e119d-127">To enable remote access after you have deployed to Azure, use the following steps:</span></span>

1. <span data-ttu-id="e119d-128">Přihlaste se k portálu správy Azure pomocí účtu Azure</span><span class="sxs-lookup"><span data-stu-id="e119d-128">Log into the Azure management portal using your Azure account</span></span>

2. <span data-ttu-id="e119d-129">V seznamu **cloudové služby**, vyberte nasazené cloudové služby</span><span class="sxs-lookup"><span data-stu-id="e119d-129">In your list of **Cloud Services**, select your deployed cloud service</span></span>

3. <span data-ttu-id="e119d-130">Cloudové služby webové stránce, klikněte na tlačítko **konfigurace** odkaz</span><span class="sxs-lookup"><span data-stu-id="e119d-130">In the cloud service web page, click the **Configure** link</span></span>

4. <span data-ttu-id="e119d-131">V dolní části stránky konfigurace, klikněte na **vzdáleného** odkaz</span><span class="sxs-lookup"><span data-stu-id="e119d-131">On the bottom of the configuration page, click the **Remote** link</span></span>

5. <span data-ttu-id="e119d-132">Jakmile se zobrazí dialogové okno automaticky otevírané okno:</span><span class="sxs-lookup"><span data-stu-id="e119d-132">When the pop-up dialog box appears:</span></span>
   
   * <span data-ttu-id="e119d-133">Zadejte Role, pro který chcete povolit vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="e119d-133">Specify the Role you for which you want to enable remote access</span></span>

   * <span data-ttu-id="e119d-134">Kliknutím vyberte **povolení vzdálené plochy** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="e119d-134">Click to select the **Enable Remote Desktop** checkbox</span></span>
   
   * <span data-ttu-id="e119d-135">Zadejte uživatelské jméno a heslo, které chcete použít pro vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="e119d-135">Specify a user name and password you want to use for remote access</span></span>
   
   * <span data-ttu-id="e119d-136">Vyberte certifikát, který chcete použít</span><span class="sxs-lookup"><span data-stu-id="e119d-136">Select the certificate to use</span></span>

6. <span data-ttu-id="e119d-137">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e119d-137">Click **OK**</span></span> 

<span data-ttu-id="e119d-138">Zobrazí se zpráva, že změna konfigurace je v průběhu, který může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="e119d-138">You will see a message stating that your configuration change is in progress, which may take a few minutes to complete.</span></span> <span data-ttu-id="e119d-139">Po dokončení změny konfigurace, postupujte podle kroků v **přihlásit vzdáleně** později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e119d-139">After the configuration change has completed, follow the steps in the **To log in remotely** section later in this article.</span></span>

## <a name="how-to-enable-remote-access-in-your-package"></a><span data-ttu-id="e119d-140">Postup povolení vzdáleného přístupu na balíček</span><span class="sxs-lookup"><span data-stu-id="e119d-140">How to enable Remote Access in your package</span></span>
1. <span data-ttu-id="e119d-141">V podokně Eclipse na prohlížeči projektu klikněte pravým tlačítkem na projekt Azure a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e119d-141">Within Eclipse's Project Explorer pane, right-click your Azure project and click **Properties**.</span></span>

2. <span data-ttu-id="e119d-142">V **vlastnosti** dialogové okno, rozbalte položku **Azure** v levém podokně a klikněte na **vzdáleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="e119d-142">In the **Properties** dialog, expand **Azure** in the left-hand pane and click **Remote Access**.</span></span>

3. <span data-ttu-id="e119d-143">V **vzdáleného přístupu** dialogové okno, ujistěte se, **povolit všechny role tak, aby přijímal připojení vzdálené plochy s tyto přihlašovací údaje** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="e119d-143">In the **Remote Access** dialog, ensure **Enable all roles to accept Remote Desktop Connections with these login credentials** is checked.</span></span>

4. <span data-ttu-id="e119d-144">Zadejte uživatelské jméno pro připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="e119d-144">Specify a user name for the Remote Desktop connection.</span></span>

5. <span data-ttu-id="e119d-145">Zadejte a potvrďte heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e119d-145">Specify and confirm the password for the user.</span></span> <span data-ttu-id="e119d-146">Uživatelské jméno a heslo hodnotami nastavenými v tomto dialogu se použije při vytváření připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="e119d-146">The user name and password values set in this dialog will be used when you make a Remote Desktop connection.</span></span> <span data-ttu-id="e119d-147">(Všimněte si, že se jedná o samostatný heslo z PFX heslo.)</span><span class="sxs-lookup"><span data-stu-id="e119d-147">(Note that this is a separate password from your PFX password.)</span></span>

6. <span data-ttu-id="e119d-148">Zadejte datum vypršení platnosti pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="e119d-148">Specify the expiration date for the user account.</span></span>

7. <span data-ttu-id="e119d-149">Klikněte na tlačítko **nový** k vytvoření nového certifikátu podepsaného svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="e119d-149">Click **New** to create a new self-signed certificate.</span></span> <span data-ttu-id="e119d-150">(Alternativně můžete třeba vybrat certifikát z vašeho pracovního prostoru nebo souboru systému prostřednictvím **prostoru** nebo **FileSystem** tlačítka, v uvedeném pořadí, ale pro účely tohoto kurzu vytvoříme novou certifikát.)</span><span class="sxs-lookup"><span data-stu-id="e119d-150">(Alternatively, you could select a certificate from your workspace or file system through the **Workspace** or **FileSystem** buttons, respectively, but for purposes of this tutorial we'll create a new certificate.)</span></span>

   * <span data-ttu-id="e119d-151">V **nový certifikát** dialogové okno, zadejte a potvrďte heslo budete používat pro soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="e119d-151">In the **New Certificate** dialog, specify and confirm the password you'll use for your PFX file.</span></span>

   * <span data-ttu-id="e119d-152">Hodnota zadaná pro přijmout **název (CN)**, nebo použít vlastní název.</span><span class="sxs-lookup"><span data-stu-id="e119d-152">Accept the value provided for **Name (CN)**, or use a custom name.</span></span>

   * <span data-ttu-id="e119d-153">Zadejte název a cesta k souboru, kde bude uložen nový certifikát v podobě .cer.</span><span class="sxs-lookup"><span data-stu-id="e119d-153">Specify the path and file name where the new certificate, in .cer form, will be saved.</span></span> <span data-ttu-id="e119d-154">Pro tento krok a dalšímu kroku, můžete použít **cert** složce projektu Azure, ale máte volné vyberte jiné umístění.</span><span class="sxs-lookup"><span data-stu-id="e119d-154">For this step and the next step, you could use the **cert** folder of your Azure project, but you're free to choose another location.</span></span> <span data-ttu-id="e119d-155">Pro účely tohoto kurzu použijeme **c:\mycert\mycert.cer**.</span><span class="sxs-lookup"><span data-stu-id="e119d-155">For purposes of this tutorial, we'll use **c:\mycert\mycert.cer**.</span></span> <span data-ttu-id="e119d-156">(Vytvořte **c:\mycert** složku před pokračováním, nebo použijte existující složku v případě potřeby.)</span><span class="sxs-lookup"><span data-stu-id="e119d-156">(Create the **c:\mycert** folder prior to proceeding, or use an existing folder if desired.)</span></span>

   * <span data-ttu-id="e119d-157">Zadejte název a cesta k souboru, kde bude uložen nový certifikát a jeho privátní klíč ve formátu .pfx.</span><span class="sxs-lookup"><span data-stu-id="e119d-157">Specify the path and file name where the new certificate and its private key, in .pfx form, will be saved.</span></span> <span data-ttu-id="e119d-158">Pro účely tohoto kurzu použijeme **c:\mycert\mycert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="e119d-158">For purposes of this tutorial, we'll use **c:\mycert\mycert.pfx**.</span></span> <span data-ttu-id="e119d-159">Vaše **nový certifikát** by měl vypadat podobně jako následující dialogové okno (aktualizace cestách ke složkám, pokud jste nepoužili **c:\mycert**):</span><span class="sxs-lookup"><span data-stu-id="e119d-159">Your **New Certificate** dialog should look similar to the following (update the folder paths if you did not use **c:\mycert**):</span></span>
     
      ![][ic712275]

   * <span data-ttu-id="e119d-160">Klikněte na tlačítko **OK** zavřete **nový certifikát** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e119d-160">Click **OK** to close the **New Certificate** dialog.</span></span>

8. <span data-ttu-id="e119d-161">Vaše **vzdáleného přístupu** dialogové okno by měl vypadat podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="e119d-161">Your **Remote Access** dialog should look similar to the following:</span></span></p>
   
   ![][ic719495]

9. <span data-ttu-id="e119d-162">Klikněte na tlačítko **OK** zavřete **vzdáleného přístupu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e119d-162">Click **OK** to close the **Remote Access** dialog.</span></span>

<span data-ttu-id="e119d-163">Sestavení aplikace, se sestavením, nastavte pro nasazení do cloudu.</span><span class="sxs-lookup"><span data-stu-id="e119d-163">Rebuild your application, with the build set for deployment to cloud.</span></span>

## <a name="to-log-in-remotely"></a><span data-ttu-id="e119d-164">Ke vzdálenému</span><span class="sxs-lookup"><span data-stu-id="e119d-164">To log in remotely</span></span>
<span data-ttu-id="e119d-165">Jakmile vaši instanci role je připraven, můžete vzdáleně přihlášení do virtuálního počítače, který je hostitelem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e119d-165">Once your role instance is ready, you can remotely log in to the virtual machine that is hosting your application.</span></span>

* <span data-ttu-id="e119d-166">Pokud používáte Eclipse ve Windows a vybrali jste **spuštění vzdálené plochy na nasazení** možnost během nasazení do Azure, zobrazí se přihlašovací obrazovka připojení ke vzdálené ploše při spuštění vašeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="e119d-166">If are using Eclipse on Windows and you selected the **Start remote desktop on deploy** option during your deployment to Azure, you will be presented with a Remote Desktop Connection logon screen when your deployment starts.</span></span> <span data-ttu-id="e119d-167">Po zobrazení výzvy pro uživatelské jméno a heslo, zadejte hodnoty, které jste zadali pro vzdálené uživatele a bude moct přihlásit.</span><span class="sxs-lookup"><span data-stu-id="e119d-167">When you are prompted for the user name and password, enter the values that you specified for the remote user and will be able to log in.</span></span>

* <span data-ttu-id="e119d-168">Je také možné přihlásit vzdáleně přes <a href="http://go.microsoft.com/fwlink/?LinkID=512959">portálu pro správu Azure</a>:</span><span class="sxs-lookup"><span data-stu-id="e119d-168">Another way to log in remotely is through the <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:</span></span>
  
  * <span data-ttu-id="e119d-169">V rámci **cloudové služby** zobrazení portálu pro správu Azure, klikněte na tlačítko cloudové služby, klikněte na **instance**, klikněte na konkrétní instanci a klikněte **připojit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e119d-169">Within the **Cloud Services** view of the Azure Management portal, click your cloud service, click **Instances**, click a specific instance, and then click the **Connect** button.</span></span> <span data-ttu-id="e119d-170">**Connect** tlačítko se zobrazí jako následující na panelu příkazů:</span><span class="sxs-lookup"><span data-stu-id="e119d-170">The **Connect** button appears as the following in the command bar:</span></span>
    
      ![][ic659273]

  * <span data-ttu-id="e119d-171">Po kliknutí na tlačítko **Connect** tlačítko, zobrazí se výzva k otevření souboru RDP.</span><span class="sxs-lookup"><span data-stu-id="e119d-171">After clicking the **Connect** button, you will be prompted to open an RDP file.</span></span> <span data-ttu-id="e119d-172">Otevřete soubor a postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="e119d-172">Open the file and follow the prompts.</span></span> <span data-ttu-id="e119d-173">(Můžete může také uložte tento soubor do místního počítače a potom spusťte soubor poklepáním na vzdálené přihlášení k virtuálnímu počítači bez nutnosti nejprve přejděte na portálu pro správu.)</span><span class="sxs-lookup"><span data-stu-id="e119d-173">(You could also save this file to your local computer, and then run the file by double-clicking it to remote log in to your virtual machine without needing to first go the management portal.)</span></span>

  * <span data-ttu-id="e119d-174">Po zobrazení výzvy pro uživatelské jméno a heslo, zadejte hodnoty, které jste zadali pro vzdálené uživatele a bude moct přihlásit.</span><span class="sxs-lookup"><span data-stu-id="e119d-174">When you are prompted for the user name and password, enter the values that you specified for the remote user and will be able to log in.</span></span>

> [!NOTE]
> <span data-ttu-id="e119d-175">Pokud jste na jiný systém než Windows operační systém, budete muset použít klienta vzdálené plochy, který je kompatibilní s operačním systémem a postup konfigurace tohoto klienta pomocí nastavení v souboru RDP, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="e119d-175">If you are on a non-Windows operating system, you need to use a Remote Desktop client that is compatible with your operating system and follow the steps to configure that client with the settings in the RDP file that you downloaded.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="e119d-176">Viz také</span><span class="sxs-lookup"><span data-stu-id="e119d-176">See Also</span></span>
<span data-ttu-id="e119d-177">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e119d-177">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="e119d-178">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e119d-178">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="e119d-179">[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e119d-179">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="e119d-180">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="e119d-180">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
