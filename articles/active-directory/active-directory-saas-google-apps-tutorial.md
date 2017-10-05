---
title: 'Kurz: Azure Active Directory integrace s Google Apps v Azure | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 065841d6b4fe50e953f01bba4d3f23de82b82726
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a><span data-ttu-id="4dfe3-103">Kurz: Azure Active Directory integrace s Google Apps</span><span class="sxs-lookup"><span data-stu-id="4dfe3-103">Tutorial: Azure Active Directory integration with Google Apps</span></span>

<span data-ttu-id="4dfe3-104">V tomto kurzu zjistěte, jak integrovat Google Apps v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4dfe3-104">In this tutorial, you learn how to integrate Google Apps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4dfe3-105">Integrace s Azure AD Google Apps poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-105">Integrating Google Apps with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4dfe3-106">Můžete řídit ve službě Azure AD, který má přístup ke Google Apps</span><span class="sxs-lookup"><span data-stu-id="4dfe3-106">You can control in Azure AD who has access to Google Apps</span></span>
- <span data-ttu-id="4dfe3-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Google Apps (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4dfe3-107">You can enable your users to automatically get signed-on to Google Apps (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4dfe3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4dfe3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4dfe3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4dfe3-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4dfe3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4dfe3-110">Prerequisites</span></span>

<span data-ttu-id="4dfe3-111">Konfigurace integrace Azure AD s Google Apps, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-111">To configure Azure AD integration with Google Apps, you need the following items:</span></span>

- <span data-ttu-id="4dfe3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4dfe3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4dfe3-113">Google Apps jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4dfe3-113">A Google Apps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4dfe3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4dfe3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4dfe3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4dfe3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4dfe3-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="4dfe3-118">Videokurz</span><span class="sxs-lookup"><span data-stu-id="4dfe3-118">Video tutorial</span></span>
<span data-ttu-id="4dfe3-119">Postup při povolení jednotného přihlašování ke Google Apps během 2 minut.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-119">How to Enable Single Sign-On to Google Apps in 2 Minutes:</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a><span data-ttu-id="4dfe3-120">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="4dfe3-120">Frequently Asked Questions</span></span>
1. <span data-ttu-id="4dfe3-121">**Otázka: je Chromebooks a dalších zařízení Chrome kompatibilní s Azure AD jednotné přihlašování?**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-121">**Q: Are Chromebooks and other Chrome devices compatible with Azure AD single sign-on?**</span></span>
   
    <span data-ttu-id="4dfe3-122">Odpověď: Ano, se uživatelé moct přihlásit k jejich Chromebook zařízení pomocí svých přihlašovacích údajů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-122">A: Yes, users are able to sign into their Chromebook devices using their Azure AD credentials.</span></span> <span data-ttu-id="4dfe3-123">Toto [Google Apps podporují článku](https://support.google.com/chrome/a/answer/6060880) informace o důvod, proč může získat uživatelé vyzváni k zadání pověření dvakrát.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-123">See this [Google Apps support article](https://support.google.com/chrome/a/answer/6060880) for information on why users may get prompted for credentials twice.</span></span>

2. <span data-ttu-id="4dfe3-124">**Otázka: Pokud lze povolit jednotné přihlašování, budou uživatelé moci žádné Google produkt, například Google učebny, GMail, Google Drive, YouTube a tak dále se přihlaste pomocí přihlašovacích údajů Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-124">**Q: If I enable single sign-on, will users be able to use their Azure AD credentials to sign into any Google product, such as Google Classroom, GMail, Google Drive, YouTube, and so on?**</span></span>
   
    <span data-ttu-id="4dfe3-125">Odpověď: Ano, v závislosti na [které Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) rozhodnete povolit nebo zakázat pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-125">A: Yes, depending on [which Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) you choose to enable or disable for your organization.</span></span>

3. <span data-ttu-id="4dfe3-126">**Otázka: je možné povolit jednotné přihlašování pro pouze podmnožinu Moji uživatelé Google Apps?**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-126">**Q: Can I enable single sign-on for only a subset of my Google Apps users?**</span></span>
   
    <span data-ttu-id="4dfe3-127">Odpověď: Ne, zapnout jednotné přihlašování okamžitě vyžaduje všichni uživatelé Google Apps k ověření pomocí přihlašovacích údajů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-127">A: No, turning on single sign-on immediately requires all your Google Apps users to authenticate with their Azure AD credentials.</span></span> <span data-ttu-id="4dfe3-128">Protože Google Apps nepodporuje, s více poskytovatelů identit, zprostředkovatele identity pro vaše prostředí Google Apps může být buď Azure AD nebo Google – ale ne oba ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-128">Because Google Apps doesn't support having multiple identity providers, the identity provider for your Google Apps environment can either be Azure AD or Google -- but not both at the same time.</span></span>

4. <span data-ttu-id="4dfe3-129">**Otázka: Pokud je uživatel přihlášený prostřednictvím systému Windows, jsou že automaticky ověřování pro Google Apps bez získávání výzva k zadání hesla?**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-129">**Q: If a user is signed in through Windows, are they automatically authenticate to Google Apps without getting prompted for a password?**</span></span>
   
    <span data-ttu-id="4dfe3-130">Odpověď: existují dvě možnosti pro povolení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-130">A: There are two options for enabling this scenario.</span></span> <span data-ttu-id="4dfe3-131">Nejdřív by uživatelé přihlašovat do zařízení s Windows 10 prostřednictvím [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4dfe3-131">First, users could sign into Windows 10 devices via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span> <span data-ttu-id="4dfe3-132">Alternativně může uživatelé přihlašovat do zařízení s Windows, které jsou připojené k doméně pro místní Active Directory byla povolena pro jednotné přihlašování do služby Azure AD pomocí [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) nasazení.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-132">Alternatively, users could sign into Windows devices that are domain-joined to an on-premises Active Directory that has been enabled for single sign-on to Azure AD via an [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) deployment.</span></span> <span data-ttu-id="4dfe3-133">Obě možnosti vyžadují, abyste proveďte kroky v následujícím kurzu umožňující jednotného přihlašování mezi službou Azure AD a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-133">Both options require you to perform the steps in the following tutorial to enable single sign-on between Azure AD and Google Apps.</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4dfe3-134">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4dfe3-134">Scenario description</span></span>
<span data-ttu-id="4dfe3-135">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-135">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4dfe3-136">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-136">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4dfe3-137">Přidání Google Apps z Galerie</span><span class="sxs-lookup"><span data-stu-id="4dfe3-137">Adding Google Apps from the gallery</span></span>
2. <span data-ttu-id="4dfe3-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4dfe3-138">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-google-apps-from-the-gallery"></a><span data-ttu-id="4dfe3-139">Přidání Google Apps z Galerie</span><span class="sxs-lookup"><span data-stu-id="4dfe3-139">Adding Google Apps from the gallery</span></span>
<span data-ttu-id="4dfe3-140">Při konfiguraci integrace Google Apps do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS aplikace Google z galerie.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-140">To configure the integration of Google Apps into Azure AD, you need to add Google Apps from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4dfe3-141">**Pokud chcete přidat Google Apps z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-141">**To add Google Apps from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4dfe3-142">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-142">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4dfe3-144">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-144">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4dfe3-145">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-145">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4dfe3-147">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-147">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4dfe3-149">Do vyhledávacího pole zadejte **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-149">In the search box, type **Google Apps**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. <span data-ttu-id="4dfe3-151">Na panelu výsledků vyberte **Google Apps**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-151">In the results panel, select **Google Apps**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4dfe3-153">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4dfe3-153">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4dfe3-154">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="4dfe3-154">In this section, you configure and test Azure AD single sign-on with Google Apps based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4dfe3-155">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Google Apps je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-155">For single sign-on to work, Azure AD needs to know what the counterpart user in Google Apps is to a user in Azure AD.</span></span> <span data-ttu-id="4dfe3-156">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-156">In other words, a link relationship between an Azure AD user and the related user in Google Apps needs to be established.</span></span>

<span data-ttu-id="4dfe3-157">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-157">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Google Apps.</span></span>

<span data-ttu-id="4dfe3-158">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-158">To configure and test Azure AD single sign-on with Google Apps, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4dfe3-159">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-159">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4dfe3-160">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-160">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4dfe3-161">**[Vytvoření zkušebního uživatele Google Apps](#creating-a-google-apps-test-user)**  – Pokud chcete mít protějšek Britta Simon v Google Apps, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-161">**[Creating a Google Apps test user](#creating-a-google-apps-test-user)** - to have a counterpart of Britta Simon in Google Apps that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4dfe3-162">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-162">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4dfe3-163">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-163">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4dfe3-164">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4dfe3-164">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4dfe3-165">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-165">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Google Apps application.</span></span>

<span data-ttu-id="4dfe3-166">**Ke konfiguraci Azure AD jednotné přihlašování s Google Apps, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-166">**To configure Azure AD single sign-on with Google Apps, perform the following steps:**</span></span>

1. <span data-ttu-id="4dfe3-167">Na portálu Azure na **Google Apps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-167">In the Azure portal, on the **Google Apps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4dfe3-169">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-169">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. <span data-ttu-id="4dfe3-171">Na **Google Apps domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-171">On the **Google Apps Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    <span data-ttu-id="4dfe3-173">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://mail.google.com/a/<yourdomain>`</span><span class="sxs-lookup"><span data-stu-id="4dfe3-173">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://mail.google.com/a/<yourdomain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4dfe3-174">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-174">This value is not real.</span></span> <span data-ttu-id="4dfe3-175">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-175">Update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="4dfe3-176">Obraťte se [tým podpory Google](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="4dfe3-176">contact the [Google support team](https://www.google.com/contact/).</span></span>
 
4. <span data-ttu-id="4dfe3-177">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát v počítači.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-177">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. <span data-ttu-id="4dfe3-179">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-179">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4dfe3-181">Na **Google Apps konfigurace** klikněte na tlačítko **konfigurace Google Apps** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-181">On the **Google Apps Configuration** section, click **Configure Google Apps** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4dfe3-182">Kopírování **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby a změnit heslo URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-182">Copy the **Sign-Out URL, SAML Single Sign-On Service URL and Change password URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. <span data-ttu-id="4dfe3-184">V prohlížeči otevřete novou kartu a přihlaste se k [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-184">Open a new tab in your browser, and sign into the [Google Apps Admin Console](http://admin.google.com/) using your administrator account.</span></span>

8. <span data-ttu-id="4dfe3-185">Klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-185">Click **Security**.</span></span> <span data-ttu-id="4dfe3-186">Pokud nevidíte odkaz, mohou být skryty pod **více ovládacích prvků** nabídky v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-186">If you don't see the link, it may be hidden under the **More Controls** menu at the bottom of the screen.</span></span>
   
    ![Klikněte na Zabezpečení.][10]

9. <span data-ttu-id="4dfe3-188">Na **zabezpečení** klikněte na tlačítko **nastavit jednotné přihlašování (SSO).**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-188">On the **Security** page, click **Set up single sign-on (SSO).**</span></span>
   
    ![Klikněte na tlačítko jednotné přihlašování.][11]

10. <span data-ttu-id="4dfe3-190">Proveďte následující změny konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-190">Perform the following configuration changes:</span></span>
   
    ![Konfigurace jednotného přihlašování][12]
   
    <span data-ttu-id="4dfe3-192">a.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-192">a.</span></span> <span data-ttu-id="4dfe3-193">Vyberte **nastavení jednotného přihlašování pomocí zprostředkovatele identity jiných výrobců**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-193">Select **Setup SSO with third-party identity provider**.</span></span>

    <span data-ttu-id="4dfe3-194">b.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-194">b.</span></span> <span data-ttu-id="4dfe3-195">V **přihlašovací adresa URL stránky** pole v Google Apps, vložte hodnotu **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-195">In the **Sign-in page URL** field in Google Apps, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="4dfe3-196">c.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-196">c.</span></span> <span data-ttu-id="4dfe3-197">V **adresy URL odhlašovací stránky** pole v Google Apps, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-197">In the **Sign-out page URL** field in Google Apps, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="4dfe3-198">d.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-198">d.</span></span> <span data-ttu-id="4dfe3-199">V **změnit heslo URL** pole v Google Apps, vložte hodnotu **změnit heslo URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-199">In the **Change password URL** field in Google Apps, paste the value of **Change password URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="4dfe3-200">e.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-200">e.</span></span> <span data-ttu-id="4dfe3-201">V Google Apps pro **ověřovací certifikát**, nahrajte certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-201">In Google Apps, for the **Verification certificate**, upload the certificate that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="4dfe3-202">f.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-202">f.</span></span> <span data-ttu-id="4dfe3-203">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-203">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="4dfe3-204">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="4dfe3-204">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4dfe3-205">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-205">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4dfe3-206">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4dfe3-206">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4dfe3-207">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4dfe3-207">Creating an Azure AD test user</span></span>
<span data-ttu-id="4dfe3-208">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-208">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4dfe3-210">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-210">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4dfe3-211">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-211">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4dfe3-213">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-213">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4dfe3-215">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-215">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4dfe3-217">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4dfe3-217">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4dfe3-219">a.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-219">a.</span></span> <span data-ttu-id="4dfe3-220">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-220">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4dfe3-221">b.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-221">b.</span></span> <span data-ttu-id="4dfe3-222">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-222">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4dfe3-223">c.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-223">c.</span></span> <span data-ttu-id="4dfe3-224">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-224">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4dfe3-225">d.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-225">d.</span></span> <span data-ttu-id="4dfe3-226">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-226">Click **Create**.</span></span>
 
### <a name="creating-a-google-apps-test-user"></a><span data-ttu-id="4dfe3-227">Vytvoření zkušebního uživatele Google Apps</span><span class="sxs-lookup"><span data-stu-id="4dfe3-227">Creating a Google Apps test user</span></span>

<span data-ttu-id="4dfe3-228">Cílem této části je vytvoření uživatele volal Britta Simon v Google Apps softwaru.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-228">The objective of this section is to create a user called Britta Simon in Google Apps Software.</span></span> <span data-ttu-id="4dfe3-229">Google Apps podporuje automatické zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-229">Google Apps supports auto provisioning, which is by default enabled.</span></span> <span data-ttu-id="4dfe3-230">Neexistuje žádná akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-230">There is no action for you in this section.</span></span> <span data-ttu-id="4dfe3-231">Pokud uživatel v Google Apps softwaru ještě neexistuje, vytvoří se nový při pokusu o přístup k softwaru Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-231">If a user doesn't already exist in Google Apps Software, a new one is created when you attempt to access Google Apps Software.</span></span>

>[!NOTE] 
><span data-ttu-id="4dfe3-232">Pokud je potřeba ručně vytvořit uživateli, obraťte se [tým podpory Google](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="4dfe3-232">If you need to create a user manually, contact the [Google support team](https://www.google.com/contact/).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4dfe3-233">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4dfe3-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4dfe3-234">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ke Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Google Apps.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="4dfe3-236">**Pokud chcete přiřadit Britta Simon Google Apps, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="4dfe3-236">**To assign Britta Simon to Google Apps, perform the following steps:**</span></span>

1. <span data-ttu-id="4dfe3-237">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4dfe3-239">V seznamu aplikací vyberte **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-239">In the applications list, select **Google Apps**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. <span data-ttu-id="4dfe3-241">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4dfe3-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-243">Click **Add** button.</span></span> <span data-ttu-id="4dfe3-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4dfe3-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4dfe3-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4dfe3-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4dfe3-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4dfe3-249">Testing single sign-on</span></span>

<span data-ttu-id="4dfe3-250">V této části, pokud chcete otestovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu v [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), pak se přihlaste do testovací účet a klikněte na tlačítko **Google Apps** dlaždici na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="4dfe3-250">In this section, to test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), then sign into the test account, and click **Google Apps** tile in the Access Panel.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4dfe3-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4dfe3-251">Additional resources</span></span>

* [<span data-ttu-id="4dfe3-252">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4dfe3-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4dfe3-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4dfe3-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="4dfe3-254">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="4dfe3-254">Configure User Provisioning</span></span>](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png