---
title: 'Kurz: Azure Active Directory integrace s Freshservice | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Freshservice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d32775fa91d3a49da1ef55e57d1d38990fa09346
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a><span data-ttu-id="4e890-103">Kurz: Azure Active Directory integrace s Freshservice</span><span class="sxs-lookup"><span data-stu-id="4e890-103">Tutorial: Azure Active Directory integration with Freshservice</span></span>

<span data-ttu-id="4e890-104">V tomto kurzu zjistěte, jak integrovat Freshservice s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4e890-104">In this tutorial, you learn how to integrate Freshservice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4e890-105">Integrace Freshservice s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4e890-105">Integrating Freshservice with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4e890-106">Můžete řídit ve službě Azure AD, který má přístup k Freshservice</span><span class="sxs-lookup"><span data-stu-id="4e890-106">You can control in Azure AD who has access to Freshservice</span></span>
- <span data-ttu-id="4e890-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Freshservice (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e890-107">You can enable your users to automatically get signed-on to Freshservice (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4e890-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4e890-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4e890-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4e890-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e890-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e890-110">Prerequisites</span></span>

<span data-ttu-id="4e890-111">Konfigurace integrace Azure AD s Freshservice, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="4e890-111">To configure Azure AD integration with Freshservice, you need the following items:</span></span>

- <span data-ttu-id="4e890-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e890-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4e890-113">Freshservice jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4e890-113">A Freshservice single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4e890-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e890-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4e890-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4e890-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4e890-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4e890-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4e890-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e890-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4e890-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4e890-118">Scenario description</span></span>
<span data-ttu-id="4e890-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e890-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4e890-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4e890-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4e890-121">Přidání Freshservice z Galerie</span><span class="sxs-lookup"><span data-stu-id="4e890-121">Adding Freshservice from the gallery</span></span>
2. <span data-ttu-id="4e890-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e890-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshservice-from-the-gallery"></a><span data-ttu-id="4e890-123">Přidání Freshservice z Galerie</span><span class="sxs-lookup"><span data-stu-id="4e890-123">Adding Freshservice from the gallery</span></span>
<span data-ttu-id="4e890-124">Při konfiguraci integrace Freshservice do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Freshservice z galerie.</span><span class="sxs-lookup"><span data-stu-id="4e890-124">To configure the integration of Freshservice into Azure AD, you need to add Freshservice from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4e890-125">**Pokud chcete přidat Freshservice z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4e890-125">**To add Freshservice from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4e890-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4e890-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4e890-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4e890-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4e890-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4e890-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4e890-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e890-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4e890-133">Do vyhledávacího pole zadejte **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="4e890-133">In the search box, type **Freshservice**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. <span data-ttu-id="4e890-135">Na panelu výsledků vyberte **Freshservice**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e890-135">In the results panel, select **Freshservice**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4e890-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e890-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4e890-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Freshservice podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="4e890-138">In this section, you configure and test Azure AD single sign-on with Freshservice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4e890-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Freshservice je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e890-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Freshservice is to a user in Azure AD.</span></span> <span data-ttu-id="4e890-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Freshservice musí navázat.</span><span class="sxs-lookup"><span data-stu-id="4e890-140">In other words, a link relationship between an Azure AD user and the related user in Freshservice needs to be established.</span></span>

<span data-ttu-id="4e890-141">V Freshservice, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="4e890-141">In Freshservice, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4e890-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Freshservice, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="4e890-142">To configure and test Azure AD single sign-on with Freshservice, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4e890-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="4e890-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4e890-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e890-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4e890-145">**[Vytvoření zkušebního uživatele Freshservice](#creating-a-freshservice-test-user)**  – Pokud chcete mít protějšek Britta Simon v Freshservice propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="4e890-145">**[Creating a Freshservice test user](#creating-a-freshservice-test-user)** - to have a counterpart of Britta Simon in Freshservice that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4e890-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4e890-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4e890-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4e890-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4e890-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e890-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4e890-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Freshservice.</span><span class="sxs-lookup"><span data-stu-id="4e890-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Freshservice application.</span></span>

<span data-ttu-id="4e890-150">**Ke konfiguraci Azure AD jednotné přihlašování s Freshservice, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4e890-150">**To configure Azure AD single sign-on with Freshservice, perform the following steps:**</span></span>

1. <span data-ttu-id="4e890-151">Na portálu Azure na **Freshservice** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4e890-151">In the Azure portal, on the **Freshservice** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4e890-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4e890-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. <span data-ttu-id="4e890-155">Na **Freshservice domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e890-155">On the **Freshservice Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    <span data-ttu-id="4e890-157">a.</span><span class="sxs-lookup"><span data-stu-id="4e890-157">a.</span></span> <span data-ttu-id="4e890-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="4e890-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<democompany>.freshservice.com`</span></span>

    <span data-ttu-id="4e890-159">b.</span><span class="sxs-lookup"><span data-stu-id="4e890-159">b.</span></span> <span data-ttu-id="4e890-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="4e890-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<democompany>.freshservice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4e890-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="4e890-161">These values are not real.</span></span> <span data-ttu-id="4e890-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="4e890-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4e890-163">Obraťte se na [tým podpory Freshservice klienta](https://support.freshservice.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="4e890-163">Contact [Freshservice Client support team](https://support.freshservice.com/) to get these values.</span></span> 
 
4. <span data-ttu-id="4e890-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="4e890-164">On the **SAML Signing Certificate** section, copy **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. <span data-ttu-id="4e890-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e890-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4e890-168">Na **Freshservice konfigurace** klikněte na tlačítko **konfigurace Freshservice** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="4e890-168">On the **Freshservice Configuration** section, click **Configure Freshservice** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4e890-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="4e890-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. <span data-ttu-id="4e890-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Freshservice jako správce.</span><span class="sxs-lookup"><span data-stu-id="4e890-171">In a different web browser window, log in to your Freshservice company site as an administrator.</span></span>

8. <span data-ttu-id="4e890-172">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="4e890-172">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="4e890-173">![Správce](./media/active-directory-saas-freshservice-tutorial/ic790814.png "správce")</span><span class="sxs-lookup"><span data-stu-id="4e890-173">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

9. <span data-ttu-id="4e890-174">V **zákaznický portál**, klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4e890-174">In the **Customer Portal**, click **Security**.</span></span>
   
    <span data-ttu-id="4e890-175">![Zabezpečení](./media/active-directory-saas-freshservice-tutorial/ic790815.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="4e890-175">![Security](./media/active-directory-saas-freshservice-tutorial/ic790815.png "Security")</span></span>

10. <span data-ttu-id="4e890-176">V **zabezpečení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e890-176">In the **Security** section, perform the following steps:</span></span>
   
    <span data-ttu-id="4e890-177">![Jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/ic790816.png "jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="4e890-177">![Single Sign On](./media/active-directory-saas-freshservice-tutorial/ic790816.png "Single Sign On")</span></span>
   
    <span data-ttu-id="4e890-178">a.</span><span class="sxs-lookup"><span data-stu-id="4e890-178">a.</span></span> <span data-ttu-id="4e890-179">Přepínač **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4e890-179">Switch **Single Sign On**.</span></span>

    <span data-ttu-id="4e890-180">b.</span><span class="sxs-lookup"><span data-stu-id="4e890-180">b.</span></span> <span data-ttu-id="4e890-181">Vyberte **jednotné přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="4e890-181">Select **SAML SSO**.</span></span>

    <span data-ttu-id="4e890-182">c.</span><span class="sxs-lookup"><span data-stu-id="4e890-182">c.</span></span> <span data-ttu-id="4e890-183">V **SAML přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e890-183">In the **SAML Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="4e890-184">d.</span><span class="sxs-lookup"><span data-stu-id="4e890-184">d.</span></span> <span data-ttu-id="4e890-185">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e890-185">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="4e890-186">e.</span><span class="sxs-lookup"><span data-stu-id="4e890-186">e.</span></span> <span data-ttu-id="4e890-187">V **otisků certifikátu zabezpečení** textovému poli, Vložit **kryptografický OTISK** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e890-187">In **Security Certificate Fingerprint** textbox, paste the **THUMBPRINT** value of certificate which you have copied from Azure portal.</span></span>

    <span data-ttu-id="4e890-188">f.</span><span class="sxs-lookup"><span data-stu-id="4e890-188">f.</span></span> <span data-ttu-id="4e890-189">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="4e890-189">Click **Save**</span></span>
   
> [!TIP]
> <span data-ttu-id="4e890-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="4e890-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4e890-191">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="4e890-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4e890-192">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4e890-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4e890-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e890-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="4e890-194">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e890-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4e890-196">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4e890-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4e890-197">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4e890-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4e890-199">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4e890-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4e890-201">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e890-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4e890-203">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e890-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4e890-205">a.</span><span class="sxs-lookup"><span data-stu-id="4e890-205">a.</span></span> <span data-ttu-id="4e890-206">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4e890-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4e890-207">b.</span><span class="sxs-lookup"><span data-stu-id="4e890-207">b.</span></span> <span data-ttu-id="4e890-208">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4e890-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4e890-209">c.</span><span class="sxs-lookup"><span data-stu-id="4e890-209">c.</span></span> <span data-ttu-id="4e890-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4e890-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4e890-211">d.</span><span class="sxs-lookup"><span data-stu-id="4e890-211">d.</span></span> <span data-ttu-id="4e890-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e890-212">Click **Create**.</span></span>
 
### <a name="creating-a-freshservice-test-user"></a><span data-ttu-id="4e890-213">Vytvoření zkušebního uživatele Freshservice</span><span class="sxs-lookup"><span data-stu-id="4e890-213">Creating a Freshservice test user</span></span>

<span data-ttu-id="4e890-214">Pokud chcete povolit uživatelům Azure AD přihlášení k FreshService, musí být zřízená do FreshService.</span><span class="sxs-lookup"><span data-stu-id="4e890-214">To enable Azure AD users to log in to FreshService, they must be provisioned into FreshService.</span></span> <span data-ttu-id="4e890-215">V případě FreshService zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="4e890-215">In the case of FreshService, provisioning is a manual task.</span></span>

<span data-ttu-id="4e890-216">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4e890-216">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="4e890-217">Přihlaste se k vaší **FreshService** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="4e890-217">Log in to your **FreshService** company site as an administrator.</span></span>

2. <span data-ttu-id="4e890-218">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="4e890-218">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="4e890-219">![Správce](./media/active-directory-saas-freshservice-tutorial/ic790814.png "správce")</span><span class="sxs-lookup"><span data-stu-id="4e890-219">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

3. <span data-ttu-id="4e890-220">V **Správa uživatelů** klikněte na tlačítko **žadatelů o**.</span><span class="sxs-lookup"><span data-stu-id="4e890-220">In the **User Management** section, click **Requesters**.</span></span>
   
    <span data-ttu-id="4e890-221">![Žadatelů o](./media/active-directory-saas-freshservice-tutorial/ic790818.png "žadatelů o")</span><span class="sxs-lookup"><span data-stu-id="4e890-221">![Requesters](./media/active-directory-saas-freshservice-tutorial/ic790818.png "Requesters")</span></span>

4. <span data-ttu-id="4e890-222">Klikněte na tlačítko **nový žadatel**.</span><span class="sxs-lookup"><span data-stu-id="4e890-222">Click **New Requester**.</span></span>
   
    <span data-ttu-id="4e890-223">![Nové žadatelů o](./media/active-directory-saas-freshservice-tutorial/ic790819.png "nové žadatelů o")</span><span class="sxs-lookup"><span data-stu-id="4e890-223">![New Requesters](./media/active-directory-saas-freshservice-tutorial/ic790819.png "New Requesters")</span></span>

5. <span data-ttu-id="4e890-224">V **nový žadatel** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e890-224">In the **New Requester** section, perform the following steps:</span></span>
   
    <span data-ttu-id="4e890-225">![Nový žadatel](./media/active-directory-saas-freshservice-tutorial/ic790820.png "nový žadatel")</span><span class="sxs-lookup"><span data-stu-id="4e890-225">![New Requester](./media/active-directory-saas-freshservice-tutorial/ic790820.png "New Requester")</span></span>   

    <span data-ttu-id="4e890-226">a.</span><span class="sxs-lookup"><span data-stu-id="4e890-226">a.</span></span> <span data-ttu-id="4e890-227">Zadejte **křestní jméno** a **e-mailu** atributy platný účet služby Azure Active Directory chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="4e890-227">Enter the **First Name** and **Email** attributes of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="4e890-228">b.</span><span class="sxs-lookup"><span data-stu-id="4e890-228">b.</span></span> <span data-ttu-id="4e890-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4e890-229">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="4e890-230">Držitel účtu Azure Active Directory získá zahrnutím odkazu pro potvrzení účtu před stane aktivní e-mailu</span><span class="sxs-lookup"><span data-stu-id="4e890-230">The Azure Active Directory account holder gets an email including a link to confirm the account before it becomes active</span></span>
    >  

>[!NOTE]
><span data-ttu-id="4e890-231">Můžete použít všechny ostatní FreshService uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované FreshService zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="4e890-231">You can use any other FreshService user account creation tools or APIs provided by FreshService to provision AAD user accounts.</span></span>
>  

![Přiřadit uživatele][200] 

<span data-ttu-id="4e890-233">**Pokud chcete přiřadit Britta Simon Freshservice, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4e890-233">**To assign Britta Simon to Freshservice, perform the following steps:**</span></span>

1. <span data-ttu-id="4e890-234">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4e890-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4e890-236">V seznamu aplikací vyberte **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="4e890-236">In the applications list, select **Freshservice**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. <span data-ttu-id="4e890-238">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4e890-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4e890-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e890-240">Click **Add** button.</span></span> <span data-ttu-id="4e890-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e890-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4e890-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4e890-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4e890-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e890-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4e890-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e890-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4e890-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e890-246">Testing single sign-on</span></span>

<span data-ttu-id="4e890-247">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4e890-247">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4e890-248">Když kliknete na dlaždici Freshservice na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Freshservice.</span><span class="sxs-lookup"><span data-stu-id="4e890-248">When you click the Freshservice tile in the Access Panel, you should get automatically signed-on to your Freshservice application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e890-249">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4e890-249">Additional resources</span></span>

* [<span data-ttu-id="4e890-250">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e890-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4e890-251">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4e890-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

