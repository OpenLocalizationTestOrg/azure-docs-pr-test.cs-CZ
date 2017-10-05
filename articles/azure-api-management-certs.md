---
title: "Nahrát na server certifikát rozhraní API správy Azure | Microsoft Docs"
description: "Naučte se nahrát athe rozhraní API pro správu certifikátů pro portál Azure Classic."
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
ms.openlocfilehash: 9dc438e927acd9aef38f06807fabf3dda9b021c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a><span data-ttu-id="a477f-103">Nahrajte certifikát pro správu Azure rozhraní API pro správu</span><span class="sxs-lookup"><span data-stu-id="a477f-103">Upload an Azure Management API Management Certificate</span></span>
<span data-ttu-id="a477f-104">Certifikáty pro správu umožňují ověření pomocí modelu nasazení classic poskytovaný platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="a477f-104">Management certificates allow you to authenticate with the classic deployment model provided by Azure.</span></span> <span data-ttu-id="a477f-105">Mnoho programy a nástroje (například Visual Studio nebo sadu Azure SDK) použít tyto certifikáty k automatizaci konfigurace a nasazení různých služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="a477f-105">Many programs and tools (such as Visual Studio or the Azure SDK) use these certificates to automate configuration and deployment of various Azure services.</span></span> 

> [!WARNING]
> <span data-ttu-id="a477f-106">Dej si pozor!</span><span class="sxs-lookup"><span data-stu-id="a477f-106">Be careful!</span></span> <span data-ttu-id="a477f-107">Tyto typy certifikátů, povolí všem uživatelům, kteří se mají spravovat předplatné, které jsou přidruženy ověřuje.</span><span class="sxs-lookup"><span data-stu-id="a477f-107">These types of certificates allow anyone who authenticates with them to manage the subscription they are associated with.</span></span>
>
>

<span data-ttu-id="a477f-108">Pokud vás zajímají další informace o Azure certifikáty (včetně vytváření certifikát podepsaný svým držitelem), najdete v části [Přehled certifikátů pro Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span><span class="sxs-lookup"><span data-stu-id="a477f-108">If you'd like more information about Azure certificates (including creating a self-signed certificate), see [Certificates overview for Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span></span>

<span data-ttu-id="a477f-109">Můžete také použít [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) k ověření kódu klienta pro účely automatizace.</span><span class="sxs-lookup"><span data-stu-id="a477f-109">You can also use [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) to authenticate client-code for automation purposes.</span></span>

## <a name="upload-a-management-certificate"></a><span data-ttu-id="a477f-110">Nahrání certifikátu pro správu</span><span class="sxs-lookup"><span data-stu-id="a477f-110">Upload a management certificate</span></span>
<span data-ttu-id="a477f-111">Jakmile je certifikát správy vytvořený, (soubor .cer s pouze veřejný klíč) nahrajte ho do portálu.</span><span class="sxs-lookup"><span data-stu-id="a477f-111">Once you have a management certificate created, (.cer file with only the public key) you can upload it into the portal.</span></span> <span data-ttu-id="a477f-112">Je-li certifikát k dispozici na portálu, každý, kdo má odpovídající certifikátu (privátní klíč) můžete připojit přes rozhraní API pro správu a přístup k prostředkům pro přidružené předplatné.</span><span class="sxs-lookup"><span data-stu-id="a477f-112">When the certificate is available in the portal, anyone with a matching certificate (private key) can connect through the Management API and access the resources for the associated subscription.</span></span>

1. <span data-ttu-id="a477f-113">Přihlaste se k portálu [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a477f-113">Log in to the [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="a477f-114">Klikněte na tlačítko **další služby** v seznamu dolní služby Azure, pak vyberte **odběry** v _Obecné_ skupinu služeb.</span><span class="sxs-lookup"><span data-stu-id="a477f-114">Click **More services** at the bottom Azure service list, then select **Subscriptions** in the _General_ service group.</span></span>

    ![Předplatné nabídky](./media/azure-api-management-certs/subscriptions_menu.png)

3. <span data-ttu-id="a477f-116">Ujistěte se, že vyberte správné předplatné, který chcete přidružit k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a477f-116">Make sure to select the correct subscription that you want to associate with the certificate.</span></span>     
4. <span data-ttu-id="a477f-117">Po výběru správné předplatné, stiskněte klávesu **certifikáty pro správu** v _nastavení_ skupiny.</span><span class="sxs-lookup"><span data-stu-id="a477f-117">After you have selected the correct subscription, press **Management certificates** in the _Settings_ group.</span></span>

    ![Nastavení](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. <span data-ttu-id="a477f-119">Stiskněte **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a477f-119">Press the **Upload** button.</span></span>

    ![Nahrát na stránce certifikáty](./media/azure-api-management-certs/certificates_page.png)
6. <span data-ttu-id="a477f-121">Vyplňte informace dialogové okno a stiskněte klávesu **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="a477f-121">Fill out the dialog information and press **Upload**.</span></span>

    ![Nastavení](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a><span data-ttu-id="a477f-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a477f-123">Next steps</span></span>
<span data-ttu-id="a477f-124">Teď, když máte certifikát pro správu přidružené předplatné, můžete (po instalaci odpovídající certifikátu místně) prostřednictvím kódu programu připojit k [modelu nasazení classic REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) a automatizovat různé prostředky Azure, které jsou také přidružené tomuto předplatnému.</span><span class="sxs-lookup"><span data-stu-id="a477f-124">Now that you have a management certificate associated with a subscription, you can (after you have installed the matching certificate locally) programmatically connect to the [classic deployment model REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) and automate the various Azure resources that are also associated with that subscription.</span></span>
