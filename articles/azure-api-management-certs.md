---
title: "aaaUpload certifikát správy rozhraní API služby Azure | Microsoft Docs"
description: "Zjistěte, jak tooupload athe rozhraní API pro správu certifikátů pro hello portálu Azure Classic."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 8294d7131cfb01dba664bd4fd04b6fc22c1e93ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a><span data-ttu-id="b5616-103">Nahrajte certifikát pro správu Azure rozhraní API pro správu</span><span class="sxs-lookup"><span data-stu-id="b5616-103">Upload an Azure Management API Management Certificate</span></span>
<span data-ttu-id="b5616-104">Certifikáty pro správu povolit tooauthenticate s modelem nasazení classic hello poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="b5616-104">Management certificates allow you tooauthenticate with hello classic deployment model provided by Azure.</span></span> <span data-ttu-id="b5616-105">Mnoho programy a nástroje (například Visual Studio nebo hello Azure SDK) používat tyto certifikáty tooautomate konfigurace a nasazení různých služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="b5616-105">Many programs and tools (such as Visual Studio or hello Azure SDK) use these certificates tooautomate configuration and deployment of various Azure services.</span></span> 

> [!WARNING]
> <span data-ttu-id="b5616-106">Dej si pozor!</span><span class="sxs-lookup"><span data-stu-id="b5616-106">Be careful!</span></span> <span data-ttu-id="b5616-107">Tyto typy certifikátů povolit všem uživatelům, kteří s nimi ověřuje toomanage hello předplatného jsou přidruženy.</span><span class="sxs-lookup"><span data-stu-id="b5616-107">These types of certificates allow anyone who authenticates with them toomanage hello subscription they are associated with.</span></span>
>
>

<span data-ttu-id="b5616-108">Pokud vás zajímají další informace o Azure certifikáty (včetně vytváření certifikát podepsaný svým držitelem), najdete v části [Přehled certifikátů pro Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span><span class="sxs-lookup"><span data-stu-id="b5616-108">If you'd like more information about Azure certificates (including creating a self-signed certificate), see [Certificates overview for Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span></span>

<span data-ttu-id="b5616-109">Můžete také použít [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) kód klienta tooauthenticate za účelem automatizace.</span><span class="sxs-lookup"><span data-stu-id="b5616-109">You can also use [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate client-code for automation purposes.</span></span>

## <a name="upload-a-management-certificate"></a><span data-ttu-id="b5616-110">Nahrání certifikátu pro správu</span><span class="sxs-lookup"><span data-stu-id="b5616-110">Upload a management certificate</span></span>
<span data-ttu-id="b5616-111">Jakmile je certifikát správy vytvořený, (soubor .cer s pouze veřejný klíč hello) nahrajte ho do portálu hello.</span><span class="sxs-lookup"><span data-stu-id="b5616-111">Once you have a management certificate created, (.cer file with only hello public key) you can upload it into hello portal.</span></span> <span data-ttu-id="b5616-112">Pokud je k dispozici v portálu hello hello certifikát, každý, kdo má odpovídající certifikátu (privátní klíč) připojovat prostřednictvím hello rozhraní API pro správu a přístup k prostředkům hello hello přidruženého odběru.</span><span class="sxs-lookup"><span data-stu-id="b5616-112">When hello certificate is available in hello portal, anyone with a matching certificate (private key) can connect through hello Management API and access hello resources for hello associated subscription.</span></span>

1. <span data-ttu-id="b5616-113">Přihlaste se toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b5616-113">Log in toohello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="b5616-114">Klikněte na tlačítko **další služby** v seznamu služeb Azure dolní hello, pak vyberte **odběry** v hello _Obecné_ skupinu služeb.</span><span class="sxs-lookup"><span data-stu-id="b5616-114">Click **More services** at hello bottom Azure service list, then select **Subscriptions** in hello _General_ service group.</span></span>

    ![Předplatné nabídky](./media/azure-api-management-certs/subscriptions_menu.png)

3. <span data-ttu-id="b5616-116">Zajistěte, aby tooselect hello správné předplatné, které chcete tooassociate certifikátem hello.</span><span class="sxs-lookup"><span data-stu-id="b5616-116">Make sure tooselect hello correct subscription that you want tooassociate with hello certificate.</span></span>     
4. <span data-ttu-id="b5616-117">Po výběru správné předplatné hello stiskněte **certifikáty pro správu** v hello _nastavení_ skupiny.</span><span class="sxs-lookup"><span data-stu-id="b5616-117">After you have selected hello correct subscription, press **Management certificates** in hello _Settings_ group.</span></span>

    ![Nastavení](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. <span data-ttu-id="b5616-119">Stiskněte klávesu hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5616-119">Press hello **Upload** button.</span></span>

    ![Nahrát na stránce certifikáty](./media/azure-api-management-certs/certificates_page.png)
6. <span data-ttu-id="b5616-121">Vyplňte informace hello dialogové okno a stiskněte klávesu **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="b5616-121">Fill out hello dialog information and press **Upload**.</span></span>

    ![Nastavení](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a><span data-ttu-id="b5616-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5616-123">Next steps</span></span>
<span data-ttu-id="b5616-124">Teď, když máte certifikát pro správu přidružené předplatné, můžete (po instalaci hello certifikát, se místně) prostřednictvím kódu programu připojit toohello [modelu nasazení classic REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) a automatizovat Hello různé prostředky Azure, které jsou také přidružené tomuto předplatnému.</span><span class="sxs-lookup"><span data-stu-id="b5616-124">Now that you have a management certificate associated with a subscription, you can (after you have installed hello matching certificate locally) programmatically connect toohello [classic deployment model REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) and automate hello various Azure resources that are also associated with that subscription.</span></span>
