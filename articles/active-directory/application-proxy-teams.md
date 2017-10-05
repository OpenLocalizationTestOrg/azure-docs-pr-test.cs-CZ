---
title: "Přístup k aplikacím Proxy aplikace Azure AD v týmy | Microsoft Docs"
description: "Přístup k místní aplikace přes Microsoft Teams pomocí proxy aplikace služby Azure AD."
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
ms.openlocfilehash: 24e22d7314de536714a825cd7035d2cec2112278
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a><span data-ttu-id="67012-103">Přístup k místním aplikacím přes Teams společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="67012-103">Access your on-premises applications through Microsoft Teams</span></span>

<span data-ttu-id="67012-104">Azure Proxy aplikace Active Directory umožňuje jednotné přihlašování k místním aplikacím bez ohledu na to, kde se a Microsoft Teams zjednodušuje vaše spolupráce úsilí na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="67012-104">Azure Active Directory Application Proxy gives you single sign-on to on-premises applications no matter where you are, and Microsoft Teams streamlines your collaborative efforts in one place.</span></span> <span data-ttu-id="67012-105">Integraci dvou společně znamená, že vaši uživatelé mohli být produktivní s jejich členy týmu v jakékoliv jiné situaci.</span><span class="sxs-lookup"><span data-stu-id="67012-105">Integrating the two together means that your users can be productive with their teammates in any situation.</span></span> 

<span data-ttu-id="67012-106">Uživatele můžete přidat cloudových aplikací k jejich týmy kanály [pomocí karty](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), ale co se stane, když je tento web služby SharePoint nebo všechny používají nástroj plánování hostovaný místně?</span><span class="sxs-lookup"><span data-stu-id="67012-106">Your users can add cloud apps to their Teams channels [using tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), but what happens if that SharePoint site or planning tool they all use is hosted on-premises?</span></span> <span data-ttu-id="67012-107">Proxy aplikací je řešení.</span><span class="sxs-lookup"><span data-stu-id="67012-107">Application Proxy is the solution.</span></span> <span data-ttu-id="67012-108">Můžou přidat aplikacím publikovaným pomocí Proxy aplikací k jejich kanály pomocí stejné externí adresy URL vždy používají vzdálený přístup ke své aplikace.</span><span class="sxs-lookup"><span data-stu-id="67012-108">They can add apps published through Application Proxy to their channels using the same external URLs they always use to access their apps remotely.</span></span> <span data-ttu-id="67012-109">A protože Proxy aplikace ověřuje prostřednictvím Azure Active Directory, představuje stejné jeden přihlašování prostředí.</span><span class="sxs-lookup"><span data-stu-id="67012-109">And because Application Proxy authenticates through Azure Active Directory, the same single sign-on experience carries through.</span></span>


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a><span data-ttu-id="67012-110">Instalace konektoru Proxy aplikace a publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="67012-110">Install the Application Proxy connector and publish your app</span></span>

<span data-ttu-id="67012-111">Pokud jste to ještě neudělali, [konfiguraci Proxy aplikace vašeho klienta a instalaci konektoru](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="67012-111">If you haven't already, [configure Application Proxy for your tenant and install the connector](active-directory-application-proxy-enable.md).</span></span> <span data-ttu-id="67012-112">Potom [publikovat místní aplikace](application-proxy-publish-azure-portal.md) pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="67012-112">Then, [publish your on-premises application](application-proxy-publish-azure-portal.md) for remote access.</span></span> <span data-ttu-id="67012-113">Když publikujete aplikaci, poznamenejte externí adresu URL, protože koncoví uživatelé musí tyto informace při přidání aplikace do týmů.</span><span class="sxs-lookup"><span data-stu-id="67012-113">When you're publishing the app, make note of the external URL because your end users need that information when they add the app to Teams.</span></span>

<span data-ttu-id="67012-114">Pokud již máte aplikace publikována, ale nepamatujete jejich externí adresy URL, vyhledejte je [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="67012-114">If you already have your apps published but don't remember their external URLs, look them up in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="67012-115">Přihlaste se a potom přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** > vyberte svou aplikaci > **proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67012-115">Sign in, then navigate to **Azure Active Directory** > **Enterprise applications** > **All applications** > select your app > **Application proxy**.</span></span>

## <a name="add-your-app-to-teams"></a><span data-ttu-id="67012-116">Přidání aplikace do týmů</span><span class="sxs-lookup"><span data-stu-id="67012-116">Add your app to Teams</span></span>

<span data-ttu-id="67012-117">Po publikování aplikace prostřednictvím Proxy aplikace dát uživatelům vědět, že se můžete přidat jako karty přímo v jejich týmy kanály.</span><span class="sxs-lookup"><span data-stu-id="67012-117">Once you publish the app through Application Proxy, let your users know that they can add it as a tab directly in their Teams channels.</span></span> <span data-ttu-id="67012-118">Kliknul proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="67012-118">Have them follow these three steps:</span></span>

1. <span data-ttu-id="67012-119">Přejděte do týmů kanál, ve které chcete přidat tuto aplikaci a vyberte  **+**  přidat na kartě.</span><span class="sxs-lookup"><span data-stu-id="67012-119">Navigate to the Teams channel where you want to add this app and select **+** to add a tab.</span></span>

   ![Vyberte Přidat na kartě](./media/application-proxy-teams/add-tab.png)

2. <span data-ttu-id="67012-121">Vyberte **webu** z kartě Možnosti.</span><span class="sxs-lookup"><span data-stu-id="67012-121">Select **Website** from the tab options.</span></span>

   ![Přidání webu](./media/application-proxy-teams/website.png)

3. <span data-ttu-id="67012-123">Pojmenujte kartě a nastavte adresu URL na externí adresu URL Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="67012-123">Give the tab a name and set the URL to the Application Proxy external URL.</span></span> 

   ![Konfigurovat adresu URL a název karty](./media/application-proxy-teams/tab-name-url.png)

<span data-ttu-id="67012-125">Jakmile jeden člen týmu přidá kartu, se zobrazí u všech uživatelů v kanálu.</span><span class="sxs-lookup"><span data-stu-id="67012-125">Once one member of a team adds the tab, it shows up for everyone in the channel.</span></span> <span data-ttu-id="67012-126">Uživatelé, kteří mají přístup k aplikaci získat přístup jednoho přihlášení pomocí přihlašovacích údajů, které používají pro Teams společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="67012-126">Any users who have access to the app get single sign-on access with the credentials they use for Microsoft Teams.</span></span> <span data-ttu-id="67012-127">Uživatelé, kteří nemají přístup k aplikaci se zobrazí na kartě v týmy, ale jsou zablokované bez poskytnutí oprávnění pro místní aplikace a portálu Azure publikovanou verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="67012-127">Any users who don't have access to the app will see the tab in Teams, but are blocked until you give them permissions to the on-premises app and the Azure portal published version of the app.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="67012-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67012-128">Next steps</span></span>

- <span data-ttu-id="67012-129">Zjistěte, jak [publikování webů služby SharePoint místní](application-proxy-enable-remote-access-sharepoint.md) pomocí Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="67012-129">Learn how to [publish on-premises SharePoint sites](application-proxy-enable-remote-access-sharepoint.md) with Application Proxy.</span></span>
- <span data-ttu-id="67012-130">Nakonfigurovat aplikace pro použití [vlastní domény](active-directory-application-proxy-custom-domains.md) pro jejich externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="67012-130">Configure your apps to use [custom domains](active-directory-application-proxy-custom-domains.md) for their external URL.</span></span> 
