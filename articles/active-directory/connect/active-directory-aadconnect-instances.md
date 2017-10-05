---
title: "Azure AD Connect: Instance služby synchronizace | Microsoft Docs"
description: "Tato stránka dokumenty zvláštní upozornění pro instancí služby Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a><span data-ttu-id="40434-103">Azure AD Connect: Zvláštní upozornění pro instance</span><span class="sxs-lookup"><span data-stu-id="40434-103">Azure AD Connect: Special considerations for instances</span></span>
<span data-ttu-id="40434-104">Azure AD Connect se nejčastěji používá s celosvětové instance služby Azure AD a Office 365.</span><span class="sxs-lookup"><span data-stu-id="40434-104">Azure AD Connect is most commonly used with the world-wide instance of Azure AD and Office 365.</span></span> <span data-ttu-id="40434-105">Ale existují také další instance a ty mají různé požadavky na adresy URL a další důležité.</span><span class="sxs-lookup"><span data-stu-id="40434-105">But there are also other instances and these have different requirements for URLs and other special considerations.</span></span>

## <a name="microsoft-cloud-germany"></a><span data-ttu-id="40434-106">Microsoft Cloud Germany</span><span class="sxs-lookup"><span data-stu-id="40434-106">Microsoft Cloud Germany</span></span>
<span data-ttu-id="40434-107">[Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) je svrchovaných Cloudová provozují zplnomocněnec němčině data.</span><span class="sxs-lookup"><span data-stu-id="40434-107">The [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) is a sovereign cloud operated by a German data trustee.</span></span>

| <span data-ttu-id="40434-108">Adresy URL otevřete v proxy serveru</span><span class="sxs-lookup"><span data-stu-id="40434-108">URLs to open in proxy server</span></span> |
| --- |
| <span data-ttu-id="40434-109">\*. microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="40434-109">\*.microsoftonline.de</span></span> |
| <span data-ttu-id="40434-110">\*.windows.net</span><span class="sxs-lookup"><span data-stu-id="40434-110">\*.windows.net</span></span> |
| <span data-ttu-id="40434-111">+ Seznamy odvolaných certifikátů</span><span class="sxs-lookup"><span data-stu-id="40434-111">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="40434-112">Když se přihlásíte ke klientovi Azure AD, musíte použít účet v doméně onmicrosoft.de.</span><span class="sxs-lookup"><span data-stu-id="40434-112">When you sign in to your Azure AD tenant, you must use an account in the onmicrosoft.de domain.</span></span>

<span data-ttu-id="40434-113">Funkce aktuálně nejsou k dispozici v Německu cloudu společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="40434-113">Features currently not present in the Microsoft Cloud Germany:</span></span>

* <span data-ttu-id="40434-114">**Azure AD Connect Health** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="40434-114">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="40434-115">**Funkce Automatické aktualizace** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="40434-115">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="40434-116">**Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.</span><span class="sxs-lookup"><span data-stu-id="40434-116">**Password writeback** is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="40434-117">Nejsou k dispozici další služby Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="40434-117">Other Azure AD Premium services are not available.</span></span>

## <a name="microsoft-azure-government-cloud"></a><span data-ttu-id="40434-118">Cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="40434-118">Microsoft Azure Government cloud</span></span>
<span data-ttu-id="40434-119">[Cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/) je cloud pro US government.</span><span class="sxs-lookup"><span data-stu-id="40434-119">The [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is a cloud for US government.</span></span>

<span data-ttu-id="40434-120">Tento cloud byl podporován ve starších verzích nástroje DirSync.</span><span class="sxs-lookup"><span data-stu-id="40434-120">This cloud has been supported by earlier releases of DirSync.</span></span> <span data-ttu-id="40434-121">Ze sestavení 1.1.180 služby Azure AD Connect je podporována nové generace cloudu.</span><span class="sxs-lookup"><span data-stu-id="40434-121">From build 1.1.180 of Azure AD Connect, the next generation of the cloud is supported.</span></span> <span data-ttu-id="40434-122">Generování používá jen USA založené na koncových bodů a mít jiný seznam adres URL, otevřete v proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="40434-122">This generation is using US-only based endpoints and have a different list of URLs to open in your proxy server.</span></span>

| <span data-ttu-id="40434-123">Adresy URL otevřete v proxy serveru</span><span class="sxs-lookup"><span data-stu-id="40434-123">URLs to open in proxy server</span></span> |
| --- |
| <span data-ttu-id="40434-124">\*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="40434-124">\*.microsoftonline.com</span></span> |
| <span data-ttu-id="40434-125">\*. microsoftonline.us</span><span class="sxs-lookup"><span data-stu-id="40434-125">\*.microsoftonline.us</span></span> |
| <span data-ttu-id="40434-126">\*. gov.us.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="40434-126">\*.gov.us.microsoftonline.com</span></span> |
| <span data-ttu-id="40434-127">+ Seznamy odvolaných certifikátů</span><span class="sxs-lookup"><span data-stu-id="40434-127">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="40434-128">Azure AD Connect není schopna automaticky zjistit, že klientovi Azure AD je umístěný v cloudu Government.</span><span class="sxs-lookup"><span data-stu-id="40434-128">Azure AD Connect is not able to automatically detect that your Azure AD tenant is located in the Government cloud.</span></span> <span data-ttu-id="40434-129">Místo toho musíte provést následující akce při instalaci Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="40434-129">Instead you need to take the following actions when you install Azure AD Connect.</span></span>

1. <span data-ttu-id="40434-130">Spusťte instalaci Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="40434-130">Start the Azure AD Connect installation.</span></span>
2. <span data-ttu-id="40434-131">Když se zobrazí na první stránku, kde je mají přijetí smlouvy EULA, nebudete pokračovat, ale ponechte spuštění Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="40434-131">When you see the first page where you are supposed to accept the EULA, do not continue but leave the installation wizard running.</span></span>
3. <span data-ttu-id="40434-132">Spustit program regedit a změňte klíč registru `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` na hodnotu `2`.</span><span class="sxs-lookup"><span data-stu-id="40434-132">Start regedit and change the registry key `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` to the value `2`.</span></span>
4. <span data-ttu-id="40434-133">Přejděte zpět na Průvodce instalací služby Azure AD Connect, přijetí smlouvy EULA a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="40434-133">Go back to the Azure AD Connect installation wizard, accept the EULA, and continue.</span></span> <span data-ttu-id="40434-134">Během instalace, nezapomeňte použít **vlastní konfigurace** cesta instalace (a ne expresní instalaci).</span><span class="sxs-lookup"><span data-stu-id="40434-134">During installation, make sure to use the **custom configuration** installation path (and not Express installation).</span></span> <span data-ttu-id="40434-135">Potom pokračujte v instalaci jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="40434-135">Then continue the installation as usual.</span></span>

<span data-ttu-id="40434-136">Aktuálně nejsou k dispozici v cloudu Microsoft Azure Government funkce:</span><span class="sxs-lookup"><span data-stu-id="40434-136">Features currently not present in the Microsoft Azure Government cloud:</span></span>

* <span data-ttu-id="40434-137">**Azure AD Connect Health** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="40434-137">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="40434-138">**Funkce Automatické aktualizace** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="40434-138">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="40434-139">**Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.</span><span class="sxs-lookup"><span data-stu-id="40434-139">**Password writeback**  is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="40434-140">Nejsou k dispozici další služby Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="40434-140">Other Azure AD Premium services are not available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="40434-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40434-141">Next steps</span></span>
<span data-ttu-id="40434-142">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="40434-142">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
