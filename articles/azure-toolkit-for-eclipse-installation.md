---
title: Instalace Azure Toolkit pro Eclipse | Microsoft Docs
description: "Informace o instalaci sady nástrojů Azure pro prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35cddba38c364dfb2f6a8646b0014d48ca4cb795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="f0b2e-103">Instalace Azure Toolkit pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="f0b2e-103">Installing the Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="f0b2e-104">Sada Azure pro Eclipse obsahuje šablony a funkce, které vám umožní snadno vytvářet, vývoj, testování a nasazení aplikace na platformě Azure pomocí vývojové prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the Eclipse development environment.</span></span> <span data-ttu-id="f0b2e-105">Sada nástrojů Azure pro Eclipse je projekt Open Source.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-105">The Azure Toolkit for Eclipse is an Open Source project.</span></span> <span data-ttu-id="f0b2e-106">Zdrojový kód je k dispozici v části licencí MIT z <https://github.com/microsoft/azure-tools-for-java>.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-106">The source code is available under the MIT License from <https://github.com/microsoft/azure-tools-for-java>.</span></span>

<span data-ttu-id="f0b2e-107">Následující kroky ukazují, jak nainstalovat sadu Azure pro Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="f0b2e-108">K instalaci sady nástrojů Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="f0b2e-108">To install the Azure Toolkit for Eclipse</span></span>
1. <span data-ttu-id="f0b2e-109">Spusťte Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-109">Start Eclipse.</span></span>
2. <span data-ttu-id="f0b2e-110">Klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **instalace nového softwaru**, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
    ![Instalace Azure Toolkit pro Eclipse][01]
3. <span data-ttu-id="f0b2e-112">V **dostupného softwaru** dialogové okno, v **pracovat s** textového pole, typ `http://dl.microsoft.com/eclipse` následuje **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse` followed by the **Enter** key.</span></span>
4. <span data-ttu-id="f0b2e-113">V **název** podokně zkontrolujte **nástrojů Azure pro Eclipse**a zrušte zaškrtnutí políčka **obraťte se na všechny lokality aktualizace při instalaci najít požadovaný software**.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="f0b2e-114">Na obrazovce by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="f0b2e-114">Your screen should appear similar to the following:</span></span>
   
    ![Instalace Azure Toolkit pro Eclipse][02]
5. <span data-ttu-id="f0b2e-116">Pokud rozbalíte **nástrojů Azure pro Eclipse**, zobrazí se následující položky:</span><span class="sxs-lookup"><span data-stu-id="f0b2e-116">If you expand the **Azure Toolkit for Eclipse**, you will see the following items:</span></span>
   
   * <span data-ttu-id="f0b2e-117">**Modul plug-in přehled aplikace pro jazyk Java**: Tato součást umožňuje používat služby protokolování a analýza telemetrie Azure pro vaše aplikace a instance serveru.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-117">**Application Insights Plugin for Java**: This component allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span>
   * <span data-ttu-id="f0b2e-118">**Azure filtru služeb řízení přístupu**: Tato součást poskytuje podporu pro ověřování uživatelů aplikace s Azure ACS, povolení scénářů přihlašování a externího logiku identity vně aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-118">**Azure Access Control Services Filter**: This component provides support for authenticating application users with Azure ACS, enabling single sign-on scenarios and externalizing identity logic from the application.</span></span>
   * <span data-ttu-id="f0b2e-119">**Modul plug-in Azure běžné**: Tato součást poskytuje běžné funkce vyžaduje další součásti sady toolkit.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-119">**Azure Common Plugin**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="f0b2e-120">**Průzkumník Azure pro Eclipse**: Tato součást poskytuje běžné funkce vyžaduje další součásti sady toolkit.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-120">**Azure Explorer for Eclipse**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="f0b2e-121">**Azure modul plug-in pro Eclipse s Javou**: Tato součást poskytuje podporu pro vývoj projekty, které pomáhají vytváření, testování a nasazení aplikací v jazyce Java pro Microsoft Azure cloud v prostředí Eclipse a prostřednictvím příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-121">**Azure Plugin for Eclipse with Java**: This component provides support for developing projects that help build, test and deploy Java applications for the Microsoft Azure cloud in Eclipse and via command line.</span></span>
   * <span data-ttu-id="f0b2e-122">**Azure modulu plug-in webové aplikace s Javou**: Tato součást poskytuje podporu pro nasazení webových aplikací Java do kontejnerů webové aplikace Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-122">**Azure Web Apps Plugin with Java**: This component provides support for deploying Java web applications to Microsoft Azure Web App containers.</span></span>
   * <span data-ttu-id="f0b2e-123">**4.2 ovladač JDBC Microsoft pro systém SQL Server**: Tato součást poskytuje JDBC rozhraní API pro SQL Server a Microsoft Azure SQL Database pro Java platformy Enterprise Edition 8.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-123">**Microsoft JDBC Driver 4.2 for SQL Server**: This component provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span>
   * <span data-ttu-id="f0b2e-124">**Balíček pro Apache Qpid klientské knihovny pro JMS**: Tato součást poskytuje součást klienta nástroje JMS z projektu Apache Qpid povolit aplikace k používání protokolu AMQP zasílání zpráv ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-124">**Package for Apache Qpid Client Libraries for JMS**: This component provides the JMS client component from the Apache Qpid project to enable your application to use AMQP messaging in Microsoft Azure.</span></span>
   * <span data-ttu-id="f0b2e-125">**Balíček pro knihovny Microsoft Azure Libraries for Java**: Tato součást poskytuje rozhraní API pro přístup ke službám Microsoft Azure, jako je například úložiště, služby service bus, běh služby atd.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-125">**Package for Microsoft Azure Libraries for Java**: This component provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span>
6. <span data-ttu-id="f0b2e-126">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-126">Click **Next**.</span></span> <span data-ttu-id="f0b2e-127">(Pokud se setkáte neobvyklou zpoždění při instalaci sady nástrojů, ujistěte se, že **obraťte se na všechny lokality aktualizace při instalaci najít požadovaný software** není zaškrtnuta.)</span><span class="sxs-lookup"><span data-stu-id="f0b2e-127">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>
7. <span data-ttu-id="f0b2e-128">V **nainstalovat podrobnosti** dialogové okno, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-128">In the **Install Details** dialog, click **Next**.</span></span>
   
    ![Přečtěte si podrobné informace o instalaci][03]
8. <span data-ttu-id="f0b2e-130">V **zkontrolujte licence** dialogové okno, přečtěte si podmínky licenční smlouvy.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-130">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="f0b2e-131">Pokud souhlasíte s podmínkami licenční smlouvy, klikněte na tlačítko **s podmínkami licenční smlouvy souhlasím** a pak klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-131">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="f0b2e-132">(Zbývající kroky předpokládají, že souhlasíte s podmínkami licenční smlouvy.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-132">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="f0b2e-133">Pokud nesouhlasíte s podmínkami licenční smlouvy, ukončete proces instalace.)</span><span class="sxs-lookup"><span data-stu-id="f0b2e-133">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
    ![Zkontrolujte licencí][04]
   
    <span data-ttu-id="f0b2e-135">Eclipse bude stáhněte a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-135">Eclipse will download and install the requisite packages.</span></span>
   
    ![Průběh instalace][05]
9. <span data-ttu-id="f0b2e-137">Pokud se zobrazí výzva k restartování prostředí Eclipse pro dokončení instalace, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="f0b2e-137">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
    ![Restartujte řádku][06]

## <a name="see-also"></a><span data-ttu-id="f0b2e-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="f0b2e-139">See Also</span></span>
<span data-ttu-id="f0b2e-140">Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="f0b2e-140">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="f0b2e-141">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-141">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="f0b2e-142">[Novinky v sadě Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-142">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="f0b2e-143">*Instalace Azure Toolkit pro Eclipse (v tomto článku)*</span><span class="sxs-lookup"><span data-stu-id="f0b2e-143">*Installing the Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="f0b2e-144">[Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-144">[Sign In Instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="f0b2e-145">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-145">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="f0b2e-146">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-146">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="f0b2e-147">[Novinky v sadě Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-147">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="f0b2e-148">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-148">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="f0b2e-149">[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-149">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="f0b2e-150">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="f0b2e-150">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="f0b2e-151">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].</span><span class="sxs-lookup"><span data-stu-id="f0b2e-151">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="f0b2e-152">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-152">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="f0b2e-153">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-153">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="f0b2e-154">[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-154">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="f0b2e-155">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-155">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
<span data-ttu-id="f0b2e-156">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-156">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="f0b2e-157">[Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-157">[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="f0b2e-158">[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-158">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="f0b2e-159">[Novinky v sadě Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-159">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="f0b2e-160">[Novinky v sadě Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="f0b2e-160">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="f0b2e-161">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="f0b2e-161">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
