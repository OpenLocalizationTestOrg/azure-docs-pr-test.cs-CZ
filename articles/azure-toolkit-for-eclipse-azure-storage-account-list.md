---
title: "aaaAzure seznam účtů úložiště"
description: "Spravovat nastavení účtu úložiště pomocí hello Azure Toolkit pro Eclipse"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35e25881ca95ae4050a26283e4726d9549b37f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-account-list"></a><span data-ttu-id="73ba3-103">Seznam účtů úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="73ba3-103">Azure Storage Account List</span></span>
<span data-ttu-id="73ba3-104">Úložiště Azure, která účty povolit stáhnout toobe umístění použít JDK, aplikační server a libovolné součásti a také pro ukládání stavu, při použití ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="73ba3-104">Azure storage accounts enable download locations toobe used for your JDK, application server, and arbitrary components, as well as for storing state when using caching.</span></span> <span data-ttu-id="73ba3-105">Eclipse udržuje seznam známých úložiště účty, které jsou k dispozici tooyour projekty v pracovním prostoru Eclipse.</span><span class="sxs-lookup"><span data-stu-id="73ba3-105">Eclipse maintains a list of known storage accounts that are available tooyour projects in your Eclipse workspace.</span></span> <span data-ttu-id="73ba3-106">tooopen hello **účty úložiště** dialog, který je použité toomanage, který zobrazí seznam v prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure** a potom klikněte na **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-106">tooopen hello **Storage Accounts** dialog, which is used toomanage that list, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Storage Accounts**.</span></span>

<span data-ttu-id="73ba3-107">Následující Hello ukazuje hello **účty úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-107">hello following shows hello **Storage Accounts** dialog.</span></span>

![][ic719496]

<span data-ttu-id="73ba3-108">Tento dialog můžete také otevřít z **účty** odkaz v dialogových oknech, které používají účty úložiště, jako je například hello následující:</span><span class="sxs-lookup"><span data-stu-id="73ba3-108">This dialog can also be opened from an **Accounts** link on dialog boxes that use storage accounts, such as hello following:</span></span>

* <span data-ttu-id="73ba3-109">Hello **JDK** kartě hello **konfigurace serveru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-109">hello **JDK** tab of hello **Server Configuration** dialog.</span></span>
* <span data-ttu-id="73ba3-110">Hello **Server** kartě hello **konfigurace serveru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-110">hello **Server** tab of hello **Server Configuration** dialog.</span></span>
* <span data-ttu-id="73ba3-111">Hello **přidat součást** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-111">hello **Add Component** dialog.</span></span>
* <span data-ttu-id="73ba3-112">Hello **ukládání do mezipaměti** dialogovém okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="73ba3-112">hello **Caching** properties dialog.</span></span>

## <a name="tooimport-your-storage-accounts-using-a-publish-settings-file"></a><span data-ttu-id="73ba3-113">tooimport účtů úložiště pomocí soubor nastavení publikování</span><span class="sxs-lookup"><span data-stu-id="73ba3-113">tooimport your storage accounts using a publish settings file</span></span>
1. <span data-ttu-id="73ba3-114">V rámci hello **účty úložiště** dialogové okno, klikněte na tlačítko **Import ze souboru nastavení publikování**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-114">Within hello **Storage Accounts** dialog, click **Import from PUBLISH-SETTINGS file**.</span></span>

2. <span data-ttu-id="73ba3-115">(Tento krok přeskočte, pokud již máte uložené publikování nastavení souboru tooyour místního počítače.) V hello **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **stáhnout soubor nastavení publikování**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-115">(Skip this step if you have already saved a publish settings file tooyour local machine.) In hello **Import Subscription Information** dialog, click **Download PUBLISH-SETTINGS File**.</span></span> <span data-ttu-id="73ba3-116">Pokud ještě nejste přihlášeni k účtu Azure, bude výzvami toolog v.</span><span class="sxs-lookup"><span data-stu-id="73ba3-116">If you are not yet logged into your Azure account, you will be prompted toolog in.</span></span> <span data-ttu-id="73ba3-117">Potom budete vyzváni k souboru s nastavením publikování toosave Azure.</span><span class="sxs-lookup"><span data-stu-id="73ba3-117">Then you'll be prompted toosave an Azure publish settings file.</span></span> <span data-ttu-id="73ba3-118">(Můžete ignorovat hello výsledné pokynů na stránkách přihlášení hello - jejich jsou poskytovány hello portál Azure a jsou určeny pro Visual Studio uživatele.) Uložte tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="73ba3-118">(You can ignore hello resulting instructions shown on hello logon pages - they are provided by hello Azure portal and are intended for Visual Studio users.) Save it tooyour local machine.</span></span>

3. <span data-ttu-id="73ba3-119">Stále v hello **importovat informace o předplatném** dialogové okno, klikněte na tlačítko hello **Procházet** tlačítko, vyberte hello publikovat soubor nastavení, který jste si místně předtím uložili a potom klikněte na **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-119">Still in hello **Import Subscription Information** dialog, click hello **Browse** button, select hello publish settings file that you saved locally previously, and then click **Open**.</span></span>

4. <span data-ttu-id="73ba3-120">Klikněte na tlačítko **OK** tooclose hello **importovat informace o předplatném** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-120">Click **OK** tooclose hello **Import Subscription Information** dialog.</span></span>

## <a name="toocreate-a-new-storage-account"></a><span data-ttu-id="73ba3-121">toocreate nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="73ba3-121">toocreate a new storage account</span></span>
1. <span data-ttu-id="73ba3-122">V rámci hello **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-122">Within hello **Storage Accounts** dialog, click **Add**.</span></span>

2. <span data-ttu-id="73ba3-123">V rámci hello **přidat účet úložiště** dialogové okno, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-123">Within hello **Add Storage Account** dialog, click **New**.</span></span>

3. <span data-ttu-id="73ba3-124">V rámci hello **nový účet úložiště** dialogové okno, zadejte hodnoty pro hello následující:</span><span class="sxs-lookup"><span data-stu-id="73ba3-124">Within hello **New Storage Account** dialog, specify values for hello following:</span></span>

   * <span data-ttu-id="73ba3-125">Název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="73ba3-125">Storage account name.</span></span>

   * <span data-ttu-id="73ba3-126">Umístění účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="73ba3-126">Location of hello storage account.</span></span>

   * <span data-ttu-id="73ba3-127">Popis účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="73ba3-127">Description of hello storage account.</span></span>

   * <span data-ttu-id="73ba3-128">účet úložiště Hello předplatné toowhich hello patří.</span><span class="sxs-lookup"><span data-stu-id="73ba3-128">hello subscription toowhich hello storage account belongs.</span></span>

4. <span data-ttu-id="73ba3-129">Klikněte na tlačítko **OK** tooclose hello **nový účet úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-129">Click **OK** tooclose hello **New Storage Account** dialog.</span></span>

<span data-ttu-id="73ba3-130">Ho může trvat několik minut, než vaše toobe účet úložiště vytvořit.</span><span class="sxs-lookup"><span data-stu-id="73ba3-130">It may take several minutes for your storage account toobe created.</span></span> <span data-ttu-id="73ba3-131">Po vytvoření, klikněte na tlačítko **OK** tooclose hello **přidat účet úložiště** dialogové okno a váš nový účet úložiště se přidá toohello seznam účtů úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="73ba3-131">After it is created, click **OK** tooclose hello **Add Storage Account** dialog, and your new storage account will be added toohello list of available storage accounts.</span></span>

## <a name="tooadd-an-existing-storage-account-toohello-list"></a><span data-ttu-id="73ba3-132">tooadd existující toohello seznam účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="73ba3-132">tooadd an existing storage account toohello list</span></span>
1. <span data-ttu-id="73ba3-133">Pokud již nemáte úložiště Azure, účet, vytvořte ho pomocí následujících kroků hello uvedené hello **toocreate novou část účtu úložiště** výše.</span><span class="sxs-lookup"><span data-stu-id="73ba3-133">If you do not already have a Azure storage account, create one by following hello steps listed in hello **toocreate a new storage account section** above.</span></span> <span data-ttu-id="73ba3-134">(Alternativně můžete vytvořit nový účet úložiště v hello [portálu pro správu Azure][Azure Management Portal].)</span><span class="sxs-lookup"><span data-stu-id="73ba3-134">(Alternatively, you can create a new storage account at hello [Azure Management Portal][Azure Management Portal].)</span></span>

2. <span data-ttu-id="73ba3-135">V rámci hello **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-135">Within hello **Storage Accounts** dialog, click **Add**.</span></span>

3. <span data-ttu-id="73ba3-136">V rámci hello **přidat účet úložiště** dialogové okno, zadejte hodnoty pro **název** a **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-136">Within hello **Add Storage Account** dialog, enter values for **Name** and **Access Key**.</span></span> <span data-ttu-id="73ba3-137">název a přístupový klíč účtu Hello musí být pro existující účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="73ba3-137">hello account name and access key must be for an existing Azure storage account.</span></span> <span data-ttu-id="73ba3-138">Použití hello **úložiště** části hello [portálu pro správu Azure] [ Azure Management Portal] tooview účtu úložiště názvy a klíče.</span><span class="sxs-lookup"><span data-stu-id="73ba3-138">Use hello **Storage** section of hello [Azure Management Portal][Azure Management Portal] tooview your storage account names and keys.</span></span> <span data-ttu-id="73ba3-139">Vaše **přidat účet úložiště** dialogové okno bude vypadat podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="73ba3-139">Your **Add Storage Account** dialog will look similar toohello following.</span></span>
   
   ![][ic719497]

4. <span data-ttu-id="73ba3-140">Klikněte na tlačítko **OK** tooclose hello **přidat účet úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-140">Click **OK** tooclose hello **Add Storage Account** dialog.</span></span>

## <a name="toomodify-a-storage-account-toouse-a-new-access-key"></a><span data-ttu-id="73ba3-141">toomodify toouse účtu úložiště nového přístupového klíče</span><span class="sxs-lookup"><span data-stu-id="73ba3-141">toomodify a storage account toouse a new access key</span></span>
1. <span data-ttu-id="73ba3-142">V rámci hello **účty úložiště** dialogové okno, klikněte na účet úložiště hello má tooedit a pak klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-142">Within hello **Storage Accounts** dialog, click hello storage account that you want tooedit and then click **Edit**.</span></span>

2. <span data-ttu-id="73ba3-143">V rámci hello **upravit přístupový klíč účtu úložiště** dialogu Upravit hello **přístupový klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="73ba3-143">Within hello **Edit Storage Account Access Key** dialog, modify hello **Access Key** value.</span></span>

3. <span data-ttu-id="73ba3-144">Klikněte na tlačítko **OK** tooclose hello **upravit přístupový klíč účtu úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73ba3-144">Click **OK** tooclose hello **Edit Storage Account Access Key** dialog.</span></span>

## <a name="tooremove-a-storage-account-from-hello-list-maintained-in-eclipse"></a><span data-ttu-id="73ba3-145">tooremove účet úložiště ze seznamu hello udržovat v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="73ba3-145">tooremove a storage account from hello list maintained in Eclipse</span></span>
1. <span data-ttu-id="73ba3-146">V rámci hello **účty úložiště** dialogové okno, klikněte na účet úložiště hello má tooedit a pak klikněte na **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="73ba3-146">Within hello **Storage Accounts** dialog, click hello storage account that you want tooedit and then click **Remove**.</span></span>

2. <span data-ttu-id="73ba3-147">Klikněte na tlačítko **OK** při výzvami tooremove hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="73ba3-147">Click **OK** when prompted tooremove hello storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="73ba3-148">Odebrání účtu úložiště hello prostřednictvím hello **účty úložiště** dialogové okno pouze odebere z hello seznam účtů úložiště lze zobrazit v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="73ba3-148">Removing hello storage account through hello **Storage Accounts** dialog only removes it from hello list of storage accounts viewable within Eclipse.</span></span> <span data-ttu-id="73ba3-149">Neodebere účet úložiště hello ze svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="73ba3-149">It does not remove hello storage account from your Azure subscription.</span></span> <span data-ttu-id="73ba3-150">Kromě toho hello účet úložiště může znovu po zobrazí v seznamu Eclipse znovu načte hello podrobnosti svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="73ba3-150">Additionally, hello storage account could appear again in your list after Eclipse reloads hello details of your subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="73ba3-151">Viz také</span><span class="sxs-lookup"><span data-stu-id="73ba3-151">See Also</span></span>
<span data-ttu-id="73ba3-152">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="73ba3-152">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="73ba3-153">[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="73ba3-153">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="73ba3-154">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="73ba3-154">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="73ba3-155">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="73ba3-155">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
