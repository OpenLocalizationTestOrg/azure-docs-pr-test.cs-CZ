---
title: "Konfigurace federované jednotné přihlašování pro aplikace bez Galerie aaaProblem | Microsoft Docs"
description: "Adresa některé hello běžné problémy, se můžete setkat při konfiguraci federované jeden přihlašování tooyour vlastní SAML aplikaci, která není uvedena v hello Azure AD Application Gallery"
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
ms.openlocfilehash: 8c80f0001de01cbc9930ef0441cd804082ee8578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="4cba0-103">Problém konfigurace federované jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="4cba0-103">Problem configuring federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="4cba0-104">Pokud dojde k potížím při konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="4cba0-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="4cba0-105">Ověřte všechny kroky hello v článku hello [konfigurace jedné přihlašování tooapplications které nejsou v galerii aplikací Azure Active Directory hello.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span><span class="sxs-lookup"><span data-stu-id="4cba0-105">Verify you have followed all hello steps in hello article [Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span></span>

## <a name="cant-add-another-instance-of-hello-application"></a><span data-ttu-id="4cba0-106">Nelze přidat jiná instance aplikace hello</span><span class="sxs-lookup"><span data-stu-id="4cba0-106">Can’t add another instance of hello application</span></span>

<span data-ttu-id="4cba0-107">tooadd druhou instanci aplikace, je třeba toobe moci:</span><span class="sxs-lookup"><span data-stu-id="4cba0-107">tooadd a second instance of an application, you need toobe able to:</span></span>

-   <span data-ttu-id="4cba0-108">Jedinečný identifikátor pro druhou instanci hello nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="4cba0-108">Configure a unique identifier for hello second instance.</span></span> <span data-ttu-id="4cba0-109">Nebudete moct tooconfigure hello, používá stejný identifikátor pro první instanci hello.</span><span class="sxs-lookup"><span data-stu-id="4cba0-109">You won’t be able tooconfigure hello same identifier used for hello first instance.</span></span>

-   <span data-ttu-id="4cba0-110">Nakonfigurujte jiný certifikát než hello, jedna pro hello první instance.</span><span class="sxs-lookup"><span data-stu-id="4cba0-110">Configure a different certificate than hello one used for hello first instance.</span></span>

<span data-ttu-id="4cba0-111">Pokud hello aplikace nepodporuje žádné z výše uvedených hello.</span><span class="sxs-lookup"><span data-stu-id="4cba0-111">If hello application doesn’t support any of hello above.</span></span> <span data-ttu-id="4cba0-112">Potom nebudete moct tooconfigure druhou instanci.</span><span class="sxs-lookup"><span data-stu-id="4cba0-112">Then, you won’t be able tooconfigure a second instance.</span></span>

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a><span data-ttu-id="4cba0-113">Kde lze nastavit formát hello EntityID (identifikátor uživatele)</span><span class="sxs-lookup"><span data-stu-id="4cba0-113">Where do I set hello EntityID (User Identifier) format</span></span>

<span data-ttu-id="4cba0-114">Nebudete moct tooselect hello EntityID (identifikátor uživatele) formát, Azure AD odešle toohello aplikace v odpovědi hello po ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="4cba0-114">You won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="4cba0-115">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="4cba0-115">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="4cba0-116">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části hello NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="4cba0-116">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy,</span></span>

## <a name="where-do-i-get-hello-application-metadata-or-certificate-from-azure-ad"></a><span data-ttu-id="4cba0-117">Kde získat metadata aplikace hello nebo certifikát z Azure AD</span><span class="sxs-lookup"><span data-stu-id="4cba0-117">Where do I get hello application metadata or certificate from Azure AD</span></span>

<span data-ttu-id="4cba0-118">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="4cba0-118">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="4cba0-119">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="4cba0-119">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4cba0-120">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="4cba0-120">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4cba0-121">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4cba0-121">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4cba0-122">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="4cba0-122">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4cba0-123">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="4cba0-123">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="4cba0-124">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4cba0-124">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="4cba0-125">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4cba0-125">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="4cba0-126">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="4cba0-126">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4cba0-127">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="4cba0-127">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="4cba0-128">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="4cba0-128">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="4cba0-129">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="4cba0-129">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="4cba0-130">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="4cba0-130">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a><span data-ttu-id="4cba0-131">Nevíte, jak deklarace SAML toocustomize odeslané tooan aplikace</span><span class="sxs-lookup"><span data-stu-id="4cba0-131">Don't know how toocustomize SAML claims sent tooan application</span></span>

<span data-ttu-id="4cba0-132">toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4cba0-132">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cba0-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cba0-133">Next steps</span></span>
[<span data-ttu-id="4cba0-134">Správa aplikací pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4cba0-134">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
