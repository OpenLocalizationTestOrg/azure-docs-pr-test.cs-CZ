---
title: "Problém konfigurace federované jednotné přihlašování pro aplikace bez Galerie | Microsoft Docs"
description: "Adresa některé běžné problémy, ke kterým může dojít při konfiguraci federované jednotné přihlašování k vaší vlastní aplikaci SAML, která není uvedena v galerii aplikací Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4eb2c04c940dd6ad15a491a331aed76c237f0387
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="ec6ea-103">Problém konfigurace federované jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="ec6ea-103">Problem configuring federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="ec6ea-104">Pokud dojde k potížím při konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="ec6ea-105">Ověřte jste postupovali podle pokynů v článku [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikací Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span><span class="sxs-lookup"><span data-stu-id="ec6ea-105">Verify you have followed all the steps in the article [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span></span>

## <a name="cant-add-another-instance-of-the-application"></a><span data-ttu-id="ec6ea-106">Nelze přidat jiná instance aplikace</span><span class="sxs-lookup"><span data-stu-id="ec6ea-106">Can’t add another instance of the application</span></span>

<span data-ttu-id="ec6ea-107">Přidat druhou instanci aplikace, musíte být schopni:</span><span class="sxs-lookup"><span data-stu-id="ec6ea-107">To add a second instance of an application, you need to be able to:</span></span>

-   <span data-ttu-id="ec6ea-108">Jedinečný identifikátor pro druhou instanci nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-108">Configure a unique identifier for the second instance.</span></span> <span data-ttu-id="ec6ea-109">Nebudete moci konfigurovat stejný identifikátor použitý pro první instanci.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-109">You won’t be able to configure the same identifier used for the first instance.</span></span>

-   <span data-ttu-id="ec6ea-110">Nakonfigurujte jiný certifikát než ten, který používá pro první instanci.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-110">Configure a different certificate than the one used for the first instance.</span></span>

<span data-ttu-id="ec6ea-111">Pokud aplikace nepodporuje žádné z výše.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-111">If the application doesn’t support any of the above.</span></span> <span data-ttu-id="ec6ea-112">Potom nebudete moci konfigurovat druhou instanci.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-112">Then, you won’t be able to configure a second instance.</span></span>

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a><span data-ttu-id="ec6ea-113">Kde lze nastavit formát EntityID (identifikátor uživatele)</span><span class="sxs-lookup"><span data-stu-id="ec6ea-113">Where do I set the EntityID (User Identifier) format</span></span>

<span data-ttu-id="ec6ea-114">Nebudete moci vybrat formát EntityID (identifikátor uživatele), který odešle aplikaci v odpovědi po ověření uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-114">You won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="ec6ea-115">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-115">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="ec6ea-116">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="ec6ea-116">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy,</span></span>

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a><span data-ttu-id="ec6ea-117">Kde získat metadata aplikace nebo certifikát z Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec6ea-117">Where do I get the application metadata or certificate from Azure AD</span></span>

<span data-ttu-id="ec6ea-118">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="ec6ea-118">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="ec6ea-119">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="ec6ea-119">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="ec6ea-120">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-120">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="ec6ea-121">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-121">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="ec6ea-122">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-122">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="ec6ea-123">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-123">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="ec6ea-124">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="ec6ea-124">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="ec6ea-125">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-125">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="ec6ea-126">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-126">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="ec6ea-127">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-127">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="ec6ea-128">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-128">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="ec6ea-129">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-129">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="ec6ea-130">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-130">The metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a><span data-ttu-id="ec6ea-131">Nemusíte vědět, jak přizpůsobit SAML deklarace identity odeslat do aplikace</span><span class="sxs-lookup"><span data-stu-id="ec6ea-131">Don't know how to customize SAML claims sent to an application</span></span>

<span data-ttu-id="ec6ea-132">Naučte se přizpůsobovat deklarací identity atributu SAML odeslaných do vaší aplikace, najdete v tématu [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ec6ea-132">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec6ea-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec6ea-133">Next steps</span></span>
[<span data-ttu-id="ec6ea-134">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ec6ea-134">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
