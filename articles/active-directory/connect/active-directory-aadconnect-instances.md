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
ms.openlocfilehash: 2a0d8a599cf84cd6530bdbb24951156510d2cf3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a><span data-ttu-id="70add-103">Azure AD Connect: Zvláštní upozornění pro instance</span><span class="sxs-lookup"><span data-stu-id="70add-103">Azure AD Connect: Special considerations for instances</span></span>
<span data-ttu-id="70add-104">Azure AD Connect se nejčastěji používá s hello world wide instanci Azure AD a Office 365.</span><span class="sxs-lookup"><span data-stu-id="70add-104">Azure AD Connect is most commonly used with hello world-wide instance of Azure AD and Office 365.</span></span> <span data-ttu-id="70add-105">Ale existují také další instance a ty mají různé požadavky na adresy URL a další důležité.</span><span class="sxs-lookup"><span data-stu-id="70add-105">But there are also other instances and these have different requirements for URLs and other special considerations.</span></span>

## <a name="microsoft-cloud-germany"></a><span data-ttu-id="70add-106">Microsoft Cloud Germany</span><span class="sxs-lookup"><span data-stu-id="70add-106">Microsoft Cloud Germany</span></span>
<span data-ttu-id="70add-107">Hello [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) je svrchovaných Cloudová provozují zplnomocněnec němčině data.</span><span class="sxs-lookup"><span data-stu-id="70add-107">hello [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) is a sovereign cloud operated by a German data trustee.</span></span>

| <span data-ttu-id="70add-108">Adresy URL tooopen v proxy serveru</span><span class="sxs-lookup"><span data-stu-id="70add-108">URLs tooopen in proxy server</span></span> |
| --- |
| <span data-ttu-id="70add-109">\*. microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="70add-109">\*.microsoftonline.de</span></span> |
| <span data-ttu-id="70add-110">\*.windows.net</span><span class="sxs-lookup"><span data-stu-id="70add-110">\*.windows.net</span></span> |
| <span data-ttu-id="70add-111">+ Seznamy odvolaných certifikátů</span><span class="sxs-lookup"><span data-stu-id="70add-111">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="70add-112">Když se přihlásíte v klientovi Azure AD tooyour, musíte použít účet v doméně onmicrosoft.de hello.</span><span class="sxs-lookup"><span data-stu-id="70add-112">When you sign in tooyour Azure AD tenant, you must use an account in hello onmicrosoft.de domain.</span></span>

<span data-ttu-id="70add-113">Funkce aktuálně nejsou k dispozici v Německu cloudu Microsoft hello:</span><span class="sxs-lookup"><span data-stu-id="70add-113">Features currently not present in hello Microsoft Cloud Germany:</span></span>

* <span data-ttu-id="70add-114">**Azure AD Connect Health** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="70add-114">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="70add-115">**Funkce Automatické aktualizace** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="70add-115">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="70add-116">**Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.</span><span class="sxs-lookup"><span data-stu-id="70add-116">**Password writeback** is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="70add-117">Nejsou k dispozici další služby Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="70add-117">Other Azure AD Premium services are not available.</span></span>

## <a name="microsoft-azure-government-cloud"></a><span data-ttu-id="70add-118">Cloudu Microsoft Azure Government.</span><span class="sxs-lookup"><span data-stu-id="70add-118">Microsoft Azure Government cloud</span></span>
<span data-ttu-id="70add-119">Hello [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/) je cloud pro US government.</span><span class="sxs-lookup"><span data-stu-id="70add-119">hello [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is a cloud for US government.</span></span>

<span data-ttu-id="70add-120">Tento cloud byl podporován ve starších verzích nástroje DirSync.</span><span class="sxs-lookup"><span data-stu-id="70add-120">This cloud has been supported by earlier releases of DirSync.</span></span> <span data-ttu-id="70add-121">Ze sestavení 1.1.180 služby Azure AD Connect hello nové generace hello cloudu je podporována.</span><span class="sxs-lookup"><span data-stu-id="70add-121">From build 1.1.180 of Azure AD Connect, hello next generation of hello cloud is supported.</span></span> <span data-ttu-id="70add-122">Generování používá jen USA založené na koncových bodů a mít jiný seznam adres URL tooopen proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="70add-122">This generation is using US-only based endpoints and have a different list of URLs tooopen in your proxy server.</span></span>

| <span data-ttu-id="70add-123">Adresy URL tooopen v proxy serveru</span><span class="sxs-lookup"><span data-stu-id="70add-123">URLs tooopen in proxy server</span></span> |
| --- |
| <span data-ttu-id="70add-124">\*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="70add-124">\*.microsoftonline.com</span></span> |
| <span data-ttu-id="70add-125">\*. microsoftonline.us</span><span class="sxs-lookup"><span data-stu-id="70add-125">\*.microsoftonline.us</span></span> |
| <span data-ttu-id="70add-126">\*. gov.us.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="70add-126">\*.gov.us.microsoftonline.com</span></span> |
| <span data-ttu-id="70add-127">+ Seznamy odvolaných certifikátů</span><span class="sxs-lookup"><span data-stu-id="70add-127">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="70add-128">Azure AD Connect není možné tooautomatically zjistit, zda váš klient Azure AD je umístěno v cloud vlády hello.</span><span class="sxs-lookup"><span data-stu-id="70add-128">Azure AD Connect is not able tooautomatically detect that your Azure AD tenant is located in hello Government cloud.</span></span> <span data-ttu-id="70add-129">Místo toho musíte tootake hello následující akce při instalaci Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="70add-129">Instead you need tootake hello following actions when you install Azure AD Connect.</span></span>

1. <span data-ttu-id="70add-130">Spuštění instalace služby Azure AD Connect hello.</span><span class="sxs-lookup"><span data-stu-id="70add-130">Start hello Azure AD Connect installation.</span></span>
2. <span data-ttu-id="70add-131">Až uvidíte hello první stránka, kde je mají tooaccept hello smlouvy EULA, nebudete pokračovat, ale ponechte spuštění Průvodce instalací hello.</span><span class="sxs-lookup"><span data-stu-id="70add-131">When you see hello first page where you are supposed tooaccept hello EULA, do not continue but leave hello installation wizard running.</span></span>
3. <span data-ttu-id="70add-132">Spustit program regedit a změňte klíč registru hello `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello hodnotu `2`.</span><span class="sxs-lookup"><span data-stu-id="70add-132">Start regedit and change hello registry key `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello value `2`.</span></span>
4. <span data-ttu-id="70add-133">Vraťte se Průvodce instalací toohello Azure AD Connect, přijměte hello smlouvy EULA a pokračovat.</span><span class="sxs-lookup"><span data-stu-id="70add-133">Go back toohello Azure AD Connect installation wizard, accept hello EULA, and continue.</span></span> <span data-ttu-id="70add-134">Během instalace, ujistěte se, zda text hello toouse **vlastní konfigurace** cesta instalace (a ne expresní instalaci).</span><span class="sxs-lookup"><span data-stu-id="70add-134">During installation, make sure toouse hello **custom configuration** installation path (and not Express installation).</span></span> <span data-ttu-id="70add-135">Pokračujte hello instalace jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="70add-135">Then continue hello installation as usual.</span></span>

<span data-ttu-id="70add-136">Funkce aktuálně nejsou k dispozici v cloudu Microsoft Azure Government hello:</span><span class="sxs-lookup"><span data-stu-id="70add-136">Features currently not present in hello Microsoft Azure Government cloud:</span></span>

* <span data-ttu-id="70add-137">**Azure AD Connect Health** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="70add-137">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="70add-138">**Funkce Automatické aktualizace** není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="70add-138">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="70add-139">**Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.</span><span class="sxs-lookup"><span data-stu-id="70add-139">**Password writeback**  is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="70add-140">Nejsou k dispozici další služby Azure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="70add-140">Other Azure AD Premium services are not available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70add-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70add-141">Next steps</span></span>
<span data-ttu-id="70add-142">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="70add-142">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
