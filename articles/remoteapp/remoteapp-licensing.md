---
title: "licencování vzdálené aplikace RemoteApp aaaAzure | Microsoft Docs"
description: "Přečtěte si, jak funguje licencování v Azure RemoteAppu."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: ff8ebd20-61a1-4f10-87a6-234a170534c9
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: dfa808a65ea6b1a78bf74f3daddb9a84e186eebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-licensing-work-in-azure-remoteapp"></a><span data-ttu-id="f729a-103">Jak v Azure RemoteAppu funguje licencování?</span><span class="sxs-lookup"><span data-stu-id="f729a-103">How does licensing work in Azure RemoteApp?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f729a-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="f729a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f729a-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f729a-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="f729a-106">Abyste nastavili službu Azure RemoteApp, vytvořili šablony a jsou připravené toopublish aplikace tooyour uživatelé.</span><span class="sxs-lookup"><span data-stu-id="f729a-106">So you've set up your Azure RemoteApp service, created your templates, and are ready toopublish apps tooyour users.</span></span> <span data-ttu-id="f729a-107">Ale je stále jednu poslední věcí toofigure out: licencování.</span><span class="sxs-lookup"><span data-stu-id="f729a-107">But there's still one last thing toofigure out: licensing.</span></span> <span data-ttu-id="f729a-108">Právě jak funguje licencování pro RemoteApp a pro hello aplikace, které sdílíte prostřednictvím Remoteappu?</span><span class="sxs-lookup"><span data-stu-id="f729a-108">Just how does licensing work for RemoteApp and for hello apps you share through RemoteApp?</span></span>

<span data-ttu-id="f729a-109">RemoteApp nevyžaduje žádné licence Windows ani licence CAL pro Vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="f729a-109">RemoteApp does not require any Windows licenses or Remote Desktop CALs.</span></span> <span data-ttu-id="f729a-110">Vaše předplatné má na starosti hello RemoteApp pokryje sám sebe.</span><span class="sxs-lookup"><span data-stu-id="f729a-110">Your subscription takes care of hello RemoteApp side itself.</span></span> <span data-ttu-id="f729a-111">(Podívejte se na podrobnosti o hello hello [cenových plánů](https://azure.microsoft.com/pricing/details/remoteapp).)</span><span class="sxs-lookup"><span data-stu-id="f729a-111">(Check out hello details of hello [pricing plans](https://azure.microsoft.com/pricing/details/remoteapp).)</span></span>

<span data-ttu-id="f729a-112">Pokud použijete jeden hello bitových kopií, které jsou součástí vašeho předplatného, můžete sdílet jakékoli aplikace hello nainstalované v tomto imagi bez nutnosti samostatné licence.</span><span class="sxs-lookup"><span data-stu-id="f729a-112">If you use one of hello images that is included in your subscription, you can share any of hello apps installed on that image without needing a separate license.</span></span> <span data-ttu-id="f729a-113">Například pokud používáte toobuild image šablony Windows Server 2012 R2 hello kolekce, můžete sdílet System Center Endpoint Protection s uživateli.</span><span class="sxs-lookup"><span data-stu-id="f729a-113">For example, if you use hello Windows Server 2012 R2 template image toobuild your collection, you can share System Center Endpoint Protection with your users.</span></span> <span data-ttu-id="f729a-114">Hello pouze toothis pravidlo výjimky jsou Office 365 ProPlus, který vyžaduje samostatné předplatné, a Office 2013, který nelze sdílet v produkční kolekci.</span><span class="sxs-lookup"><span data-stu-id="f729a-114">hello only exceptions toothis rule are Office 365 ProPlus, which requires a separate subscription, and Office 2013, which cannot be shared in a production collection.</span></span>

<span data-ttu-id="f729a-115">Pokud chcete, aby image šablony Office 365 hello toouse, která se dodává s Remoteappem, musíte mít *existující* plán Office 365 ProPlus.</span><span class="sxs-lookup"><span data-stu-id="f729a-115">If you want toouse hello Office 365 template image that comes with RemoteApp, you must have an *existing* Office 365 ProPlus plan.</span></span> <span data-ttu-id="f729a-116">Hello totéž platí pro všechny aplikace Office 365 publikujete pomocí vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="f729a-116">hello same is true for any Office 365 app that you publish using a custom template.</span></span> <span data-ttu-id="f729a-117">Je třeba aplikace hello tooactivate pomocí svého vlastního předplatného.</span><span class="sxs-lookup"><span data-stu-id="f729a-117">You need tooactivate hello apps with your own subscription.</span></span> <span data-ttu-id="f729a-118">To platí pro zkušební i placené předplatné.</span><span class="sxs-lookup"><span data-stu-id="f729a-118">This is true for both trial and paid subscriptions.</span></span> <span data-ttu-id="f729a-119">Pokud chcete image šablony toouse hello Office 365 během zkušební doby hello, *a ještě nemáte předplatné*, přejděte toohello Office 365 stránku příliš[zaregistrovat](https://go.microsoft.com/fwlink/p/?LinkID=403802) zkušební předplatné.</span><span class="sxs-lookup"><span data-stu-id="f729a-119">If you want toouse hello Office 365 template image during hello trial, *and you don't already have a subscription*, go toohello Office 365 page too[sign up](https://go.microsoft.com/fwlink/p/?LinkID=403802) for a trial subscription.</span></span> <span data-ttu-id="f729a-120">Další informace najdete v článku [Jak spolu fungují RemoteApp a Office?](remoteapp-o365.md)</span><span class="sxs-lookup"><span data-stu-id="f729a-120">See [How do RemoteApp and Office work together?](remoteapp-o365.md) for more information.</span></span>

<span data-ttu-id="f729a-121">Pokud během zkušební doby hello, nechcete, aby zkušební předplatné tooget Office 365, použijte image šablony Office 2013 Professional Plus hello, která se dodává s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="f729a-121">If, during hello trial period, you don't want tooget an Office 365 trial subscription, use hello Office 2013 Professional Plus template image that comes with RemoteApp.</span></span> <span data-ttu-id="f729a-122">Tento image šablony lze používat pouze 30 dnů a nelze jej převést na placenou kolekci.</span><span class="sxs-lookup"><span data-stu-id="f729a-122">This template image can only be used for 30 days and cannot be converted into a paid collection.</span></span>

<span data-ttu-id="f729a-123">Pro jiné aplikace budete potřebovat toomake, že máte hello licence tooshare hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f729a-123">For other apps, you need toomake sure that you have hello license tooshare hello app.</span></span>

<span data-ttu-id="f729a-124">To dává smysl, ne?</span><span class="sxs-lookup"><span data-stu-id="f729a-124">That makes sense, right?</span></span> <span data-ttu-id="f729a-125">Můžete publikovat v jakékoli aplikaci, že máte nárok právními předpisy tooshare.</span><span class="sxs-lookup"><span data-stu-id="f729a-125">You can publish any app that you are legally entitled tooshare.</span></span> <span data-ttu-id="f729a-126">A je třeba toomake, že jste skutečně právo tooshare programy.</span><span class="sxs-lookup"><span data-stu-id="f729a-126">And you need toomake sure you really are entitled tooshare your programs.</span></span>

<span data-ttu-id="f729a-127">Upozorňujeme, že v cloudové kolekci nemůžete používat licence CAL ani multilicenční smlouvu.</span><span class="sxs-lookup"><span data-stu-id="f729a-127">Please note that you cannot use a CAL or Volume License agreement in a cloud collection.</span></span> <span data-ttu-id="f729a-128">Můžete *můžete* používání multilicenční smlouvy tooactivate aplikací v hybridní kolekci (s výjimkou Office).</span><span class="sxs-lookup"><span data-stu-id="f729a-128">You *can* use a Volume License agreement tooactivate applications in your hybrid collection (except for Office).</span></span> <span data-ttu-id="f729a-129">Stačí tooinstall je v šabloně obrázek z hello multilicenčního média.</span><span class="sxs-lookup"><span data-stu-id="f729a-129">You just need tooinstall them on your template image from hello Volume License media.</span></span> <span data-ttu-id="f729a-130">Podle pokynů hello informace hello aplikace dodavatele tooinstall licence v prostředí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="f729a-130">Follow hello information from hello application vendor tooinstall licenses in a Remote Desktop environment.</span></span>
