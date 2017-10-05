---
title: "Přihlaste se pokyny pro Azure nástrojů Eclipse | Microsoft Docs"
description: "Zjistěte, jak se přihlásit ke službě Microsoft Azure pomocí sady nástrojů Azure pro Eclipse."
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
ms.openlocfilehash: 02dd9935086c4c40d9ed54cc9ff2412ca96889f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="80179-103">Azure přihlášení pokyny pro Azure Toolkit pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="80179-103">Azure Sign In Instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="80179-104">Sada nástrojů Azure pro Eclipse nabízí dvě metody pro přihlášení k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="80179-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="80179-105">**Interaktivní** – při použití této metody bude zadejte vaše přihlašovací údaje Azure pokaždé, když jste přihlášení ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="80179-105">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>
  * <span data-ttu-id="80179-106">**Automatizované** – při použití této metody vytvoříte soubor přihlašovacích údajů, který obsahuje hlavní data vaší služby, po které můžete použít soubor s přihlašovacími údaji se automaticky přihlásit ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="80179-106">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use the credentials file to automatically sign into your Azure account.</span></span>

<span data-ttu-id="80179-107">Kroky v následujících částech se popisují způsob použití každé metody.</span><span class="sxs-lookup"><span data-stu-id="80179-107">The steps in the following sections will describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="80179-108">Interaktivní přihlášení k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="80179-108">Signing into your Azure account interactively</span></span>

<span data-ttu-id="80179-109">Následující postup se ukazují, jak pro přihlášení do Azure tak, že ručně zadáte vaše přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="80179-109">The following steps will illustrate how to sign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="80179-110">Otevřete projekt s Eclipse.</span><span class="sxs-lookup"><span data-stu-id="80179-110">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="80179-111">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse nabídky pro Azure přihlášení][I01]

1. <span data-ttu-id="80179-113">Když **přihlásit k Azure** se zobrazí dialogové okno, vyberte **interaktivní**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-113">When the **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![Přihlaste se dialogové okno][I02]

1. <span data-ttu-id="80179-115">Když **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-115">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][I03]

1. <span data-ttu-id="80179-117">Když **vyberte odběry** se zobrazí dialogové okno, vyberte předplatné, které chcete použít a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="80179-117">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Vyberte předplatné, dialogové okno][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="80179-119">Podepisování mimo účtu Azure, když jste přihlášení interaktivně</span><span class="sxs-lookup"><span data-stu-id="80179-119">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="80179-120">Po konfiguraci podle kroků v předchozí části, budete automaticky odhlášení ze účtu Azure při každém restartování prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="80179-120">After you have configured the steps in the previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="80179-121">Pokud chcete odhlásit z účtu Azure bez restartování prostředí Eclipse, ale použijte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="80179-121">However, if you want to sign out of your Azure account without restarting Eclipse, use the following steps.</span></span>

1. <span data-ttu-id="80179-122">V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-122">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. <span data-ttu-id="80179-124">Když **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="80179-124">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Odhlásit se dialogové okno][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a><span data-ttu-id="80179-126">Přihlášení k účtu Azure automaticky a vytvoření souboru přihlašovací údaje používat v budoucnosti</span><span class="sxs-lookup"><span data-stu-id="80179-126">Signing into your Azure account automatically and creating a credentials file to use in the future</span></span>

<span data-ttu-id="80179-127">Následující kroky vás procesem vytvoření pověření souboru, který obsahuje hlavní data služby.</span><span class="sxs-lookup"><span data-stu-id="80179-127">The following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="80179-128">Po dokončení těchto kroků Eclipse automaticky používat soubor s přihlašovacími údaji automaticky pro přihlášení do Azure při každém otevření projektu.</span><span class="sxs-lookup"><span data-stu-id="80179-128">Once you have completed these steps, Eclipse will automatically use the credentials file to automatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="80179-129">Otevřete projekt s Eclipse.</span><span class="sxs-lookup"><span data-stu-id="80179-129">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="80179-130">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-130">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. <span data-ttu-id="80179-132">Když **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="80179-132">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![Přihlaste se dialogové okno][A02]

1. <span data-ttu-id="80179-134">Když **přihlásit k Azure** dialogové okno se zobrazí, zadejte přihlašovací údaje Azure a pak klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-134">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A03]

1. <span data-ttu-id="80179-136">Když **vytvoření ověřování souborů** se zobrazí dialogové okno, vyberte předplatné, které chcete použít, vyberte cílový adresář a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="80179-136">When the **Create authentication files** dialog box appears, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A04]

1. <span data-ttu-id="80179-138">**Instanční objekt Creatation stav** zobrazí se dialogové okno a poté, co vaše soubory byly úspěšně vytvořeny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="80179-138">The **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![Dialogové okno Stav Creatation instančního objektu][A05]

1. <span data-ttu-id="80179-140">Když **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-140">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A06]

1. <span data-ttu-id="80179-142">Když **vyberte odběry** se zobrazí dialogové okno, vyberte předplatné, které chcete použít a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="80179-142">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="80179-144">Podepisování mimo účtu Azure, když jste přihlášení automaticky</span><span class="sxs-lookup"><span data-stu-id="80179-144">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="80179-145">Po konfiguraci podle kroků v předchozí části, se můžete ke svému účtu Azure při každém restartování prostředí Eclipse přihlásit automaticky nástrojů Azure.</span><span class="sxs-lookup"><span data-stu-id="80179-145">After you have configured the steps in the previous section, the Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="80179-146">Pokud chcete odhlásit z účtu Azure a zabránit automatickému přihlašování nástrojů Azure, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="80179-146">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, use the following steps.</span></span>

1. <span data-ttu-id="80179-147">V prostředí Eclipse, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-147">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse nabídky pro Azure odhlášení][L01]

1. <span data-ttu-id="80179-149">Když **Azure Odhlásit** se zobrazí dialogové okno, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="80179-149">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Odhlásit se dialogové okno][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="80179-151">Přihlášení k účtu Azure automaticky pomocí pověření souboru, který jste již vytvořili</span><span class="sxs-lookup"><span data-stu-id="80179-151">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="80179-152">Pokud podepíšete mimo Azure, pokud používáte Eclipse, musíte znovu nakonfigurovat sady nástrojů Azure pro prostředí Eclipse, aby použít soubor přihlašovacích údajů, který jste vytvořili předtím, než můžete automaticky přihlásit do váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="80179-152">If you sign out of Azure when you are using Eclipse, you will need to reconfigure the Azure Toolkit for Eclipse to use a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="80179-153">Následující postup vás provede konfigurace sady nástrojů Azure používat existující soubor přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="80179-153">The following steps will walk you through configuring the Azure Toolkit to use an existing credentials file.</span></span>

1. <span data-ttu-id="80179-154">Otevřete projekt s Eclipse.</span><span class="sxs-lookup"><span data-stu-id="80179-154">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="80179-155">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Azure**a potom klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-155">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse nabídky pro Azure přihlášení][A01]

1. <span data-ttu-id="80179-157">Když **přihlásit k Azure** se zobrazí dialogové okno, vyberte **automatizovaná**a potom klikněte na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="80179-157">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![Přihlaste se dialogové okno][A02]

1. <span data-ttu-id="80179-159">Když **vyberte ověřit soubor** dialogové okno se zobrazí, vyberte soubor přihlašovacích údajů, který jste předtím vytvořili a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="80179-159">When the **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![Přihlaste se dialogové okno][A08]

1. <span data-ttu-id="80179-161">Když **přihlásit k Azure** se zobrazí dialogové okno, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="80179-161">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogové okno Přihlášení do Azure][A06]

1. <span data-ttu-id="80179-163">Když **vyberte odběry** se zobrazí dialogové okno, vyberte předplatné, které chcete použít a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="80179-163">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Vyberte předplatné, dialogové okno][A07]

## <a name="see-also"></a><span data-ttu-id="80179-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="80179-165">See Also</span></span>
<span data-ttu-id="80179-166">Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="80179-166">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="80179-167">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="80179-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="80179-168">[Novinky v sadě Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="80179-168">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="80179-169">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="80179-169">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="80179-170">*Přihlaste se pokyny pro Azure nástrojů Eclipse (v tomto článku)*</span><span class="sxs-lookup"><span data-stu-id="80179-170">*Sign In Instructions for the Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="80179-171">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="80179-171">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="80179-172">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="80179-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="80179-173">[Novinky v sadě Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="80179-173">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="80179-174">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="80179-174">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="80179-175">[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="80179-175">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="80179-176">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="80179-176">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="80179-177">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="80179-177">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="80179-178">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="80179-178">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="80179-179">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="80179-179">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="80179-180">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="80179-180">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="80179-181">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="80179-181">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="80179-182">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="80179-182">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="80179-183">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="80179-183">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
<span data-ttu-id="80179-184">[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="80179-184">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="80179-185">[Novinky v sadě Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="80179-185">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="80179-186">[Novinky v sadě Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="80179-186">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="80179-187">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="80179-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="80179-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="80179-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

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
