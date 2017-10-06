---
title: "aaaEnabling vzdáleného přístupu pro Azure nasazení v prostředí Eclipse"
description: "Zjistěte, jak tooenable vzdáleného přístupu pro Azure nasazení pomocí hello Azure Toolkit pro Eclipse."
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
ms.openlocfilehash: 00c2bf22c1f3ec792098f154f771c87506e87881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a><span data-ttu-id="d9c9d-103">Povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="d9c9d-103">Enabling Remote Access for Azure Deployments in Eclipse</span></span>
<span data-ttu-id="d9c9d-104">toohelp řešení nasazení, můžete povolit a používat vzdálený přístup tooconnect toohello virtuální počítač hostování vašeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-104">toohelp troubleshoot your deployments, you may enable and use Remote Access tooconnect toohello virtual machine hosting your deployment.</span></span> <span data-ttu-id="d9c9d-105">Hello funkce vzdáleného přístupu závisí na hello protokolu RDP (Remote Desktop).</span><span class="sxs-lookup"><span data-stu-id="d9c9d-105">hello Remote Access functionality relies on hello Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="d9c9d-106">Po jejím publikování tooAzure, nebo pokud používáte Eclipse s operačním systémem Windows, můžete nakonfigurovat vzdálený přístup před publikováním tooAzure, můžete nakonfigurovat vzdáleného přístupu pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-106">You can configure Remote Access for your deployment after you have published it tooAzure, or if you are using Eclipse with a Windows operating system, you can configure Remote Access before you publish tooAzure.</span></span> <span data-ttu-id="d9c9d-107">Všimněte si, že budete potřebovat klient vzdálené plochy, který je kompatibilní s operačním systémem v pořadí tooconnect tooyour nasazení virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-107">Note that you will need a remote desktop client that is compatible with your operating system in order tooconnect tooyour deployment's virtual machine in Azure.</span></span>

## <a name="how-tooenable-remote-access-before-you-deploy-tooazure"></a><span data-ttu-id="d9c9d-108">Nasazení vzdáleného přístupu tooenable před tooAzure</span><span class="sxs-lookup"><span data-stu-id="d9c9d-108">How tooenable Remote Access before you deploy tooAzure</span></span>
> [!NOTE]
> <span data-ttu-id="d9c9d-109">tooenable vzdálený přístup před nasazením tooAzure vaší aplikace, musíte toobe Eclipse systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-109">tooenable Remote Access before you deploy your application tooAzure, you need toobe running Eclipse on Windows.</span></span>
> 
> 

<span data-ttu-id="d9c9d-110">Hello následující obrázek ukazuje hello **vzdáleného přístupu** dialogové okno Vlastnosti použít tooenable vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-110">hello following image shows hello **Remote Access** properties dialog used tooenable remote access.</span></span>

![][ic719494]

<span data-ttu-id="d9c9d-111">Existují dva způsoby toodisplay hello **vzdáleného přístupu** dialogové okno Vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d9c9d-111">There are two ways toodisplay hello **Remote Access** properties dialog:</span></span>

* <span data-ttu-id="d9c9d-112">Klikněte na tlačítko hello **Upřesnit** odkaz v hello **vzdáleného přístupu** části hello **publikování tooAzure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-112">Click hello **Advanced** link in hello **Remote Access** section of hello **Publish tooAzure** dialog.</span></span>

* <span data-ttu-id="d9c9d-113">Otevřete hello **vlastnosti** dialogové okno Azure projektu.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-113">Open hello **Properties** dialog of your Azure project.</span></span>

<span data-ttu-id="d9c9d-114">Když vytvoříte nový projekt nasazení Azure, hello projektu nebude mít vzdálený přístup ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-114">When you create a new Azure deployment project, hello project will not have Remote Access enabled by default.</span></span> <span data-ttu-id="d9c9d-115">Však můžete snadno povolit vzdálený přístup zadáním hello uživatelské jméno a heslo v hello **publikování tooAzure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-115">However, you can easily enable remote access by specifying hello user name and password in hello **Publish tooAzure** dialog.</span></span> <span data-ttu-id="d9c9d-116">heslo Hello vzdáleného přístupu je zašifrována pomocí certifikátů X.509.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-116">hello Remote Access password is encrypted using X.509 certificates.</span></span> <span data-ttu-id="d9c9d-117">Pokud nepoužijete zadejte svůj vlastní certifikát šifrování hello spoléhá na certifikát podepsaný svým držitelem, které jsou součástí hello modul plug-in Azure pro prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-117">If you do not use provide your own certificate, hello encryption relies on a self-signed certificate shipped with hello Azure Plugin for Eclipse.</span></span> <span data-ttu-id="d9c9d-118">Tento certifikát podepsaný držitelem je v hello **cert** složce projektu Azure uložená i jako soubor certifikátu veřejného (SampleRemoteAccessPublic.cer) a jako Personal Information Exchange (PFX) soubor certifikátu () SampleRemoteAccessPrivate.pfx).</span><span class="sxs-lookup"><span data-stu-id="d9c9d-118">This self-signed certificate is in hello **cert** folder of your Azure project, stored both as a public certificate file (SampleRemoteAccessPublic.cer) and as a Personal Information Exchange (PFX) certificate file (SampleRemoteAccessPrivate.pfx).</span></span> <span data-ttu-id="d9c9d-119">Hello pozdější obsahuje hello privátní klíč pro certifikát hello a má výchozí heslo **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-119">hello latter contains hello private key for hello certificate, and it has a default password, **Password1**.</span></span> <span data-ttu-id="d9c9d-120">Vzhledem k tomu, že je toto heslo veřejné znalostní báze, hello výchozí certifikát by měl použít pouze pro účely, není pro produkční nasazení učení.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-120">However, since this password is public knowledge, hello default certificate should be used only for learning purposes, not for a production deployment.</span></span> <span data-ttu-id="d9c9d-121">Takže než pro učení účely, pokud chcete vzdálené relace tooenabled nasazení, měli byste kliknout na hello **Upřesnit** odkaz v hello **publikování tooAzure** toospecify dialogové okno Vlastní certifikát.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-121">So other than for learning purposes, when you want tooenabled remote sessions for your deployments, you should click hello **Advanced** link in hello **Publish tooAzure** dialog toospecify your own certificate.</span></span> <span data-ttu-id="d9c9d-122">Všimněte si, že potřebujete tooupload hello PFX verze hello certifikát tooyour hostované služby v rámci hello portálu pro správu Azure, tak, že Azure může dešifrovat heslo uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-122">Note that you'll need tooupload hello PFX version of hello certificate tooyour hosted service within hello Azure Management Portal, so that Azure can decrypt hello user password.</span></span>

<span data-ttu-id="d9c9d-123">Hello zbytek hello kurz ukazuje, jak tooenable vzdáleného přístupu pro projekt nasazení Azure, která byla původně vytvořena s vzdálený přístup zakázán.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-123">hello remainder of hello tutorial shows you how tooenable remote access for an Azure deployment project that was initially created with remote access disabled.</span></span> <span data-ttu-id="d9c9d-124">Pro účely tohoto kurzu vytvoříme nový certifikát podepsaný svým držitelem a jeho soubor .pfx, bude mít heslo podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-124">For purposes of this tutorial, we'll create a new self-signed certificate, and its .pfx file will have a password of your choice.</span></span> <span data-ttu-id="d9c9d-125">Máte také možnost hello používá certifikát vydaný certifikační autoritou.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-125">You also have hello option of using a certificate issued by a certificate authority.</span></span>

## <a name="how-tooenable-remote-access-after-you-have-deployed-tooazure"></a><span data-ttu-id="d9c9d-126">Jak tooenable vzdáleného přístupu po jste nasadili tooAzure</span><span class="sxs-lookup"><span data-stu-id="d9c9d-126">How tooenable Remote Access after you have deployed tooAzure</span></span>
<span data-ttu-id="d9c9d-127">tooenable vzdálený přístup po nasazení tooAzure hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9c9d-127">tooenable remote access after you have deployed tooAzure, use hello following steps:</span></span>

1. <span data-ttu-id="d9c9d-128">Přihlaste se pomocí účtu Azure portál pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="d9c9d-128">Log into hello Azure management portal using your Azure account</span></span>

2. <span data-ttu-id="d9c9d-129">V seznamu **cloudové služby**, vyberte nasazené cloudové služby</span><span class="sxs-lookup"><span data-stu-id="d9c9d-129">In your list of **Cloud Services**, select your deployed cloud service</span></span>

3. <span data-ttu-id="d9c9d-130">Hello cloudové služby webové stránce, klikněte na tlačítko hello **konfigurace** odkaz</span><span class="sxs-lookup"><span data-stu-id="d9c9d-130">In hello cloud service web page, click hello **Configure** link</span></span>

4. <span data-ttu-id="d9c9d-131">Na hello dolní části stránky hello konfigurace, klikněte na tlačítko hello **vzdáleného** odkaz</span><span class="sxs-lookup"><span data-stu-id="d9c9d-131">On hello bottom of hello configuration page, click hello **Remote** link</span></span>

5. <span data-ttu-id="d9c9d-132">Jakmile se zobrazí dialogové okno hello automaticky otevírané okno:</span><span class="sxs-lookup"><span data-stu-id="d9c9d-132">When hello pop-up dialog box appears:</span></span>
   
   * <span data-ttu-id="d9c9d-133">Zadejte hello Role můžete pro kterou chcete tooenable vzdáleného přístupu</span><span class="sxs-lookup"><span data-stu-id="d9c9d-133">Specify hello Role you for which you want tooenable remote access</span></span>

   * <span data-ttu-id="d9c9d-134">Klikněte na tlačítko tooselect hello **povolení vzdálené plochy** zaškrtávací políčko</span><span class="sxs-lookup"><span data-stu-id="d9c9d-134">Click tooselect hello **Enable Remote Desktop** checkbox</span></span>
   
   * <span data-ttu-id="d9c9d-135">Zadejte uživatelské jméno a heslo, které chcete toouse pro vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="d9c9d-135">Specify a user name and password you want toouse for remote access</span></span>
   
   * <span data-ttu-id="d9c9d-136">Vyberte certifikát toouse hello</span><span class="sxs-lookup"><span data-stu-id="d9c9d-136">Select hello certificate toouse</span></span>

6. <span data-ttu-id="d9c9d-137">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-137">Click **OK**</span></span> 

<span data-ttu-id="d9c9d-138">Zobrazí se zpráva, že změna konfigurace je v průběhu, který může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-138">You will see a message stating that your configuration change is in progress, which may take a few minutes toocomplete.</span></span> <span data-ttu-id="d9c9d-139">Po změně konfigurace hello byl dokončen, postupujte podle kroků hello v hello **toolog v vzdáleně** později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-139">After hello configuration change has completed, follow hello steps in hello **toolog in remotely** section later in this article.</span></span>

## <a name="how-tooenable-remote-access-in-your-package"></a><span data-ttu-id="d9c9d-140">Jak tooenable vzdáleného přístupu na balíček</span><span class="sxs-lookup"><span data-stu-id="d9c9d-140">How tooenable Remote Access in your package</span></span>
1. <span data-ttu-id="d9c9d-141">V podokně Eclipse na prohlížeči projektu klikněte pravým tlačítkem na projekt Azure a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-141">Within Eclipse's Project Explorer pane, right-click your Azure project and click **Properties**.</span></span>

2. <span data-ttu-id="d9c9d-142">V hello **vlastnosti** dialogové okno, rozbalte položku **Azure** v levém podokně hello a klikněte na **vzdáleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-142">In hello **Properties** dialog, expand **Azure** in hello left-hand pane and click **Remote Access**.</span></span>

3. <span data-ttu-id="d9c9d-143">V hello **vzdáleného přístupu** dialogové okno, ujistěte se, **povolit všechny role tooaccept připojení vzdálené plochy s tyto přihlašovací údaje** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-143">In hello **Remote Access** dialog, ensure **Enable all roles tooaccept Remote Desktop Connections with these login credentials** is checked.</span></span>

4. <span data-ttu-id="d9c9d-144">Zadejte uživatelské jméno pro připojení ke vzdálené ploše hello.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-144">Specify a user name for hello Remote Desktop connection.</span></span>

5. <span data-ttu-id="d9c9d-145">Zadejte a potvrďte heslo hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-145">Specify and confirm hello password for hello user.</span></span> <span data-ttu-id="d9c9d-146">Hello uživatelské jméno a heslo hodnoty nastavení v tomto dialogu se použije při vytváření připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-146">hello user name and password values set in this dialog will be used when you make a Remote Desktop connection.</span></span> <span data-ttu-id="d9c9d-147">(Všimněte si, že se jedná o samostatný heslo z PFX heslo.)</span><span class="sxs-lookup"><span data-stu-id="d9c9d-147">(Note that this is a separate password from your PFX password.)</span></span>

6. <span data-ttu-id="d9c9d-148">Zadejte datum vypršení platnosti hello hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-148">Specify hello expiration date for hello user account.</span></span>

7. <span data-ttu-id="d9c9d-149">Klikněte na tlačítko **nový** toocreate nový certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-149">Click **New** toocreate a new self-signed certificate.</span></span> <span data-ttu-id="d9c9d-150">(Alternativně můžete třeba vybrat certifikát z vašeho pracovního prostoru nebo souboru systému prostřednictvím hello **prostoru** nebo **FileSystem** tlačítka, v uvedeném pořadí, ale pro účely tohoto kurzu vytvoříme novou certifikát.)</span><span class="sxs-lookup"><span data-stu-id="d9c9d-150">(Alternatively, you could select a certificate from your workspace or file system through hello **Workspace** or **FileSystem** buttons, respectively, but for purposes of this tutorial we'll create a new certificate.)</span></span>

   * <span data-ttu-id="d9c9d-151">V hello **nový certifikát** dialogové okno, zadejte a potvrďte heslo hello budete používat pro soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-151">In hello **New Certificate** dialog, specify and confirm hello password you'll use for your PFX file.</span></span>

   * <span data-ttu-id="d9c9d-152">Přijměte hello hodnota zadaná pro **název (CN)**, nebo použít vlastní název.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-152">Accept hello value provided for **Name (CN)**, or use a custom name.</span></span>

   * <span data-ttu-id="d9c9d-153">Zadejte název a cesta k souboru hello hello nový certifikát v podobě .cer, kam bude uložena.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-153">Specify hello path and file name where hello new certificate, in .cer form, will be saved.</span></span> <span data-ttu-id="d9c9d-154">Pro tento krok a další krok text hello, můžete použít hello **cert** složce projektu Azure, ale máte volné toochoose jiné umístění.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-154">For this step and hello next step, you could use hello **cert** folder of your Azure project, but you're free toochoose another location.</span></span> <span data-ttu-id="d9c9d-155">Pro účely tohoto kurzu použijeme **c:\mycert\mycert.cer**.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-155">For purposes of this tutorial, we'll use **c:\mycert\mycert.cer**.</span></span> <span data-ttu-id="d9c9d-156">(Vytvořte hello **c:\mycert** předchozí tooproceeding složky, nebo použijte existující složku v případě potřeby.)</span><span class="sxs-lookup"><span data-stu-id="d9c9d-156">(Create hello **c:\mycert** folder prior tooproceeding, or use an existing folder if desired.)</span></span>

   * <span data-ttu-id="d9c9d-157">Zadejte název a cesta k souboru hello uložení hello nový certifikát a jeho privátní klíč ve formátu .pfx.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-157">Specify hello path and file name where hello new certificate and its private key, in .pfx form, will be saved.</span></span> <span data-ttu-id="d9c9d-158">Pro účely tohoto kurzu použijeme **c:\mycert\mycert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-158">For purposes of this tutorial, we'll use **c:\mycert\mycert.pfx**.</span></span> <span data-ttu-id="d9c9d-159">Vaše **nový certifikát** dialogové okno by měl vypadat podobně jako následující toohello (aktualizace cesty ke složce zadat hello, pokud jste nepoužili **c:\mycert**):</span><span class="sxs-lookup"><span data-stu-id="d9c9d-159">Your **New Certificate** dialog should look similar toohello following (update hello folder paths if you did not use **c:\mycert**):</span></span>
     
      ![][ic712275]

   * <span data-ttu-id="d9c9d-160">Klikněte na tlačítko **OK** tooclose hello **nový certifikát** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-160">Click **OK** tooclose hello **New Certificate** dialog.</span></span>

8. <span data-ttu-id="d9c9d-161">Vaše **vzdáleného přístupu** dialogové okno by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="d9c9d-161">Your **Remote Access** dialog should look similar toohello following:</span></span></p>
   
   ![][ic719495]

9. <span data-ttu-id="d9c9d-162">Klikněte na tlačítko **OK** tooclose hello **vzdáleného přístupu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-162">Click **OK** tooclose hello **Remote Access** dialog.</span></span>

<span data-ttu-id="d9c9d-163">Sestavení aplikace, s hello sestavení sady pro toocloud nasazení.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-163">Rebuild your application, with hello build set for deployment toocloud.</span></span>

## <a name="toolog-in-remotely"></a><span data-ttu-id="d9c9d-164">toolog v vzdáleně</span><span class="sxs-lookup"><span data-stu-id="d9c9d-164">toolog in remotely</span></span>
<span data-ttu-id="d9c9d-165">Jakmile vaši instanci role je připraven, můžete vzdáleně přihlásit toohello virtuální počítač, který je hostitelem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-165">Once your role instance is ready, you can remotely log in toohello virtual machine that is hosting your application.</span></span>

* <span data-ttu-id="d9c9d-166">Pokud používáte Eclipse v systémech Windows a je vybraný hello **spuštění vzdálené plochy na nasazení** možnost během tooAzure vaše nasazení, zobrazí se přihlašovací obrazovka připojení ke vzdálené ploše při spuštění vašeho nasazení.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-166">If are using Eclipse on Windows and you selected hello **Start remote desktop on deploy** option during your deployment tooAzure, you will be presented with a Remote Desktop Connection logon screen when your deployment starts.</span></span> <span data-ttu-id="d9c9d-167">Po zobrazení výzvy k hello uživatelského jména a hesla, zadejte hello hodnoty, které jste zadali pro hello vzdáleného uživatele a bude moct toolog v.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-167">When you are prompted for hello user name and password, enter hello values that you specified for hello remote user and will be able toolog in.</span></span>

* <span data-ttu-id="d9c9d-168">Jiný způsob toolog v vzdáleně je prostřednictvím hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">portálu pro správu Azure</a>:</span><span class="sxs-lookup"><span data-stu-id="d9c9d-168">Another way toolog in remotely is through hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:</span></span>
  
  * <span data-ttu-id="d9c9d-169">V rámci hello **cloudové služby** zobrazení hello portálu pro správu Azure, klikněte na tlačítko cloudové služby, klikněte na tlačítko **instance**, klikněte na konkrétní instanci a pak klikněte na tlačítko hello **připojit**tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-169">Within hello **Cloud Services** view of hello Azure Management portal, click your cloud service, click **Instances**, click a specific instance, and then click hello **Connect** button.</span></span> <span data-ttu-id="d9c9d-170">Hello **Connect** tlačítko se zobrazí jako následující hello v řádku nabídek hello:</span><span class="sxs-lookup"><span data-stu-id="d9c9d-170">hello **Connect** button appears as hello following in hello command bar:</span></span>
    
      ![][ic659273]

  * <span data-ttu-id="d9c9d-171">Po kliknutí na hello **Connect** tlačítko bude výzvami tooopen soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-171">After clicking hello **Connect** button, you will be prompted tooopen an RDP file.</span></span> <span data-ttu-id="d9c9d-172">Otevřete soubor hello a budete postupovat podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-172">Open hello file and follow hello prompts.</span></span> <span data-ttu-id="d9c9d-173">(Můžete také uložit tento soubor tooyour místní počítač a spusťte soubor hello poklepáním tooremote protokolu v tooyour virtuálního počítače bez nutnosti toofirst přejděte hello portálu pro správu.)</span><span class="sxs-lookup"><span data-stu-id="d9c9d-173">(You could also save this file tooyour local computer, and then run hello file by double-clicking it tooremote log in tooyour virtual machine without needing toofirst go hello management portal.)</span></span>

  * <span data-ttu-id="d9c9d-174">Po zobrazení výzvy k hello uživatelského jména a hesla, zadejte hello hodnoty, které jste zadali pro hello vzdáleného uživatele a bude moct toolog v.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-174">When you are prompted for hello user name and password, enter hello values that you specified for hello remote user and will be able toolog in.</span></span>

> [!NOTE]
> <span data-ttu-id="d9c9d-175">Pokud jste na jiný systém než Windows operační systém, budete potřebovat toouse klienta vzdálené plochy, který je kompatibilní s operačním systémem a postupujte podle kroků tooconfigure hello tohoto klienta s hello nastavení v souboru RDP hello, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="d9c9d-175">If you are on a non-Windows operating system, you need toouse a Remote Desktop client that is compatible with your operating system and follow hello steps tooconfigure that client with hello settings in hello RDP file that you downloaded.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="d9c9d-176">Viz také</span><span class="sxs-lookup"><span data-stu-id="d9c9d-176">See Also</span></span>
<span data-ttu-id="d9c9d-177">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d9c9d-177">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="d9c9d-178">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d9c9d-178">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="d9c9d-179">[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d9c9d-179">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="d9c9d-180">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="d9c9d-180">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
