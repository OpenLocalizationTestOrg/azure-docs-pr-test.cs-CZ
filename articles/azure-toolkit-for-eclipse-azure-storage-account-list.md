---
title: "Seznam účtů úložiště Azure"
description: "Spravovat nastavení účtu úložiště pomocí sady nástrojů pro Azure pro Eclipse"
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
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a><span data-ttu-id="f5a34-103">Seznam účtů úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="f5a34-103">Azure Storage Account List</span></span>
<span data-ttu-id="f5a34-104">Účty úložiště Azure povolit umístění stahování, který se má použít pro vaše JDK, aplikační server a libovolné součásti a také pro ukládání stavu, při použití ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f5a34-104">Azure storage accounts enable download locations to be used for your JDK, application server, and arbitrary components, as well as for storing state when using caching.</span></span> <span data-ttu-id="f5a34-105">Eclipse udržuje seznam známých úložiště účty, které jsou k dispozici do vašich projektů v pracovním prostoru Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f5a34-105">Eclipse maintains a list of known storage accounts that are available to your projects in your Eclipse workspace.</span></span> <span data-ttu-id="f5a34-106">Otevřete **účty úložiště** dialog, který se používá ke správě tohoto seznamu, v rámci prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure**a potom klikněte na **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-106">To open the **Storage Accounts** dialog, which is used to manage that list, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Storage Accounts**.</span></span>

<span data-ttu-id="f5a34-107">Následující ukazuje **účty úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-107">The following shows the **Storage Accounts** dialog.</span></span>

![][ic719496]

<span data-ttu-id="f5a34-108">Tento dialog můžete také otevřít z **účty** odkaz v dialogových oknech, které používají účty úložiště, jako jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f5a34-108">This dialog can also be opened from an **Accounts** link on dialog boxes that use storage accounts, such as the following:</span></span>

* <span data-ttu-id="f5a34-109">**JDK** kartě **konfigurace serveru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-109">The **JDK** tab of the **Server Configuration** dialog.</span></span>
* <span data-ttu-id="f5a34-110">**Server** kartě **konfigurace serveru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-110">The **Server** tab of the **Server Configuration** dialog.</span></span>
* <span data-ttu-id="f5a34-111">**Přidat součást** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-111">The **Add Component** dialog.</span></span>
* <span data-ttu-id="f5a34-112">**Ukládání do mezipaměti** dialogovém okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f5a34-112">The **Caching** properties dialog.</span></span>

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a><span data-ttu-id="f5a34-113">Chcete-li importovat soubor nastavení publikování pomocí účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="f5a34-113">To import your storage accounts using a publish settings file</span></span>
1. <span data-ttu-id="f5a34-114">V rámci **účty úložiště** dialogové okno, klikněte na tlačítko **Import ze souboru nastavení publikování**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-114">Within the **Storage Accounts** dialog, click **Import from PUBLISH-SETTINGS file**.</span></span>

2. <span data-ttu-id="f5a34-115">(Tento krok přeskočte, pokud jste už uložili soubor nastavení publikování do místního počítače.) V **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **stáhnout soubor nastavení publikování**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-115">(Skip this step if you have already saved a publish settings file to your local machine.) In the **Import Subscription Information** dialog, click **Download PUBLISH-SETTINGS File**.</span></span> <span data-ttu-id="f5a34-116">Pokud ještě nejste přihlášeni k účtu Azure, budete vyzváni k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f5a34-116">If you are not yet logged into your Azure account, you will be prompted to log in.</span></span> <span data-ttu-id="f5a34-117">Potom budete vyzváni k uložení Azure soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="f5a34-117">Then you'll be prompted to save an Azure publish settings file.</span></span> <span data-ttu-id="f5a34-118">(Můžete ignorovat výsledné pokynů na stránkách přihlášení - jejich jsou poskytovány na portálu Azure a jsou určeny pro Visual Studio uživatele.) Uložte ho do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="f5a34-118">(You can ignore the resulting instructions shown on the logon pages - they are provided by the Azure portal and are intended for Visual Studio users.) Save it to your local machine.</span></span>

3. <span data-ttu-id="f5a34-119">Pořád ještě v **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **Procházet** tlačítko, vyberte soubor nastavení publikování, který jste si uložili místně dříve a pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-119">Still in the **Import Subscription Information** dialog, click the **Browse** button, select the publish settings file that you saved locally previously, and then click **Open**.</span></span>

4. <span data-ttu-id="f5a34-120">Klikněte na tlačítko **OK** zavřete **importovat informace o předplatném** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-120">Click **OK** to close the **Import Subscription Information** dialog.</span></span>

## <a name="to-create-a-new-storage-account"></a><span data-ttu-id="f5a34-121">Chcete-li vytvořit nový účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f5a34-121">To create a new storage account</span></span>
1. <span data-ttu-id="f5a34-122">V rámci **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-122">Within the **Storage Accounts** dialog, click **Add**.</span></span>

2. <span data-ttu-id="f5a34-123">V rámci **přidat účet úložiště** dialogové okno, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-123">Within the **Add Storage Account** dialog, click **New**.</span></span>

3. <span data-ttu-id="f5a34-124">V rámci **nový účet úložiště** dialogové okno, zadejte hodnoty pro následující:</span><span class="sxs-lookup"><span data-stu-id="f5a34-124">Within the **New Storage Account** dialog, specify values for the following:</span></span>

   * <span data-ttu-id="f5a34-125">Název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5a34-125">Storage account name.</span></span>

   * <span data-ttu-id="f5a34-126">Umístění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5a34-126">Location of the storage account.</span></span>

   * <span data-ttu-id="f5a34-127">Popis účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5a34-127">Description of the storage account.</span></span>

   * <span data-ttu-id="f5a34-128">Odběr, do které patří k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5a34-128">The subscription to which the storage account belongs.</span></span>

4. <span data-ttu-id="f5a34-129">Klikněte na tlačítko **OK** zavřete **nový účet úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-129">Click **OK** to close the **New Storage Account** dialog.</span></span>

<span data-ttu-id="f5a34-130">Ho může trvat několik minut, než váš účet úložiště, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f5a34-130">It may take several minutes for your storage account to be created.</span></span> <span data-ttu-id="f5a34-131">Po vytvoření, klikněte na tlačítko **OK** zavřete **přidat účet úložiště** dialogové okno a váš nový účet úložiště se zařadí do seznamu účtů úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f5a34-131">After it is created, click **OK** to close the **Add Storage Account** dialog, and your new storage account will be added to the list of available storage accounts.</span></span>

## <a name="to-add-an-existing-storage-account-to-the-list"></a><span data-ttu-id="f5a34-132">Chcete-li přidat do seznamu existující účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f5a34-132">To add an existing storage account to the list</span></span>
1. <span data-ttu-id="f5a34-133">Pokud již účet úložiště Azure nemáte, vytvořte ho pomocí následujících kroků uvedených v **k vytvoření nového účtu úložiště v tématu** výše.</span><span class="sxs-lookup"><span data-stu-id="f5a34-133">If you do not already have a Azure storage account, create one by following the steps listed in the **To create a new storage account section** above.</span></span> <span data-ttu-id="f5a34-134">(Alternativně můžete vytvořit nový účet úložiště na [portálu pro správu Azure][Azure Management Portal].)</span><span class="sxs-lookup"><span data-stu-id="f5a34-134">(Alternatively, you can create a new storage account at the [Azure Management Portal][Azure Management Portal].)</span></span>

2. <span data-ttu-id="f5a34-135">V rámci **účty úložiště** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-135">Within the **Storage Accounts** dialog, click **Add**.</span></span>

3. <span data-ttu-id="f5a34-136">V rámci **přidat účet úložiště** dialogové okno, zadejte hodnoty pro **název** a **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-136">Within the **Add Storage Account** dialog, enter values for **Name** and **Access Key**.</span></span> <span data-ttu-id="f5a34-137">Název a přístupový klíč účtu musí být pro existující účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f5a34-137">The account name and access key must be for an existing Azure storage account.</span></span> <span data-ttu-id="f5a34-138">Použití **úložiště** části [portálu pro správu Azure] [ Azure Management Portal] zobrazíte názvy účtů úložiště a klíče.</span><span class="sxs-lookup"><span data-stu-id="f5a34-138">Use the **Storage** section of the [Azure Management Portal][Azure Management Portal] to view your storage account names and keys.</span></span> <span data-ttu-id="f5a34-139">Vaše **přidat účet úložiště** dialogové okno bude vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="f5a34-139">Your **Add Storage Account** dialog will look similar to the following.</span></span>
   
   ![][ic719497]

4. <span data-ttu-id="f5a34-140">Klikněte na tlačítko **OK** zavřete **přidat účet úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-140">Click **OK** to close the **Add Storage Account** dialog.</span></span>

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a><span data-ttu-id="f5a34-141">Chcete-li upravit účet úložiště, který pomocí nového přístupového klíče</span><span class="sxs-lookup"><span data-stu-id="f5a34-141">To modify a storage account to use a new access key</span></span>
1. <span data-ttu-id="f5a34-142">V rámci **účty úložiště** dialogové okno, klikněte na účet úložiště, které chcete upravit a pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-142">Within the **Storage Accounts** dialog, click the storage account that you want to edit and then click **Edit**.</span></span>

2. <span data-ttu-id="f5a34-143">V rámci **upravit přístupový klíč účtu úložiště** dialogu Upravit **přístupový klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f5a34-143">Within the **Edit Storage Account Access Key** dialog, modify the **Access Key** value.</span></span>

3. <span data-ttu-id="f5a34-144">Klikněte na tlačítko **OK** zavřete **upravit přístupový klíč účtu úložiště** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a34-144">Click **OK** to close the **Edit Storage Account Access Key** dialog.</span></span>

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a><span data-ttu-id="f5a34-145">Chcete odstranit účet úložiště ze seznamu udržovat v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="f5a34-145">To remove a storage account from the list maintained in Eclipse</span></span>
1. <span data-ttu-id="f5a34-146">V rámci **účty úložiště** dialogové okno, klikněte na účet úložiště, které chcete upravit a pak klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="f5a34-146">Within the **Storage Accounts** dialog, click the storage account that you want to edit and then click **Remove**.</span></span>

2. <span data-ttu-id="f5a34-147">Klikněte na tlačítko **OK** po zobrazení výzvy k odebrání účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5a34-147">Click **OK** when prompted to remove the storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="f5a34-148">Odebrání účtu úložiště prostřednictvím **účty úložiště** dialogové okno pouze odebere ze seznamu účtů úložiště lze zobrazit v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f5a34-148">Removing the storage account through the **Storage Accounts** dialog only removes it from the list of storage accounts viewable within Eclipse.</span></span> <span data-ttu-id="f5a34-149">Neodebere účet úložiště ze svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f5a34-149">It does not remove the storage account from your Azure subscription.</span></span> <span data-ttu-id="f5a34-150">Kromě toho účet úložiště může znovu po zobrazí v seznamu Eclipse znovu načte podrobnosti svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="f5a34-150">Additionally, the storage account could appear again in your list after Eclipse reloads the details of your subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="f5a34-151">Viz také</span><span class="sxs-lookup"><span data-stu-id="f5a34-151">See Also</span></span>
<span data-ttu-id="f5a34-152">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f5a34-152">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="f5a34-153">[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f5a34-153">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="f5a34-154">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="f5a34-154">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="f5a34-155">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="f5a34-155">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
