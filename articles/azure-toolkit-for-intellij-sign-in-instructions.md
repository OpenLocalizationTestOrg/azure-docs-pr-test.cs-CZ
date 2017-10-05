---
title: "Přihlášení pokyny pro Azure Toolkit IntelliJ | Microsoft Docs"
description: "Zjistěte, jak se přihlásit do služby Microsoft Azure pomocí sady nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: 4e2ed072bdaea0a71fef042c0c72b7656a42bbe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="534d4-103">Přihlášení pokyny pro Azure Toolkit IntelliJ</span><span class="sxs-lookup"><span data-stu-id="534d4-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="534d4-104">Sada nástrojů Azure pro IntelliJ nabízí dvě metody pro přihlášení k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="534d4-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="534d4-105">**Interaktivní**: Zadejte vaše přihlašovací údaje Azure při každém přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="534d4-105">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>
  * <span data-ttu-id="534d4-106">**Automatizované**: Vytvořte soubor přihlašovacích údajů, který můžete použít automatické přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="534d4-106">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>

<span data-ttu-id="534d4-107">Následující části popisují způsob použití každé metody.</span><span class="sxs-lookup"><span data-stu-id="534d4-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="534d4-108">Interaktivní přihlášení k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="534d4-108">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="534d4-109">Přihlaste se k Azure tak, že ručně zadáte vaše přihlašovací údaje Azure, takto:</span><span class="sxs-lookup"><span data-stu-id="534d4-109">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="534d4-110">Otevřete projekt s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="534d4-110">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="534d4-111">Klikněte na tlačítko **nástroje**, přejděte na příkaz **Azure**a potom klikněte na **přihlásit k Azure**.</span><span class="sxs-lookup"><span data-stu-id="534d4-111">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Příkaz IntelliJ Azure Sign In][I01]

3. <span data-ttu-id="534d4-113">V **přihlásit k Azure** vyberte **interaktivní**a potom klikněte na **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="534d4-113">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![Okno Azure přihlásit s interaktivní vybrané][I02]

4. <span data-ttu-id="534d4-115">V **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="534d4-115">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Okno dialogovém okně pro přihlášení k Azure][I03]

5. <span data-ttu-id="534d4-117">V **vyberte odběry** dialogové okno Vyberte předplatné, které chcete použít a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="534d4-117">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogové okno Vybrat odběrů][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="534d4-119">Po registraci interaktivním se odhlásit z účtu Azure</span><span class="sxs-lookup"><span data-stu-id="534d4-119">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="534d4-120">Po dokončení konfigurace účtu pomocí předchozího postupu automaticky odhlásíte se z účtu Azure pokaždé, když je restartovat IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="534d4-120">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="534d4-121">Ale pokud se chcete odhlásit z účtu Azure *bez* restartování IntelliJ IDEA, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="534d4-121">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="534d4-122">V IntelliJ IDEA na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **Azure Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="534d4-122">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Příkaz IntelliJ Azure odhlásit][L01]

2. <span data-ttu-id="534d4-124">V **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="534d4-124">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Okno potvrzení Azure odhlásit][L02]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="534d4-126">Přihlaste se k účtu Azure automaticky</span><span class="sxs-lookup"><span data-stu-id="534d4-126">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="534d4-127">Tato část vás provede procesem vytváření pověření souboru, který obsahuje hlavní data služby.</span><span class="sxs-lookup"><span data-stu-id="534d4-127">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="534d4-128">Po dokončení tohoto procesu se používá Eclipse automaticky pro přihlášení do Azure při každém otevření projektu soubor s přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="534d4-128">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="534d4-129">Otevřete projekt s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="534d4-129">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="534d4-130">Na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **přihlásit k Azure**.</span><span class="sxs-lookup"><span data-stu-id="534d4-130">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Příkaz IntelliJ Azure Sign In][A01]

3. <span data-ttu-id="534d4-132">V **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="534d4-132">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![Okno Azure přihlásit s automatizovaná vybrané][A02]

4. <span data-ttu-id="534d4-134">V **dialogovém okně pro přihlášení k Azure** okno, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="534d4-134">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Okno dialogovém okně pro přihlášení k Azure][A03]

5. <span data-ttu-id="534d4-136">V **vytvářet soubory ověřování** okně vyberte předplatné, které chcete použít, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="534d4-136">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Okno vytvořit soubory ověřování][A04]

6. <span data-ttu-id="534d4-138">V **stav vytváření objektu služby** dialogové okno, jakmile vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="534d4-138">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![Dialogové okno Stav vytvoření instančního objektu][A05]

7. <span data-ttu-id="534d4-140">V **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="534d4-140">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A06]

8. <span data-ttu-id="534d4-142">V **vyberte odběry** dialogové okno Vyberte předplatné, které chcete použít a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="534d4-142">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogové okno Vybrat odběrů][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="534d4-144">Po přihlášení automaticky se odhlásit z účtu Azure</span><span class="sxs-lookup"><span data-stu-id="534d4-144">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="534d4-145">Po dokončení konfigurace účtu pomocí předchozího postupu nástrojů Azure automaticky přihlásí můžete ke svému účtu Azure pokaždé, když je restartovat IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="534d4-145">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="534d4-146">Ale pokud chcete odhlásit z účtu Azure a zabránit automatickému přihlašování nástrojů Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="534d4-146">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="534d4-147">V IntelliJ IDEA na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **Azure Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="534d4-147">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Příkaz IntelliJ Azure odhlásit][L01]

2. <span data-ttu-id="534d4-149">V **Azure Odhlásit** okno pro potvrzení, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="534d4-149">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Okno potvrzení Azure odhlásit][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="534d4-151">Přihlásit k účtu Azure automaticky pomocí existující soubor přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="534d4-151">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="534d4-152">Pokud podepíšete mimo účtu Azure, když používáte IntelliJ IDEA, musíte použít existující soubor přihlašovacích údajů se automaticky znovu přihlásit k účtu.</span><span class="sxs-lookup"><span data-stu-id="534d4-152">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="534d4-153">Konfigurace sady nástrojů Azure pro prostředí Eclipse, aby používal existující soubor přihlašovacích údajů, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="534d4-153">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="534d4-154">Otevřete projekt s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="534d4-154">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="534d4-155">Na **nástroje** nabídky, přejděte na příkaz **Azure**a potom klikněte na **přihlásit k Azure**.</span><span class="sxs-lookup"><span data-stu-id="534d4-155">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Příkaz IntelliJ Azure Sign In][A01]

3. <span data-ttu-id="534d4-157">V **přihlásit k Azure** vyberte **automatizovaná**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="534d4-157">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![Okno Azure přihlásit s automatizovaná vybrané][A02]

4. <span data-ttu-id="534d4-159">V **vyberte soubor pro ověření** dialogové okno, vyberte soubor dříve vytvořenou přihlašovací údaje a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="534d4-159">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![Vyberte soubor pro ověření dialogových oken][A08]

5. <span data-ttu-id="534d4-161">V **přihlásit k Azure** okně klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="534d4-161">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Okno Azure přihlásit s automatizovaná vybrané][A06]

6. <span data-ttu-id="534d4-163">V **vyberte odběry** dialogové okno Vyberte předplatné, které chcete použít a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="534d4-163">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogové okno Vybrat odběrů][A07]

## <a name="next-steps"></a><span data-ttu-id="534d4-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="534d4-165">Next steps</span></span>
<span data-ttu-id="534d4-166">Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="534d4-166">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="534d4-167">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="534d4-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="534d4-168">[Co je nového v sadě nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="534d4-168">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="534d4-169">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="534d4-169">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="534d4-170">[Pokyny přihlášení k Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="534d4-170">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="534d4-171">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="534d4-171">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="534d4-172">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="534d4-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="534d4-173">[Co je nového v sadě nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="534d4-173">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="534d4-174">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="534d4-174">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="534d4-175">*Přihlášení pokyny pro Azure Toolkit IntelliJ* (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="534d4-175">*Sign-in instructions for the Azure Toolkit for IntelliJ* (this article)</span></span>
  * <span data-ttu-id="534d4-176">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="534d4-176">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="534d4-177">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="534d4-177">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="534d4-178">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="534d4-178">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="534d4-179">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="534d4-179">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="534d4-180">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="534d4-180">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="534d4-181">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="534d4-181">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="534d4-182">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="534d4-182">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="534d4-183">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="534d4-183">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="534d4-184">[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="534d4-184">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
<span data-ttu-id="534d4-185">[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="534d4-185">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="534d4-186">[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="534d4-186">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="534d4-187">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="534d4-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="534d4-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="534d4-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

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
