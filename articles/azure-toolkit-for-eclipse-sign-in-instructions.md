---
title: "aaaSign v pokyny pro hello nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak hello toosign do Microsoft Azure pomocí nástrojů Azure pro prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 95be64750ca0147f76dae8f364fad80cb9ccc969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sign-in-instructions-for-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="a8b98-103">Azure přihlášení v pokyny pro hello nástrojů Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="a8b98-103">Azure Sign In Instructions for hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="a8b98-104">Hello nástrojů Azure pro prostředí Eclipse nabízí dvě metody pro přihlášení k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="a8b98-104">hello Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="a8b98-105">**Interaktivní** – při použití této metody bude zadejte vaše přihlašovací údaje Azure pokaždé, když jste přihlášení ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a8b98-105">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>
  * <span data-ttu-id="a8b98-106">**Automatizované** – při použití této metody vytvoříte soubor přihlašovacích údajů, který obsahuje hlavní data vaší služby, po které můžete použít hello přihlašovací údaje souboru tooautomatically přihlášení ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a8b98-106">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use hello credentials file tooautomatically sign into your Azure account.</span></span>

<span data-ttu-id="a8b98-107">Hello kroky v hello následující části se popisují, jak toouse každá z metod.</span><span class="sxs-lookup"><span data-stu-id="a8b98-107">hello steps in hello following sections will describe how toouse each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="a8b98-108">Interaktivní přihlášení k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="a8b98-108">Signing into your Azure account interactively</span></span>

<span data-ttu-id="a8b98-109">Hello následující kroky ilustruje způsob toosign do Azure tak, že ručně zadáte vaše přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="a8b98-109">hello following steps will illustrate how toosign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="a8b98-110">Otevřete projekt s Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a8b98-110">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="a8b98-111">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse nabídky pro Azure přihlášení][I01]

1. <span data-ttu-id="a8b98-113">Když hello **přihlásit k Azure** se zobrazí dialogové okno, vyberte **interaktivní**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-113">When hello **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![Přihlaste se dialogové okno][I02]

1. <span data-ttu-id="a8b98-115">Když hello **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-115">When hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][I03]

1. <span data-ttu-id="a8b98-117">Když hello **vyberte odběry** zobrazí se dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-117">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Vyberte předplatné, dialogové okno][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="a8b98-119">Podepisování mimo účtu Azure, když jste přihlášení interaktivně</span><span class="sxs-lookup"><span data-stu-id="a8b98-119">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="a8b98-120">Po nakonfigurování hello kroky v předchozí části hello budete automaticky odhlášení ze účtu Azure při každém restartování prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a8b98-120">After you have configured hello steps in hello previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="a8b98-121">Pokud chcete toosign mimo účtu Azure bez restartování prostředí Eclipse, ale použijte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="a8b98-121">However, if you want toosign out of your Azure account without restarting Eclipse, use hello following steps.</span></span>

1. <span data-ttu-id="a8b98-122">V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-122">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. <span data-ttu-id="a8b98-124">Když hello **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-124">When hello **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Odhlásit se dialogové okno][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-toouse-in-hello-future"></a><span data-ttu-id="a8b98-126">Přihlášení k účtu Azure automaticky a vytváření přihlašovacích údajů souboru toouse v hello budoucí</span><span class="sxs-lookup"><span data-stu-id="a8b98-126">Signing into your Azure account automatically and creating a credentials file toouse in hello future</span></span>

<span data-ttu-id="a8b98-127">Hello následující postup vás provede vytváření pověření souboru, který obsahuje hlavní data služby.</span><span class="sxs-lookup"><span data-stu-id="a8b98-127">hello following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="a8b98-128">Po dokončení těchto kroků, Eclipse bude automaticky použití hello přihlašovací údaje souboru tooautomatically přihlášení, že jste do Azure každý času jste otevřete projekt.</span><span class="sxs-lookup"><span data-stu-id="a8b98-128">Once you have completed these steps, Eclipse will automatically use hello credentials file tooautomatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="a8b98-129">Otevřete projekt s Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a8b98-129">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="a8b98-130">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-130">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. <span data-ttu-id="a8b98-132">Když hello **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-132">When hello **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![Přihlaste se dialogové okno][A02]

1. <span data-ttu-id="a8b98-134">Když hello **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-134">When hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A03]

1. <span data-ttu-id="a8b98-136">Když hello **vytvoření souborů ověřování** zobrazí se dialogové okno, vyberte hello odběry mají toouse, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-136">When hello **Create authentication files** dialog box appears, select hello subscriptions that you want toouse, choose your destination directory, and then click **Start**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A04]

1. <span data-ttu-id="a8b98-138">Hello **instanční objekt Creatation stav** zobrazí se dialogové okno a poté, co vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-138">hello **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![Dialogové okno Stav Creatation instančního objektu][A05]

1. <span data-ttu-id="a8b98-140">Když hello **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-140">When hello **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A06]

1. <span data-ttu-id="a8b98-142">Když hello **vyberte odběry** zobrazí se dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-142">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="a8b98-144">Podepisování mimo účtu Azure, když jste přihlášení automaticky</span><span class="sxs-lookup"><span data-stu-id="a8b98-144">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="a8b98-145">Po nakonfigurování hello kroky v předchozí části hello hello nástrojů Azure automaticky podepíše můžete ke svému účtu Azure při každém restartování prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a8b98-145">After you have configured hello steps in hello previous section, hello Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="a8b98-146">Ale toosign mimo účtu Azure a zabránit hello nástrojů Azure automaticky hello použijte následující kroky vás přihlásit.</span><span class="sxs-lookup"><span data-stu-id="a8b98-146">However, toosign out of your Azure account and prevent hello Azure Toolkit from signing you in automatically, use hello following steps.</span></span>

1. <span data-ttu-id="a8b98-147">V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-147">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. <span data-ttu-id="a8b98-149">Když hello **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-149">When hello **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Odhlásit se dialogové okno][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="a8b98-151">Přihlášení k účtu Azure automaticky pomocí pověření souboru, který jste již vytvořili</span><span class="sxs-lookup"><span data-stu-id="a8b98-151">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="a8b98-152">Pokud podepíšete mimo Azure, pokud používáte Eclipse, je nutné tooreconfigure hello Azure Toolkit pro Eclipse toouse soubor přihlašovacích údajů, který jste vytvořili předtím, než můžete automaticky přihlásit do váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a8b98-152">If you sign out of Azure when you are using Eclipse, you will need tooreconfigure hello Azure Toolkit for Eclipse toouse a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="a8b98-153">Hello následující postup vás provede konfiguraci hello Azure Toolkit toouse existující soubor přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="a8b98-153">hello following steps will walk you through configuring hello Azure Toolkit toouse an existing credentials file.</span></span>

1. <span data-ttu-id="a8b98-154">Otevřete projekt s Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a8b98-154">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="a8b98-155">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-155">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. <span data-ttu-id="a8b98-157">Když hello **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-157">When hello **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![Přihlaste se dialogové okno][A02]

1. <span data-ttu-id="a8b98-159">Když hello **vyberte ověřit soubor** dialogové okno se zobrazí, vyberte soubor přihlašovacích údajů, který jste předtím vytvořili a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-159">When hello **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![Přihlaste se dialogové okno][A08]

1. <span data-ttu-id="a8b98-161">Když hello **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-161">When hello **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A06]

1. <span data-ttu-id="a8b98-163">Když hello **vyberte odběry** zobrazí se dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8b98-163">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="see-also"></a><span data-ttu-id="a8b98-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="a8b98-165">See Also</span></span>
<span data-ttu-id="a8b98-166">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="a8b98-166">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="a8b98-167">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a8b98-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a8b98-168">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a8b98-168">[What's New in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a8b98-169">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a8b98-169">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a8b98-170">*Přihlášení v pokyny pro hello nástrojů Azure pro Eclipse (v tomto článku)*</span><span class="sxs-lookup"><span data-stu-id="a8b98-170">*Sign In Instructions for hello Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="a8b98-171">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a8b98-171">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="a8b98-172">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a8b98-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a8b98-173">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a8b98-173">[What's New in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a8b98-174">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a8b98-174">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a8b98-175">[Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a8b98-175">[Sign In Instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a8b98-176">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a8b98-176">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="a8b98-177">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="a8b98-177">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení v pokyny pro hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
