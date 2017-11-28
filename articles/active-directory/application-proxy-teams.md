---
title: "aaaAccess aplikací Proxy aplikace Azure AD v týmy | Microsoft Docs"
description: "Pomocí proxy aplikace služby Azure AD tooaccess místní aplikaci prostřednictvím Teams společnosti Microsoft."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 13c36e43ae6349df09272e308ad4f40451cbbeb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a><span data-ttu-id="de184-103">Přístup k místním aplikacím přes Teams společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="de184-103">Access your on-premises applications through Microsoft Teams</span></span>

<span data-ttu-id="de184-104">Azure Proxy aplikace Active Directory nabízí jeden přihlašování tooon místní aplikace bez ohledu na to, kde se a Microsoft Teams zjednodušuje vaše spolupráce úsilí na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="de184-104">Azure Active Directory Application Proxy gives you single sign-on tooon-premises applications no matter where you are, and Microsoft Teams streamlines your collaborative efforts in one place.</span></span> <span data-ttu-id="de184-105">Integrace hello dva společně znamená, že vaši uživatelé mohli být produktivní s jejich členy týmu v jakékoliv jiné situaci.</span><span class="sxs-lookup"><span data-stu-id="de184-105">Integrating hello two together means that your users can be productive with their teammates in any situation.</span></span> 

<span data-ttu-id="de184-106">Uživatele můžete přidat cloudové aplikace tootheir týmy kanály [pomocí karty](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), ale co se stane, když je tento web služby SharePoint nebo všechny používají nástroj plánování hostovaný místně?</span><span class="sxs-lookup"><span data-stu-id="de184-106">Your users can add cloud apps tootheir Teams channels [using tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), but what happens if that SharePoint site or planning tool they all use is hosted on-premises?</span></span> <span data-ttu-id="de184-107">Proxy aplikací je hello řešení.</span><span class="sxs-lookup"><span data-stu-id="de184-107">Application Proxy is hello solution.</span></span> <span data-ttu-id="de184-108">Aplikace můžou přidat publikované prostřednictvím Proxy aplikace tootheir hello kanály pomocí stejné adresy URL externí vždy používají tooaccess svoje aplikace vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="de184-108">They can add apps published through Application Proxy tootheir channels using hello same external URLs they always use tooaccess their apps remotely.</span></span> <span data-ttu-id="de184-109">A protože Proxy aplikace ověřuje prostřednictvím Azure Active Directory, hello dodávané stejné jednoho přihlášení prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="de184-109">And because Application Proxy authenticates through Azure Active Directory, hello same single sign-on experience carries through.</span></span>


## <a name="install-hello-application-proxy-connector-and-publish-your-app"></a><span data-ttu-id="de184-110">Instalace konektoru Proxy aplikace hello a publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="de184-110">Install hello Application Proxy connector and publish your app</span></span>

<span data-ttu-id="de184-111">Pokud jste to ještě neudělali, [konfiguraci Proxy aplikace vašeho klienta a instalaci konektoru hello](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="de184-111">If you haven't already, [configure Application Proxy for your tenant and install hello connector](active-directory-application-proxy-enable.md).</span></span> <span data-ttu-id="de184-112">Potom [publikovat místní aplikace](application-proxy-publish-azure-portal.md) pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="de184-112">Then, [publish your on-premises application](application-proxy-publish-azure-portal.md) for remote access.</span></span> <span data-ttu-id="de184-113">Při publikování aplikace hello, poznamenejte si externí adresu URL hello vzhledem k tomu, že koncoví uživatelé musí tyto informace při přidávání tooTeams aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="de184-113">When you're publishing hello app, make note of hello external URL because your end users need that information when they add hello app tooTeams.</span></span>

<span data-ttu-id="de184-114">Pokud již máte aplikace publikována, ale nepamatujete jejich externí adresy URL, vyhledejte je v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de184-114">If you already have your apps published but don't remember their external URLs, look them up in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="de184-115">Přihlaste se a potom přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** > vyberte svou aplikaci > **Proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="de184-115">Sign in, then navigate too**Azure Active Directory** > **Enterprise applications** > **All applications** > select your app > **Application proxy**.</span></span>

## <a name="add-your-app-tooteams"></a><span data-ttu-id="de184-116">Přidat tooTeams vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="de184-116">Add your app tooTeams</span></span>

<span data-ttu-id="de184-117">Jakmile publikujete aplikaci hello prostřednictvím Proxy aplikace, dát uživatelům vědět, že se můžete přidat jako karty přímo v jejich týmy kanály.</span><span class="sxs-lookup"><span data-stu-id="de184-117">Once you publish hello app through Application Proxy, let your users know that they can add it as a tab directly in their Teams channels.</span></span> <span data-ttu-id="de184-118">Kliknul proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="de184-118">Have them follow these three steps:</span></span>

1. <span data-ttu-id="de184-119">Přejděte toohello týmy kanálu místo tooadd tuto aplikaci a vyberte  **+**  tooadd na kartě.</span><span class="sxs-lookup"><span data-stu-id="de184-119">Navigate toohello Teams channel where you want tooadd this app and select **+** tooadd a tab.</span></span>

   ![Vyberte Přidat na kartě](./media/application-proxy-teams/add-tab.png)

2. <span data-ttu-id="de184-121">Vyberte **webu** z hello kartě Možnosti.</span><span class="sxs-lookup"><span data-stu-id="de184-121">Select **Website** from hello tab options.</span></span>

   ![Přidání webu](./media/application-proxy-teams/website.png)

3. <span data-ttu-id="de184-123">Pojmenujte hello kartě a nastavte hello URL toohello Proxy aplikace externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="de184-123">Give hello tab a name and set hello URL toohello Application Proxy external URL.</span></span> 

   ![Konfigurovat adresu URL a název karty](./media/application-proxy-teams/tab-name-url.png)

<span data-ttu-id="de184-125">Jakmile jeden člen týmu přidá kartu hello, se zobrazí u všech uživatelů v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="de184-125">Once one member of a team adds hello tab, it shows up for everyone in hello channel.</span></span> <span data-ttu-id="de184-126">Uživatelé, kteří mají přístup k aplikaci toohello získat přístup jednoho přihlášení s přihlašovacími údaji hello, které používají pro Teams společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="de184-126">Any users who have access toohello app get single sign-on access with hello credentials they use for Microsoft Teams.</span></span> <span data-ttu-id="de184-127">Uživatelé, kteří nemají přístup k aplikaci toohello se zobrazí karta hello v týmy, ale je blokovaný, dokud jim poskytnout oprávnění toohello místní aplikace a hello Azure portálu publikovanou verzi aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="de184-127">Any users who don't have access toohello app will see hello tab in Teams, but are blocked until you give them permissions toohello on-premises app and hello Azure portal published version of hello app.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="de184-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de184-128">Next steps</span></span>

- <span data-ttu-id="de184-129">Zjistěte, jak příliš[publikování webů služby SharePoint místní](application-proxy-enable-remote-access-sharepoint.md) pomocí Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="de184-129">Learn how too[publish on-premises SharePoint sites](application-proxy-enable-remote-access-sharepoint.md) with Application Proxy.</span></span>
- <span data-ttu-id="de184-130">Konfigurace vaší aplikace toouse [vlastní domény](active-directory-application-proxy-custom-domains.md) pro jejich externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="de184-130">Configure your apps toouse [custom domains](active-directory-application-proxy-custom-domains.md) for their external URL.</span></span> 
