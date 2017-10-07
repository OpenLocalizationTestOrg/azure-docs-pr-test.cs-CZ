---
title: aaaCreate image Azure Remoteappu | Microsoft Docs
description: "Další informace o hello možnosti, které jsou dostupné pro vytváření bitových kopií pro Azure RemoteApp"
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
ms.openlocfilehash: 54e63b6fa13addfcda96ce581910e1ac48d91e70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-remoteapp-image"></a><span data-ttu-id="8a7b9-103">Vytvoření image Azure RemoteAppu</span><span class="sxs-lookup"><span data-stu-id="8a7b9-103">Create an Azure RemoteApp image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8a7b9-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8a7b9-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8a7b9-106">Azure RemoteApp používá aplikace hello toohold bitové kopie, které můžete sdílet s uživateli.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-106">Azure RemoteApp uses images toohold hello apps that you share with your users.</span></span> <span data-ttu-id="8a7b9-107">(Jsme trvat bitové kopie a použít ho virtuálních počítačů toocreate - která hello uživatelé přístup při zápisu do Azure RemoteApp.) toocreate kolekci Azure Remoteappu s zvoleného aplikací, ať už je Cloudová nebo hybridní, začněte vytvořením bitovou kopii s těmito aplikacemi nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-107">(We take your image and use it toocreate VMs - that's what hello users access when they sign into Azure RemoteApp.) toocreate an Azure RemoteApp collection with your choice of applications, whether it is cloud or hybrid, you  start by creating an image with those applications installed.</span></span> <span data-ttu-id="8a7b9-108">Pak vytvořte kolekci, která použije této bitové kopie, přiřazení uživatelů toohello kolekce a publikování aplikace toothose uživatele.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-108">Then, create a collection that uses that image, assign users toohello collection, and publish apps toothose users.</span></span>

<span data-ttu-id="8a7b9-109">Máte několik možností pro vytvoření nebo pomocí bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-109">You have several options for creating or using images.</span></span> <span data-ttu-id="8a7b9-110">Hello základní [požadavek](remoteapp-imagereqs.md) pro bitovou kopii je, že se systémem Windows Server 2012 R2 a role Hostitel relace vzdálené plochy (RDSH) hello nainstalována.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-110">hello basic [requirement](remoteapp-imagereqs.md) for an image is that it run Windows Server 2012 R2 and have hello Remote Desktop Session Host (RDSH) role installed.</span></span> <span data-ttu-id="8a7b9-111">Jak získat, je, kde získat zajímavé věcí.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-111">How you get that is where things get interesting.</span></span>

<span data-ttu-id="8a7b9-112">Máte následující možnosti při přechodu do tooimages hello:</span><span class="sxs-lookup"><span data-stu-id="8a7b9-112">You have hello following options when it comes tooimages:</span></span>

* <span data-ttu-id="8a7b9-113">Můžete importovat a používat [na základě bitové kopie na virtuální počítač Azure](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="8a7b9-113">You can import and use an [image based on an Azure virtual machine](remoteapp-image-on-azurevm.md).</span></span> <span data-ttu-id="8a7b9-114">Toto je vhodný pro-obchodní aplikace, které vyžadují vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-114">This is good for line-of-business apps that require custom settings.</span></span> <span data-ttu-id="8a7b9-115">Můžete přizpůsobit hello toowork bitové kopie pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-115">You can customize hello image toowork for hello app.</span></span>
* <span data-ttu-id="8a7b9-116">Můžete [vytvořit a nahrát vlastní image](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="8a7b9-116">You can [create and upload a custom image](remoteapp-create-custom-image.md).</span></span> <span data-ttu-id="8a7b9-117">To je vhodný, pokud již máte obrázek, který používáte pro vaše nasazení služby Vzdálená plocha na místě.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-117">This is good if you already have an image that you use for your on-premises Remote Desktop Services deployment.</span></span>
* <span data-ttu-id="8a7b9-118">Můžete použít jednu z hello [Image šablony](remoteapp-images.md) součástí vašeho předplatného vzdálené aplikace RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-118">You can use one of hello [template images](remoteapp-images.md) included in your RemoteApp subscription.</span></span> <span data-ttu-id="8a7b9-119">Tyto bitové kopie se vytvoří a udržuje tým služby RemoteApp hello a obsahovat některé standardní aplikace (např. hello sady Office), abyste vytvořili uživatelé k dispozici tooyour.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-119">These images are created and maintained by hello RemoteApp team and contain some standard applications (like hello Office suite) that you can make available tooyour users.</span></span> <span data-ttu-id="8a7b9-120">Všimněte si, že pouze image Office 365 Pro Plus hello může být použit v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-120">Note that only hello Office 365 Pro Plus image can be used in a production setting.</span></span>

<span data-ttu-id="8a7b9-121">Bez ohledu na to, kde získat bitové kopie nebo jak vytvořit, je výhodné rozumět hello toomake [požadavky aplikací na](remoteapp-appreqs.md) tooensure, které vaše aplikace funguje dobře v Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-121">Regardless of where you get your image or how you create it, you'll want toomake sure you understand hello [app requirements](remoteapp-appreqs.md) tooensure that your app works well in RemoteApp.</span></span> <span data-ttu-id="8a7b9-122">Potom hello dalším krokem je toocreate [cloudu](remoteapp-create-cloud-deployment.md) nebo [hybridní](remoteapp-create-hybrid-deployment.md) kolekce.</span><span class="sxs-lookup"><span data-stu-id="8a7b9-122">Then, hello next step is toocreate a [cloud](remoteapp-create-cloud-deployment.md) or [hybrid](remoteapp-create-hybrid-deployment.md) collection.</span></span>

