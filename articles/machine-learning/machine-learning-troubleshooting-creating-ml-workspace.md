---
title: "Řešení potíží: Vytvořte a připojte se k pracovní prostor Machine Learning | Microsoft Docs"
description: "Řešení pro běžné problémy při vytvoření a připojení k pracovní prostor služby Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 398ac3d9c9d32a1ab10413ce0d7ce8d448890409
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a><span data-ttu-id="6e934-103">Průvodce odstraňováním potíží: Vytvoření pracovního prostoru Machine Learning a připojení k němu</span><span class="sxs-lookup"><span data-stu-id="6e934-103">Troubleshooting guide: Create and connect to an Machine Learning workspace</span></span>
<span data-ttu-id="6e934-104">Tato příručka poskytuje řešení pro některé nejčastější problémy při se nastavování pracovních prostorů Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6e934-104">This guide provides solutions for some frequently encountered challenges when you are setting up Azure Machine Learning workspaces.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a><span data-ttu-id="6e934-105">Vlastník pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="6e934-105">Workspace owner</span></span>
<span data-ttu-id="6e934-106">Otevřete pracovní prostor v nástroji Machine Learning Studio, musíte být přihlášeni k Microsoft Account jste použili k vytvoření pracovního prostoru, nebo je potřeba přijmout pozvánku od vlastníka pro připojení k pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="6e934-106">To open a workspace in Machine Learning Studio, you must be signed in to the Microsoft Account you used to create the workspace, or you need to receive an invitation from the owner to join the workspace.</span></span> <span data-ttu-id="6e934-107">Z portálu Azure můžete spravovat pracovní prostor, který zahrnuje možnost konfigurace přístupu.</span><span class="sxs-lookup"><span data-stu-id="6e934-107">From the Azure portal you can manage the workspace, which includes the ability to configure access.</span></span>

<span data-ttu-id="6e934-108">Další informace o pracovním prostoru Správa, najdete v části [spravovat pracovní prostor služby Azure Machine Learning].</span><span class="sxs-lookup"><span data-stu-id="6e934-108">For more information on managing a workspace, see [Manage an Azure Machine Learning workspace].</span></span>

<span data-ttu-id="6e934-109">[spravovat pracovní prostor služby Azure Machine Learning]: machine-learning-manage-workspace.md</span><span class="sxs-lookup"><span data-stu-id="6e934-109">[Manage an Azure Machine Learning workspace]: machine-learning-manage-workspace.md</span></span>

## <a name="allowed-regions"></a><span data-ttu-id="6e934-110">Povolených oblastí</span><span class="sxs-lookup"><span data-stu-id="6e934-110">Allowed regions</span></span>
<span data-ttu-id="6e934-111">Machine Learning je aktuálně k dispozici pro omezený počet oblastí.</span><span class="sxs-lookup"><span data-stu-id="6e934-111">Machine Learning is currently available in a limited number of regions.</span></span> <span data-ttu-id="6e934-112">Pokud vaše předplatné neobsahuje jednu z těchto oblastí, mohou se zobrazit chybová zpráva, "V povolených oblastí používat žádná předplatná."</span><span class="sxs-lookup"><span data-stu-id="6e934-112">If your subscription does not include one of these regions, you may see the error message, “You have no subscriptions in the allowed regions.”</span></span>

<span data-ttu-id="6e934-113">Chcete-li požádat o, oblast přidat do vašeho předplatného, z portálu Azure vytvořit novou žádost o podporu společnosti Microsoft, zvolte **fakturace** jako typ problému a postupujte podle pokynů a odešlete žádost.</span><span class="sxs-lookup"><span data-stu-id="6e934-113">To request that a region be added to your subscription, create a new Microsoft support request from the Azure portal, choose **Billing** as the problem type, and follow the prompts to submit your request.</span></span>

## <a name="storage-account"></a><span data-ttu-id="6e934-114">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6e934-114">Storage account</span></span>
<span data-ttu-id="6e934-115">Služba Machine Learning potřebuje účet úložiště pro ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="6e934-115">The Machine Learning service needs a storage account to store data.</span></span> <span data-ttu-id="6e934-116">Můžete použít existující účet úložiště, nebo můžete vytvořit nový účet úložiště, když vytvoříte nový pracovní prostor Machine Learning (Pokud máte kvóty pro vytvoření nového účtu úložiště).</span><span class="sxs-lookup"><span data-stu-id="6e934-116">You can use an existing storage account, or you can create a new storage account when you create the new Machine Learning workspace (if you have quota to create a new storage account).</span></span>

<span data-ttu-id="6e934-117">Po vytvoření nového pracovního prostoru Machine Learning se můžete přihlásit Machine Learning Studio pomocí účtu Microsoft, který jste použili k vytvoření pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6e934-117">After the new Machine Learning workspace is created, you can sign in to Machine Learning Studio by using the Microsoft account you used to create the workspace.</span></span> <span data-ttu-id="6e934-118">Pokud narazíte na chybová zpráva "Prostoru nebyl nalezen" (podobně jako na následujícím snímku obrazovky), použijte následující postup odstranit soubory cookie prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6e934-118">If you encounter the error message, “Workspace Not Found” (similar to the following screenshot), please use the following steps to delete your browser cookies.</span></span>

![Pracovní prostor nebyl nalezen][screen3]

<span data-ttu-id="6e934-120">**Chcete-li odstranit soubory cookie prohlížeče**</span><span class="sxs-lookup"><span data-stu-id="6e934-120">**To delete browser cookies**</span></span>

1. <span data-ttu-id="6e934-121">Pokud používáte Internet Explorer, klikněte **nástroje** tlačítko v pravém horním rohu a vyberte **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="6e934-121">If you use Internet Explorer, click the **Tools** button in the upper-right corner and select **Internet options**.</span></span>  

![Možnosti Internetu][screen4]

2. <span data-ttu-id="6e934-123">V části **Obecné** , klikněte na **odstranit...**</span><span class="sxs-lookup"><span data-stu-id="6e934-123">Under the **General** tab, click **Delete…**</span></span>

![Karta Obecné][screen5]

3. <span data-ttu-id="6e934-125">V **odstranit historii procházení** dialogové okno zkontrolujte, zda **soubory cookie a data webové stránky** je vybrána a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6e934-125">In the **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.</span></span>

![Odstraňte soubory cookie][screen6]

<span data-ttu-id="6e934-127">Po odstranění souborů cookie se znovu spustit prohlížeč a přejděte ke [Microsoft Azure Machine Learning](https://studio.azureml.net) stránky.</span><span class="sxs-lookup"><span data-stu-id="6e934-127">After the cookies are deleted, restart the browser and then go to the [Microsoft Azure Machine Learning](https://studio.azureml.net) page.</span></span> <span data-ttu-id="6e934-128">Po zobrazení výzvy k zadání uživatelského jména a hesla, zadejte stejný účet Microsoft, které jste použili k vytvoření pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6e934-128">When you are prompted for a user name and password, enter the same Microsoft account you used to create the workspace.</span></span>

## <a name="comments"></a><span data-ttu-id="6e934-129">Komentáře</span><span class="sxs-lookup"><span data-stu-id="6e934-129">Comments</span></span>

<span data-ttu-id="6e934-130">Naším cílem je zajistit nejblíže jako bezproblémové prostředí Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6e934-130">Our goal is to make the Machine Learning experience as seamless as possible.</span></span> <span data-ttu-id="6e934-131">Položit všechny komentáře a problémy v [fórum Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) a pomoci tak můžete poskytovat lepší.</span><span class="sxs-lookup"><span data-stu-id="6e934-131">Please post any comments and issues at the [Azure Machine Learning forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) to help us serve you better.</span></span>

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
