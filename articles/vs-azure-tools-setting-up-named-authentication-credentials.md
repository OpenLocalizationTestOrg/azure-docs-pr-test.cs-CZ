---
title: "aaaSetting si pověření ověřování s názvem | Microsoft Docs"
description: "Zjistěte, jak tootooprovide přihlašovací údaje, sady Visual Studio můžete použít tooauthenticate požadavky tooAzure toopublish tooAzure aplikace z Visual Studio nebo toomonitor existující cloudové služby... "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 3cc147a2f7a3e766293ca282f56078cf24281346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-named-authentication-credentials"></a><span data-ttu-id="be7bf-103">Nastavení pověření s názvem ověřování</span><span class="sxs-lookup"><span data-stu-id="be7bf-103">Setting Up Named Authentication Credentials</span></span>
<span data-ttu-id="be7bf-104">toopublish tooAzure aplikace z Visual Studio nebo toomonitor stávající cloudovou službu, je třeba zadat pověření, Visual Studio můžete použít tooauthenticate požadavky tooAzure.</span><span class="sxs-lookup"><span data-stu-id="be7bf-104">toopublish an application tooAzure from Visual Studio or toomonitor an existing cloud service, you must provide credentials that Visual Studio can use tooauthenticate requests tooAzure.</span></span> <span data-ttu-id="be7bf-105">Existuje několik míst v sadě Visual Studio, kde se můžete přihlásit tooprovide tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="be7bf-105">There are several places in Visual Studio where you can sign in tooprovide these credentials.</span></span> <span data-ttu-id="be7bf-106">Například z hello Průzkumníka serveru, můžete otevřít hello místní nabídku pro hello **Azure** uzel a zvolte **připojení tooMicrosoft předplatné Azure...** . Při přihlášení, informace o předplatném hello přidružené k účtu Azure je k dispozici v sadě Visual Studio a není nic jiného, že máte toodo.</span><span class="sxs-lookup"><span data-stu-id="be7bf-106">For example, from hello Server Explorer, you can open hello shortcut menu for hello **Azure** node and choose **Connect tooMicrosoft Azure Subscription...**. When you sign in, hello subscription information associated with your Azure account is available in Visual Studio, and there is nothing more you have toodo.</span></span>

<span data-ttu-id="be7bf-107">Nástroje Azure také podporuje starší způsob, jak poskytnout přihlašovací údaje, pomocí hello předplatné soubor (soubor .publishsettings).</span><span class="sxs-lookup"><span data-stu-id="be7bf-107">Azure Tools also supports an older way of providing credentials, using hello subscription file (.publishsettings file).</span></span> <span data-ttu-id="be7bf-108">Toto téma popisuje tuto metodu, který ještě není podporovaný v Azure SDK 2.2.</span><span class="sxs-lookup"><span data-stu-id="be7bf-108">This topic describes this method, which is still supported in Azure SDK 2.2.</span></span>

<span data-ttu-id="be7bf-109">jsou požadovány pro ověřování tooAzure Hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="be7bf-109">hello following items are required for authentication tooAzure:</span></span>

* <span data-ttu-id="be7bf-110">Vaše ID odběru</span><span class="sxs-lookup"><span data-stu-id="be7bf-110">Your subscription ID</span></span>
* <span data-ttu-id="be7bf-111">Platný certifikát X.509 v3</span><span class="sxs-lookup"><span data-stu-id="be7bf-111">A valid X.509 v3 certificate</span></span>

> [!NOTE]
> <span data-ttu-id="be7bf-112">Délka Hello certifikát X.509 v3 hello klíče musí být alespoň 2 048 bitů.</span><span class="sxs-lookup"><span data-stu-id="be7bf-112">hello length of hello X.509 v3 certificate's key must be at least 2048 bits.</span></span> <span data-ttu-id="be7bf-113">Azure odmítnou libovolný certifikát, který nesplňuje tento požadavek nebo který není platný.</span><span class="sxs-lookup"><span data-stu-id="be7bf-113">Azure will reject any certificate that doesn’t meet this requirement or that isn’t valid.</span></span>
>
>

<span data-ttu-id="be7bf-114">Visual Studio použije svoje ID předplatného společně s hello data certifikátu jako přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="be7bf-114">Visual Studio uses your subscription ID together with hello certificate data as credentials.</span></span> <span data-ttu-id="be7bf-115">Hello příslušná pověření se odkazuje v hello předplatné soubor (.publishsettings), který obsahuje veřejný klíč pro certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="be7bf-115">hello appropriate credentials are referenced in hello subscription file (.publishsettings file), which contains a public key for hello certificate.</span></span> <span data-ttu-id="be7bf-116">soubor odběru Hello může obsahovat přihlašovací údaje pro více než jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="be7bf-116">hello subscription file can contain credentials for more than one subscription.</span></span>

<span data-ttu-id="be7bf-117">Můžete upravit informace o předplatném hello z hello **nové či upravit předplatné** dialogové okno, jak je popsáno dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="be7bf-117">You can edit hello subscription information from hello **New/Edit Subscription** dialog box, as explained later in this topic.</span></span>

<span data-ttu-id="be7bf-118">Pokud chcete toocreate certifikát sami, najdete pokyny toohello v [vytvoření a nahrání certifikátu pro správu pro Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) a ručně odeslat hello certifikát toohello [portál Azure classic ](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="be7bf-118">If you want toocreate a certificate yourself, you can refer toohello instructions in [Create and Upload a Management Certificate for Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) and then manually upload hello certificate toohello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>

> [!NOTE]
> <span data-ttu-id="be7bf-119">Tyto přihlašovací údaje, které sada Visual Studio vyžaduje toomanage, které nejsou cloudové služby hello stejné přihlašovací údaje, které jsou požadované tooauthenticate požadavek hello služby Azure storage.</span><span class="sxs-lookup"><span data-stu-id="be7bf-119">These credentials that Visual Studio requires toomanage your cloud services aren’t hello same credentials that are required tooauthenticate a request against hello Azure storage services.</span></span>
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a><span data-ttu-id="be7bf-120">Import, nastavit nebo upravit pověření pro ověření v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be7bf-120">Import, set up, or edit authentication credentials in Visual Studio</span></span>
<span data-ttu-id="be7bf-121">Můžete importovat, nastavit nebo změnit pověření pro ověření v hello **nové předplatné** dialogové okno, které se zobrazí, pokud provádíte hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="be7bf-121">You can import, set up, or modify your authentication credentials in hello **New Subscription** dialog box, which appears if you perform hello following action:</span></span>

* <span data-ttu-id="be7bf-122">V **Průzkumníka serveru**, otevřete hello místní nabídku pro hello **Azure** uzlu, zvolte **spravovat a odběry filtru...** , zvolte hello **certifikáty** kartě a proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="be7bf-122">In **Server Explorer**, open hello shortcut menu for hello **Azure** node, choose **Manage and Filter Subscriptions...**, choose hello **Certificates** tab, and do one of hello following:</span></span>

    * <span data-ttu-id="be7bf-123">Zvolte **importovat** tooopen hello předplatná Microsoft Azure Import dialogové okno kde soubor můžete stáhnout hello odběry pro hello aktuálně načíst předplatného, procházení tooits umístění souboru ke stažení a naimportujte ho pro použití v ověřování.</span><span class="sxs-lookup"><span data-stu-id="be7bf-123">Choose **Import** tooopen hello Import Microsoft Azure Subscriptions dialog where you can download hello  subscriptions file for hello currently loaded subscription, browse tooits download location, and then import it for use in authentication.</span></span>
    * <span data-ttu-id="be7bf-124">Zvolte **nový** tooopen hello nové předplatné dialogu, kde můžete nastavit nový odběr pro použití v ověřování.</span><span class="sxs-lookup"><span data-stu-id="be7bf-124">Choose **New** tooopen hello New Subscription dialog where you can set up a new subscription for use in authentication.</span></span>
    * <span data-ttu-id="be7bf-125">Zvolte **upravit** (po výběru aktivní předplatné) hello dialogové okno Upravit předplatné tooopen kde můžete upravit stávající předplatné pro použití při ověřování.</span><span class="sxs-lookup"><span data-stu-id="be7bf-125">Choose **Edit** (after choosing your active subscription) tooopen hello Edit Subscription dialog where you can edit an existing subscription for use in authentication.</span></span> 

<span data-ttu-id="be7bf-126">Hello následujícího postupu se předpokládá, že hello **nové předplatné** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="be7bf-126">hello following procedure assumes that hello **New Subscription** dialog box is open.</span></span>

### <a name="tooset-up-authentication-credentials-in-visual-studio"></a><span data-ttu-id="be7bf-127">tooset si pověření pro ověření v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be7bf-127">tooset up authentication credentials in Visual Studio</span></span>
1. <span data-ttu-id="be7bf-128">V hello **vybrat existující certifikát** pro ověření seznamu zvolte certifikát.</span><span class="sxs-lookup"><span data-stu-id="be7bf-128">In hello **Select an existing certificate** for authentication list, choose a certificate.</span></span>
2. <span data-ttu-id="be7bf-129">Zvolte hello **kopírovat hello úplnou cestu** odkaz.</span><span class="sxs-lookup"><span data-stu-id="be7bf-129">Choose hello **Copy hello full path** link.</span></span> <span data-ttu-id="be7bf-130">Cesta Hello hello certifikát (soubor .cer) je zkopírovaný toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="be7bf-130">hello path for hello certificate (.cer file) is copied toohello Clipboard.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="be7bf-131">toopublish vaše aplikace Azure ze sady Visual Studio, musíte nahrát tento certifikát toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="be7bf-131">toopublish your Azure application from Visual Studio, you must upload this certificate toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
   >
   >
3. <span data-ttu-id="be7bf-132">tooupload hello certifikát toohello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="be7bf-132">tooupload hello certificate toohello Azure portal:</span></span>

   1. <span data-ttu-id="be7bf-133">Otevřete hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="be7bf-133">Open hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
   2. <span data-ttu-id="be7bf-134">Pokud budete vyzváni, přihlaste se toohello portál a pak přejděte příliš**nastavení**, **certifikáty pro správu**.</span><span class="sxs-lookup"><span data-stu-id="be7bf-134">If prompted, sign in toohello portal and then navigate too**Settings**, **Management Certificates**.</span></span>
   3. <span data-ttu-id="be7bf-135">V podokně Správa certifikátů hello, zvolte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="be7bf-135">In hello Management certificates pane, choose **Upload**.</span></span>
   4. <span data-ttu-id="be7bf-136">Vyberte předplatné Azure, vložte hello úplnou cestu hello soubor .cer, který jste právě vytvořili a zvolte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="be7bf-136">Select your Azure subscription, paste hello full path of hello .cer file that you just created, and choose **Upload**.</span></span>
