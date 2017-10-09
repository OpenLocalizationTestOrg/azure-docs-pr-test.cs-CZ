---
title: "Řešení potíží: Vytvořte a připojte tooa pracovní prostor Machine Learning | Microsoft Docs"
description: "Řešení pro běžné problémy při vytvoření a připojení tooan Azure Machine Learning prostoru"
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
ms.openlocfilehash: 965a0025e85ba4e22c2b037edfa923e7f7599069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-create-and-connect-tooan-machine-learning-workspace"></a><span data-ttu-id="a1fef-103">Příručka pro řešení potíží: Vytvořte a připojte tooan pracovní prostor Machine Learning</span><span class="sxs-lookup"><span data-stu-id="a1fef-103">Troubleshooting guide: Create and connect tooan Machine Learning workspace</span></span>
<span data-ttu-id="a1fef-104">Tato příručka poskytuje řešení pro některé nejčastější problémy při se nastavování pracovních prostorů Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="a1fef-104">This guide provides solutions for some frequently encountered challenges when you are setting up Azure Machine Learning workspaces.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a><span data-ttu-id="a1fef-105">Vlastník pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="a1fef-105">Workspace owner</span></span>
<span data-ttu-id="a1fef-106">tooopen pracovního prostoru v Machine Learning Studio, musíte být přihlášeni toohello Account Microsoft používá toocreate hello prostoru, nebo musí tooreceive Pozvánka z pracovního prostoru hello vlastníka toojoin hello.</span><span class="sxs-lookup"><span data-stu-id="a1fef-106">tooopen a workspace in Machine Learning Studio, you must be signed in toohello Microsoft Account you used toocreate hello workspace, or you need tooreceive an invitation from hello owner toojoin hello workspace.</span></span> <span data-ttu-id="a1fef-107">Z portálu Azure, které můžete spravovat hello hello pracovní prostor, který zahrnuje hello možnost tooconfigure přístup.</span><span class="sxs-lookup"><span data-stu-id="a1fef-107">From hello Azure portal you can manage hello workspace, which includes hello ability tooconfigure access.</span></span>

<span data-ttu-id="a1fef-108">Další informace o pracovním prostoru Správa, najdete v části [spravovat pracovní prostor služby Azure Machine Learning].</span><span class="sxs-lookup"><span data-stu-id="a1fef-108">For more information on managing a workspace, see [Manage an Azure Machine Learning workspace].</span></span>

[spravovat pracovní prostor služby Azure Machine Learning]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a><span data-ttu-id="a1fef-110">Povolených oblastí</span><span class="sxs-lookup"><span data-stu-id="a1fef-110">Allowed regions</span></span>
<span data-ttu-id="a1fef-111">Machine Learning je aktuálně k dispozici pro omezený počet oblastí.</span><span class="sxs-lookup"><span data-stu-id="a1fef-111">Machine Learning is currently available in a limited number of regions.</span></span> <span data-ttu-id="a1fef-112">Pokud vaše předplatné neobsahuje jednu z těchto oblastí, mohou se zobrazit chybová zpráva hello, "V hello povolené oblasti používat žádná předplatná."</span><span class="sxs-lookup"><span data-stu-id="a1fef-112">If your subscription does not include one of these regions, you may see hello error message, “You have no subscriptions in hello allowed regions.”</span></span>

<span data-ttu-id="a1fef-113">Přidat předplatné tooyour toorequest, který se v oblasti, vytvořit novou žádost o podporu společnosti Microsoft z hello portálu Azure, zvolte **fakturace** jako typ problému hello a postupujte podle kroků vyzve hello toosubmit vaši žádost.</span><span class="sxs-lookup"><span data-stu-id="a1fef-113">toorequest that a region be added tooyour subscription, create a new Microsoft support request from hello Azure portal, choose **Billing** as hello problem type, and follow hello prompts toosubmit your request.</span></span>

## <a name="storage-account"></a><span data-ttu-id="a1fef-114">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="a1fef-114">Storage account</span></span>
<span data-ttu-id="a1fef-115">Hello služby Machine Learning musí dat toostore účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a1fef-115">hello Machine Learning service needs a storage account toostore data.</span></span> <span data-ttu-id="a1fef-116">Můžete použít existující účet úložiště, nebo můžete vytvořit nový účet úložiště, když vytvoříte hello nový pracovní prostor Machine Learning (Pokud máte kvóty toocreate nový účet úložiště).</span><span class="sxs-lookup"><span data-stu-id="a1fef-116">You can use an existing storage account, or you can create a new storage account when you create hello new Machine Learning workspace (if you have quota toocreate a new storage account).</span></span>

<span data-ttu-id="a1fef-117">Po vytvoření hello nový pracovní prostor Machine Learning je tooMachine Learning Studio přihlásit pomocí účtu Microsoft hello, které že jste použili toocreate hello prostoru.</span><span class="sxs-lookup"><span data-stu-id="a1fef-117">After hello new Machine Learning workspace is created, you can sign in tooMachine Learning Studio by using hello Microsoft account you used toocreate hello workspace.</span></span> <span data-ttu-id="a1fef-118">Pokud narazíte na hello chybová zpráva, "Prostoru nebyl nalezen" (podobně jako toohello následující snímek obrazovky), použijte následující kroky toodelete hello soubory cookie prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a1fef-118">If you encounter hello error message, “Workspace Not Found” (similar toohello following screenshot), please use hello following steps toodelete your browser cookies.</span></span>

![Pracovní prostor nebyl nalezen][screen3]

<span data-ttu-id="a1fef-120">**soubory cookie prohlížeče toodelete**</span><span class="sxs-lookup"><span data-stu-id="a1fef-120">**toodelete browser cookies**</span></span>

1. <span data-ttu-id="a1fef-121">Pokud používáte Internet Explorer, klikněte na tlačítko hello **nástroje** tlačítka na hello pravém horním rohu a vyberte **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="a1fef-121">If you use Internet Explorer, click hello **Tools** button in hello upper-right corner and select **Internet options**.</span></span>  

![Možnosti Internetu][screen4]

2. <span data-ttu-id="a1fef-123">V části hello **Obecné** , klikněte na **odstranit...**</span><span class="sxs-lookup"><span data-stu-id="a1fef-123">Under hello **General** tab, click **Delete…**</span></span>

![Karta Obecné][screen5]

3. <span data-ttu-id="a1fef-125">V hello **odstranit historii procházení** dialogové okno zkontrolujte, zda **soubory cookie a data webové stránky** je vybrána a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a1fef-125">In hello **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.</span></span>

![Odstraňte soubory cookie][screen6]

<span data-ttu-id="a1fef-127">Po odstranění souborů cookie hello se restartujte hello prohlížeče a potom přejděte toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) stránky.</span><span class="sxs-lookup"><span data-stu-id="a1fef-127">After hello cookies are deleted, restart hello browser and then go toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) page.</span></span> <span data-ttu-id="a1fef-128">Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte text hello stejný účet Microsoft, které že jste použili toocreate hello prostoru.</span><span class="sxs-lookup"><span data-stu-id="a1fef-128">When you are prompted for a user name and password, enter hello same Microsoft account you used toocreate hello workspace.</span></span>

## <a name="comments"></a><span data-ttu-id="a1fef-129">Komentáře</span><span class="sxs-lookup"><span data-stu-id="a1fef-129">Comments</span></span>

<span data-ttu-id="a1fef-130">Naším cílem je toomake hello Machine Learning prostředí jako bezproblémové nejblíže.</span><span class="sxs-lookup"><span data-stu-id="a1fef-130">Our goal is toomake hello Machine Learning experience as seamless as possible.</span></span> <span data-ttu-id="a1fef-131">Prosím post všechny komentáře a problémy v hello [fórum Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp nám mohla poskytovat lepší.</span><span class="sxs-lookup"><span data-stu-id="a1fef-131">Please post any comments and issues at hello [Azure Machine Learning forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp us serve you better.</span></span>

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
