---
title: aaaSign v pokyny pro hello Azure Toolkit IntelliJ | Microsoft Docs
description: "Zjistěte, jak hello toosign v tooMicrosoft Azure pomocí Azure Toolkit pro IntelliJ."
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
ms.openlocfilehash: 2de781fc19267cce133b1e6456481497e165fce4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-instructions-for-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="01eab-103">Přihlášení pokyny pro hello Azure Toolkit IntelliJ</span><span class="sxs-lookup"><span data-stu-id="01eab-103">Sign-in instructions for hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="01eab-104">Hello nástrojů Azure pro IntelliJ nabízí dvě metody pro přihlášení tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="01eab-104">hello Azure Toolkit for IntelliJ provides two methods for signing in tooyour Azure account:</span></span>

  * <span data-ttu-id="01eab-105">**Interaktivní**: zadáte pokaždé, když jste přihlášení pro přihlašovací údaje Azure v tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="01eab-105">**Interactive**: You enter your Azure credentials each time you sign in tooyour Azure account.</span></span>
  * <span data-ttu-id="01eab-106">**Automatizované**: vytvoření souboru přihlašovací údaje používané přihlašovací tooautomatically v tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="01eab-106">**Automated**: You create a credentials file that you can use tooautomatically sign in tooyour Azure account.</span></span>

<span data-ttu-id="01eab-107">Hello následující části popisují, jak toouse každá z metod.</span><span class="sxs-lookup"><span data-stu-id="01eab-107">hello following sections describe how toouse each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-tooyour-azure-account-interactively"></a><span data-ttu-id="01eab-108">Interaktivní přihlášení tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="01eab-108">Sign in tooyour Azure account interactively</span></span>

<span data-ttu-id="01eab-109">toosign v tooAzure ručně zadáním vaše přihlašovací údaje Azure hello následující:</span><span class="sxs-lookup"><span data-stu-id="01eab-109">toosign in tooAzure by manually entering your Azure credentials, do hello following:</span></span>

1. <span data-ttu-id="01eab-110">Otevřete projekt s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="01eab-110">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="01eab-111">Klikněte na tlačítko **nástroje**, bod příliš**Azure**a potom klikněte na **přihlásit k Azure**.</span><span class="sxs-lookup"><span data-stu-id="01eab-111">Click **Tools**, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![Hello příkaz IntelliJ Azure Sign In][I01]

3. <span data-ttu-id="01eab-113">V hello **přihlásit k Azure** vyberte **interaktivní**a potom klikněte na **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="01eab-113">In hello **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![Hello přihlásit k Azure okno s interaktivní vybrané][I02]

4. <span data-ttu-id="01eab-115">V hello **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="01eab-115">In hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Hello Azure přihlášení dialogového okna][I03]

5. <span data-ttu-id="01eab-117">V hello **vyberte odběry** dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01eab-117">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Dialogové okno Vybrat odběry Hello][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="01eab-119">Po registraci interaktivním se odhlásit z účtu Azure</span><span class="sxs-lookup"><span data-stu-id="01eab-119">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="01eab-120">Po dokončení konfigurace účtu pomocí hello předchozích kroků automaticky odhlásíte se z účtu Azure pokaždé, když je restartovat IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="01eab-120">After you have configured your account by using hello preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="01eab-121">Ale pokud chcete toosign mimo účtu Azure *bez* restartování IntelliJ IDEA hello následující.</span><span class="sxs-lookup"><span data-stu-id="01eab-121">However, if you want toosign out of your Azure account *without* restarting IntelliJ IDEA, do hello following.</span></span>

1. <span data-ttu-id="01eab-122">V IntelliJ IDEA na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **Azure Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="01eab-122">In IntelliJ IDEA, on hello **Tools** menu, point too**Azure**, and then click **Azure Sign Out**.</span></span>

   ![Hello IntelliJ Azure přihlašovací Vystoupením – příkaz][L01]

2. <span data-ttu-id="01eab-124">V hello **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="01eab-124">In hello **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Hello Azure odhlásit potvrzovacím okně][L02]

## <a name="sign-in-tooyour-azure-account-automatically"></a><span data-ttu-id="01eab-126">Automaticky přihlásit tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="01eab-126">Sign in tooyour Azure account automatically</span></span>

<span data-ttu-id="01eab-127">Tato část vás provede procesem vytváření pověření souboru, který obsahuje hlavní data služby.</span><span class="sxs-lookup"><span data-stu-id="01eab-127">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="01eab-128">Po dokončení tohoto procesu, otevřete prostředí Eclipse používá hello přihlašovací údaje souboru tooautomatically přihlášení, že v každém tooAzure času jste projekt.</span><span class="sxs-lookup"><span data-stu-id="01eab-128">After you have completed this process, Eclipse uses hello credentials file tooautomatically sign you in tooAzure each time you open your project.</span></span>

1. <span data-ttu-id="01eab-129">Otevřete projekt s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="01eab-129">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="01eab-130">Na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **přihlásit k Azure**.</span><span class="sxs-lookup"><span data-stu-id="01eab-130">On hello **Tools** menu, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![Hello příkaz IntelliJ Azure Sign In][A01]

3. <span data-ttu-id="01eab-132">V hello **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="01eab-132">In hello **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![Hello přihlásit k Azure okno s automatizovaná vybrané][A02]

4. <span data-ttu-id="01eab-134">V hello **dialogovém okně pro přihlášení k Azure** okno, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="01eab-134">In hello **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Hello Azure přihlášení dialogového okna][A03]

5. <span data-ttu-id="01eab-136">V hello **vytvářet soubory ověřování** okno, vyberte hello odběry mají toouse, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="01eab-136">In hello **Create Authentication Files** window, select hello subscriptions that you want toouse, choose your destination directory, and then click **Start**.</span></span>

   ![okno vytvořit soubory ověřování Hello][A04]

6. <span data-ttu-id="01eab-138">V hello **stav vytváření objektu služby** dialogové okno, jakmile vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01eab-138">In hello **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![Hello dialogové okno Stav vytvoření instančního objektu][A05]

7. <span data-ttu-id="01eab-140">V hello **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="01eab-140">In hello **Azure Sign In** window, click **Sign in**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A06]

8. <span data-ttu-id="01eab-142">V hello **vyberte odběry** dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01eab-142">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Dialogové okno Vybrat odběry Hello][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="01eab-144">Po přihlášení automaticky se odhlásit z účtu Azure</span><span class="sxs-lookup"><span data-stu-id="01eab-144">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="01eab-145">Po dokončení konfigurace účtu pomocí hello předchozích kroků hello nástrojů Azure automaticky přihlašovat při tooyour pokaždé, když je restartovat IntelliJ IDEA účet Azure.</span><span class="sxs-lookup"><span data-stu-id="01eab-145">After you have configured your account by using hello preceding steps, hello Azure Toolkit automatically signs you in tooyour Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="01eab-146">Ale toosign mimo účtu Azure a zabránit hello nástrojů Azure z vás přihlásit automaticky, hello následující:</span><span class="sxs-lookup"><span data-stu-id="01eab-146">However, toosign out of your Azure account and prevent hello Azure Toolkit from signing you in automatically, do hello following:</span></span>

1. <span data-ttu-id="01eab-147">V IntelliJ IDEA na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **Azure Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="01eab-147">In IntelliJ IDEA, on hello **Tools** menu, point too**Azure**, and then click **Azure Sign Out**.</span></span>

   ![Hello IntelliJ Azure přihlašovací Vystoupením – příkaz][L01]

2. <span data-ttu-id="01eab-149">V hello **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="01eab-149">In hello **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Hello Azure odhlásit potvrzovacím okně][L03]

## <a name="sign-in-tooyour-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="01eab-151">Přihlaste se tooyour účtu Azure automaticky pomocí existující soubor přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="01eab-151">Sign in tooyour Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="01eab-152">Pokud podepíšete mimo účtu Azure, když používáte IntelliJ IDEA, je nutné použít symbolem tooautomatically existující soubor přihlašovacích údajů zpět v toohello účtu.</span><span class="sxs-lookup"><span data-stu-id="01eab-152">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file tooautomatically sign back in toohello account.</span></span> <span data-ttu-id="01eab-153">tooconfigure hello Azure Toolkit pro Eclipse toouse existující soubor přihlašovacích údajů, hello následující:</span><span class="sxs-lookup"><span data-stu-id="01eab-153">tooconfigure hello Azure Toolkit for Eclipse toouse an existing credentials file, do hello following:</span></span>

1. <span data-ttu-id="01eab-154">Otevřete projekt s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="01eab-154">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="01eab-155">Na hello **nástroje** nabídce bodu příliš**Azure**a potom klikněte na **přihlásit k Azure**.</span><span class="sxs-lookup"><span data-stu-id="01eab-155">On hello **Tools** menu, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![Hello příkaz IntelliJ Azure Sign In][A01]

3. <span data-ttu-id="01eab-157">V hello **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="01eab-157">In hello **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![Hello přihlásit k Azure okno s automatizovaná vybrané][A02]

4. <span data-ttu-id="01eab-159">V hello **vyberte soubor pro ověření** dialogové okno, vyberte soubor dříve vytvořenou přihlašovací údaje a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="01eab-159">In hello **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![Dialogové okno Vybrat soubor pro ověření Hello][A08]

5. <span data-ttu-id="01eab-161">V hello **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="01eab-161">In hello **Azure Sign In** window, click **Sign in**.</span></span>

   ![Hello přihlásit k Azure okno s automatizovaná vybrané][A06]

6. <span data-ttu-id="01eab-163">V hello **vyberte odběry** dialogové okno, vyberte hello odběry mají toouse a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="01eab-163">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Dialogové okno Vybrat odběry Hello][A07]

## <a name="next-steps"></a><span data-ttu-id="01eab-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01eab-165">Next steps</span></span>
<span data-ttu-id="01eab-166">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="01eab-166">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="01eab-167">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="01eab-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="01eab-168">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="01eab-168">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="01eab-169">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="01eab-169">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="01eab-170">[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="01eab-170">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="01eab-171">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="01eab-171">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="01eab-172">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="01eab-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="01eab-173">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="01eab-173">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="01eab-174">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="01eab-174">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="01eab-175">*Přihlášení pokyny pro hello Azure Toolkit IntelliJ* (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="01eab-175">*Sign-in instructions for hello Azure Toolkit for IntelliJ* (this article)</span></span>
  * <span data-ttu-id="01eab-176">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="01eab-176">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="01eab-177">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="01eab-177">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for hello Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure středisko pro vývojáře v jazyce Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
