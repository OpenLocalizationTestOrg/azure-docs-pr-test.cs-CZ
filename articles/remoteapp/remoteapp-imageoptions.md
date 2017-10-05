---
title: "Vytvoření image Azure Remoteappu | Microsoft Docs"
description: "Další informace o dostupných možnostech pro vytváření bitových kopií pro Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4b8ba6f264f982e03930c5ad4ccdb2d80f2c8665
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-remoteapp-image"></a><span data-ttu-id="64b4a-103">Vytvoření image Azure RemoteAppu</span><span class="sxs-lookup"><span data-stu-id="64b4a-103">Create an Azure RemoteApp image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="64b4a-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="64b4a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="64b4a-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="64b4a-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="64b4a-106">Azure RemoteApp používá Image s aplikacemi, které můžete sdílet s uživateli.</span><span class="sxs-lookup"><span data-stu-id="64b4a-106">Azure RemoteApp uses images to hold the apps that you share with your users.</span></span> <span data-ttu-id="64b4a-107">(Jsme trvat bitové kopie a použít ho k vytvoření virtuálních počítačů -, které je přístup uživatelé při zápisu do Azure RemoteApp.) Pomocí zvoleného aplikací, vytvořit kolekci Azure RemoteApp, jestli je Cloudová nebo hybridní, začněte vytvořením obrázku s těmito aplikacemi nainstalována.</span><span class="sxs-lookup"><span data-stu-id="64b4a-107">(We take your image and use it to create VMs - that's what the users access when they sign into Azure RemoteApp.) To create an Azure RemoteApp collection with your choice of applications, whether it is cloud or hybrid, you  start by creating an image with those applications installed.</span></span> <span data-ttu-id="64b4a-108">Pak vytvořte kolekci, která použije této bitové kopie, přiřazení uživatelů ke kolekci a publikování aplikací pro tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="64b4a-108">Then, create a collection that uses that image, assign users to the collection, and publish apps to those users.</span></span>

<span data-ttu-id="64b4a-109">Máte několik možností pro vytvoření nebo pomocí bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="64b4a-109">You have several options for creating or using images.</span></span> <span data-ttu-id="64b4a-110">Základní [požadavek](remoteapp-imagereqs.md) pro bitovou kopii je, že se systémem Windows Server 2012 R2 a role Hostitel relace vzdálené plochy (RDSH) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="64b4a-110">The basic [requirement](remoteapp-imagereqs.md) for an image is that it run Windows Server 2012 R2 and have the Remote Desktop Session Host (RDSH) role installed.</span></span> <span data-ttu-id="64b4a-111">Jak získat, je, kde získat zajímavé věcí.</span><span class="sxs-lookup"><span data-stu-id="64b4a-111">How you get that is where things get interesting.</span></span>

<span data-ttu-id="64b4a-112">Pokud jde o Image máte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="64b4a-112">You have the following options when it comes to images:</span></span>

* <span data-ttu-id="64b4a-113">Můžete importovat a používat [na základě bitové kopie na virtuální počítač Azure](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="64b4a-113">You can import and use an [image based on an Azure virtual machine](remoteapp-image-on-azurevm.md).</span></span> <span data-ttu-id="64b4a-114">Toto je vhodný pro-obchodní aplikace, které vyžadují vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="64b4a-114">This is good for line-of-business apps that require custom settings.</span></span> <span data-ttu-id="64b4a-115">Můžete přizpůsobit obrázek, který má fungovat pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64b4a-115">You can customize the image to work for the app.</span></span>
* <span data-ttu-id="64b4a-116">Můžete [vytvořit a nahrát vlastní image](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="64b4a-116">You can [create and upload a custom image](remoteapp-create-custom-image.md).</span></span> <span data-ttu-id="64b4a-117">To je vhodný, pokud již máte obrázek, který používáte pro vaše nasazení služby Vzdálená plocha na místě.</span><span class="sxs-lookup"><span data-stu-id="64b4a-117">This is good if you already have an image that you use for your on-premises Remote Desktop Services deployment.</span></span>
* <span data-ttu-id="64b4a-118">Můžete použít jednu z [Image šablony](remoteapp-images.md) součástí vašeho předplatného vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="64b4a-118">You can use one of the [template images](remoteapp-images.md) included in your RemoteApp subscription.</span></span> <span data-ttu-id="64b4a-119">Tyto bitové kopie se vytvoří a udržuje tým vzdálené aplikace RemoteApp a obsahovat některé standardní aplikace (např. v sadě Office), které můžete zpřístupnit uživatelům.</span><span class="sxs-lookup"><span data-stu-id="64b4a-119">These images are created and maintained by the RemoteApp team and contain some standard applications (like the Office suite) that you can make available to your users.</span></span> <span data-ttu-id="64b4a-120">Všimněte si, že lze použít pouze image Office 365 Pro Plus v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="64b4a-120">Note that only the Office 365 Pro Plus image can be used in a production setting.</span></span>

<span data-ttu-id="64b4a-121">Bez ohledu na to, kde získat bitové kopie nebo jak vytvořit, budete chtít musíte rozumět [požadavky aplikací na](remoteapp-appreqs.md) zajistit, že aplikace funguje dobře v Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="64b4a-121">Regardless of where you get your image or how you create it, you'll want to make sure you understand the [app requirements](remoteapp-appreqs.md) to ensure that your app works well in RemoteApp.</span></span> <span data-ttu-id="64b4a-122">Pak je dalším krokem vytvoření [cloudu](remoteapp-create-cloud-deployment.md) nebo [hybridní](remoteapp-create-hybrid-deployment.md) kolekce.</span><span class="sxs-lookup"><span data-stu-id="64b4a-122">Then, the next step is to create a [cloud](remoteapp-create-cloud-deployment.md) or [hybrid](remoteapp-create-hybrid-deployment.md) collection.</span></span>

