---
title: 'Kurz: Azure Active Directory integrace s UserVoice | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a služby UserVoice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: fcfda1c2ecb162fb93b70574a18bd745b72ee4db
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a><span data-ttu-id="a1efd-103">Kurz: Azure Active Directory integrace s UserVoice</span><span class="sxs-lookup"><span data-stu-id="a1efd-103">Tutorial: Azure Active Directory integration with UserVoice</span></span>

<span data-ttu-id="a1efd-104">V tomto kurzu zjistěte, jak integrovat UserVoice s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a1efd-104">In this tutorial, you learn how to integrate UserVoice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a1efd-105">Integrace služby UserVoice s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a1efd-105">Integrating UserVoice with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a1efd-106">Můžete ovládat ve službě Azure AD, který má přístup k UserVoice.</span><span class="sxs-lookup"><span data-stu-id="a1efd-106">You can control in Azure AD who has access to UserVoice.</span></span>
- <span data-ttu-id="a1efd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k UserVoice (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1efd-107">You can enable your users to automatically get signed-on to UserVoice (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a1efd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a1efd-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="a1efd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a1efd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1efd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a1efd-110">Prerequisites</span></span>

<span data-ttu-id="a1efd-111">Konfigurace integrace Azure AD s UserVoice, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-111">To configure Azure AD integration with UserVoice, you need the following items:</span></span>

- <span data-ttu-id="a1efd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1efd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a1efd-113">Webu UserVoice jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a1efd-113">A UserVoice single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a1efd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a1efd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a1efd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a1efd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a1efd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a1efd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a1efd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a1efd-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a1efd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a1efd-118">Scenario description</span></span>
<span data-ttu-id="a1efd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a1efd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a1efd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a1efd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a1efd-121">Přidání UserVoice z Galerie</span><span class="sxs-lookup"><span data-stu-id="a1efd-121">Adding UserVoice from the gallery</span></span>
2. <span data-ttu-id="a1efd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1efd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-uservoice-from-the-gallery"></a><span data-ttu-id="a1efd-123">Přidání UserVoice z Galerie</span><span class="sxs-lookup"><span data-stu-id="a1efd-123">Adding UserVoice from the gallery</span></span>
<span data-ttu-id="a1efd-124">Při konfiguraci integrace služby UserVoice do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS UserVoice z galerie.</span><span class="sxs-lookup"><span data-stu-id="a1efd-124">To configure the integration of UserVoice into Azure AD, you need to add UserVoice from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a1efd-125">**Pokud chcete přidat UserVoice z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a1efd-125">**To add UserVoice from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a1efd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a1efd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="a1efd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a1efd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="a1efd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1efd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="a1efd-133">Do vyhledávacího pole zadejte **UserVoice**, vyberte **UserVoice** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a1efd-133">In the search box, type **UserVoice**, select **UserVoice** from result panel then click **Add** button to add the application.</span></span>

    ![UserVoice v seznamu výsledků](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a1efd-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1efd-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a1efd-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserVoice podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a1efd-136">In this section, you configure and test Azure AD single sign-on with UserVoice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a1efd-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v UserVoice je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1efd-137">For single sign-on to work, Azure AD needs to know what the counterpart user in UserVoice is to a user in Azure AD.</span></span> <span data-ttu-id="a1efd-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v UserVoice musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a1efd-138">In other words, a link relationship between an Azure AD user and the related user in UserVoice needs to be established.</span></span>

<span data-ttu-id="a1efd-139">V UserVoice, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a1efd-139">In UserVoice, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a1efd-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserVoice, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-140">To configure and test Azure AD single sign-on with UserVoice, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a1efd-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a1efd-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a1efd-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a1efd-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a1efd-143">**[Vytvoření zkušebního uživatele UserVoice](#create-a-uservoice-test-user)**  – Pokud chcete mít protějšek Britta Simon v UserVoice propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a1efd-143">**[Create a UserVoice test user](#create-a-uservoice-test-user)** - to have a counterpart of Britta Simon in UserVoice that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a1efd-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a1efd-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a1efd-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a1efd-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a1efd-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1efd-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a1efd-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci UserVoice.</span><span class="sxs-lookup"><span data-stu-id="a1efd-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UserVoice application.</span></span>

<span data-ttu-id="a1efd-148">**Ke konfiguraci Azure AD jednotné přihlašování s UserVoice, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a1efd-148">**To configure Azure AD single sign-on with UserVoice, perform the following steps:**</span></span>

1. <span data-ttu-id="a1efd-149">Na portálu Azure na **UserVoice** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-149">In the Azure portal, on the **UserVoice** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="a1efd-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a1efd-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. <span data-ttu-id="a1efd-153">Na **UserVoice domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-153">On the **UserVoice Domain and URLs** section, perform the following steps:</span></span>

    ![Doména služby UserVoice a adresy URL jednotné přihlašování informace](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    <span data-ttu-id="a1efd-155">a.</span><span class="sxs-lookup"><span data-stu-id="a1efd-155">a.</span></span> <span data-ttu-id="a1efd-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="a1efd-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    <span data-ttu-id="a1efd-157">b.</span><span class="sxs-lookup"><span data-stu-id="a1efd-157">b.</span></span> <span data-ttu-id="a1efd-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="a1efd-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a1efd-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a1efd-159">These values are not real.</span></span> <span data-ttu-id="a1efd-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a1efd-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a1efd-161">Obraťte se na [tým podpory UserVoice klienta](https://www.uservoice.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a1efd-161">Contact [UserVoice Client support team](https://www.uservoice.com/) to get these values.</span></span>

4. <span data-ttu-id="a1efd-162">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a1efd-162">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. <span data-ttu-id="a1efd-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a1efd-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a1efd-166">Na **UserVoice konfigurace** klikněte na tlačítko **konfigurace UserVoice** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a1efd-166">On the **UserVoice Configuration** section, click **Configure UserVoice** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a1efd-167">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a1efd-167">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace služby UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. <span data-ttu-id="a1efd-169">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti UserVoice jako správce.</span><span class="sxs-lookup"><span data-stu-id="a1efd-169">In a different web browser window, log in to your UserVoice company site as an administrator.</span></span>

8. <span data-ttu-id="a1efd-170">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**a potom vyberte **webový portál** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="a1efd-170">In the toolbar on the top, click **Settings**, and then select **Web portal** from the menu.</span></span>
   
    <span data-ttu-id="a1efd-171">![V oddílu nastavení na straně aplikace](./media/active-directory-saas-uservoice-tutorial/ic777519.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="a1efd-171">![Settings Section On App Side](./media/active-directory-saas-uservoice-tutorial/ic777519.png "Settings")</span></span>

9. <span data-ttu-id="a1efd-172">Na **webový portál** ve **ověření uživatele** klikněte na tlačítko **upravit** otevřete **upravit ověření uživatele** dialogové okno stránky.</span><span class="sxs-lookup"><span data-stu-id="a1efd-172">On the **Web portal** tab, in the **User authentication** section, click **Edit** to open the **Edit User Authentication** dialog page.</span></span>
   
    <span data-ttu-id="a1efd-173">![Webový portál karta](./media/active-directory-saas-uservoice-tutorial/ic777520.png "webový portál")</span><span class="sxs-lookup"><span data-stu-id="a1efd-173">![Web portal Tab](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web portal")</span></span>

10. <span data-ttu-id="a1efd-174">Na **upravit ověření uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-174">On the **Edit User Authentication** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="a1efd-175">![Upravit ověření uživatele](./media/active-directory-saas-uservoice-tutorial/ic777521.png "upravit ověření uživatele")</span><span class="sxs-lookup"><span data-stu-id="a1efd-175">![Edit user authentication](./media/active-directory-saas-uservoice-tutorial/ic777521.png "Edit user authentication")</span></span>
   
    <span data-ttu-id="a1efd-176">a.</span><span class="sxs-lookup"><span data-stu-id="a1efd-176">a.</span></span> <span data-ttu-id="a1efd-177">Klikněte na tlačítko **jednotné přihlašování (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-177">Click **Single Sign-On (SSO)**.</span></span>
 
    <span data-ttu-id="a1efd-178">b.</span><span class="sxs-lookup"><span data-stu-id="a1efd-178">b.</span></span> <span data-ttu-id="a1efd-179">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **jednotné přihlašování vzdáleného přihlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a1efd-179">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **SSO Remote Sign-In** textbox.</span></span>

    <span data-ttu-id="a1efd-180">c.</span><span class="sxs-lookup"><span data-stu-id="a1efd-180">c.</span></span> <span data-ttu-id="a1efd-181">Vložení **Sign-Out URL** hodnotu, kterou jste zkopírovali z portálu Azure do **vzdáleného Sign-Out jednotného přihlašování k textovému poli**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-181">Paste the **Sign-Out URL** value, which you have copied from the Azure portal into the **SSO Remote Sign-Out textbox**.</span></span>
 
    <span data-ttu-id="a1efd-182">d.</span><span class="sxs-lookup"><span data-stu-id="a1efd-182">d.</span></span> <span data-ttu-id="a1efd-183">Vložení **kryptografický otisk** hodnotu, kterou jste zkopírovali z portálu Azure do **aktuální otisků prstů certifikátů SHA1** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a1efd-183">Paste the **Thumbprint** value , which you have copied from Azure portal  into the **Current certificate SHA1 fingerprint** textbox.</span></span>
    
    <span data-ttu-id="a1efd-184">e.</span><span class="sxs-lookup"><span data-stu-id="a1efd-184">e.</span></span> <span data-ttu-id="a1efd-185">Klikněte na tlačítko **uložit nastavení ověřování**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-185">Click **Save authentication settings**.</span></span>

> [!TIP]
> <span data-ttu-id="a1efd-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a1efd-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a1efd-187">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a1efd-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a1efd-188">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a1efd-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a1efd-189">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1efd-189">Create an Azure AD test user</span></span>

<span data-ttu-id="a1efd-190">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a1efd-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="a1efd-192">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a1efd-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a1efd-193">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a1efd-193">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a1efd-195">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-195">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a1efd-197">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1efd-197">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a1efd-199">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-199">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a1efd-201">a.</span><span class="sxs-lookup"><span data-stu-id="a1efd-201">a.</span></span> <span data-ttu-id="a1efd-202">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-202">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a1efd-203">b.</span><span class="sxs-lookup"><span data-stu-id="a1efd-203">b.</span></span> <span data-ttu-id="a1efd-204">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a1efd-204">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="a1efd-205">c.</span><span class="sxs-lookup"><span data-stu-id="a1efd-205">c.</span></span> <span data-ttu-id="a1efd-206">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="a1efd-206">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="a1efd-207">d.</span><span class="sxs-lookup"><span data-stu-id="a1efd-207">d.</span></span> <span data-ttu-id="a1efd-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-208">Click **Create**.</span></span>
 
### <a name="create-a-uservoice-test-user"></a><span data-ttu-id="a1efd-209">Vytvoření zkušebního uživatele UserVoice</span><span class="sxs-lookup"><span data-stu-id="a1efd-209">Create a UserVoice test user</span></span>

<span data-ttu-id="a1efd-210">Povolit uživatelům Azure AD přihlášení na stránku UserVoice, musí být zřízená do UserVoice.</span><span class="sxs-lookup"><span data-stu-id="a1efd-210">To enable Azure AD users to log in to UserVoice, they must be provisioned into UserVoice.</span></span> <span data-ttu-id="a1efd-211">V případě UserVoice zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a1efd-211">In the case of UserVoice, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="a1efd-212">K poskytnutí uživatelského účtu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-212">To provision a user account, perform the following steps:</span></span>
1. <span data-ttu-id="a1efd-213">Přihlaste se k vaší **UserVoice** klienta.</span><span class="sxs-lookup"><span data-stu-id="a1efd-213">Log in to your **UserVoice** tenant.</span></span>

2. <span data-ttu-id="a1efd-214">Přejděte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-214">Go to **Settings**.</span></span>
   
    <span data-ttu-id="a1efd-215">![Nastavení](./media/active-directory-saas-uservoice-tutorial/ic777811.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="a1efd-215">![Settings](./media/active-directory-saas-uservoice-tutorial/ic777811.png "Settings")</span></span>

3. <span data-ttu-id="a1efd-216">Klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-216">Click **General**.</span></span>

4. <span data-ttu-id="a1efd-217">Klikněte na tlačítko **agenty a oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-217">Click **Agents and permissions**.</span></span>
   
    <span data-ttu-id="a1efd-218">![Agenti a oprávnění](./media/active-directory-saas-uservoice-tutorial/ic777812.png "agenty a oprávnění")</span><span class="sxs-lookup"><span data-stu-id="a1efd-218">![Agents and permissions](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents and permissions")</span></span>

5. <span data-ttu-id="a1efd-219">Klikněte na tlačítko **přidat admins**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-219">Click **Add admins**.</span></span>
   
    <span data-ttu-id="a1efd-220">![Přidat admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "přidat admins")</span><span class="sxs-lookup"><span data-stu-id="a1efd-220">![Add admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "Add admins")</span></span>

6. <span data-ttu-id="a1efd-221">Na **pozvat správci** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1efd-221">On the **Invite admins** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="a1efd-222">![Pozvat správci](./media/active-directory-saas-uservoice-tutorial/ic777814.png "pozvat správci")</span><span class="sxs-lookup"><span data-stu-id="a1efd-222">![Invite admins](./media/active-directory-saas-uservoice-tutorial/ic777814.png "Invite admins")</span></span>
   
    <span data-ttu-id="a1efd-223">a.</span><span class="sxs-lookup"><span data-stu-id="a1efd-223">a.</span></span> <span data-ttu-id="a1efd-224">Do textového pole e-mailů, zadejte e-mailovou adresu účtu chcete zřídit a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-224">In the Emails textbox, type the email address of the account you want to provision, and then click **Add**.</span></span>
   
    <span data-ttu-id="a1efd-225">b.</span><span class="sxs-lookup"><span data-stu-id="a1efd-225">b.</span></span> <span data-ttu-id="a1efd-226">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-226">Click **Invite**.</span></span>

> [!NOTE]
> <span data-ttu-id="a1efd-227">Můžete použít všechny ostatní UserVoice uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované UserVoice zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a1efd-227">You can use any other UserVoice user account creation tools or APIs provided by UserVoice to provision AAD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a1efd-228">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1efd-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="a1efd-229">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu UserVoice.</span><span class="sxs-lookup"><span data-stu-id="a1efd-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UserVoice.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="a1efd-231">**Pokud chcete přiřadit Britta Simon UserVoice, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a1efd-231">**To assign Britta Simon to UserVoice, perform the following steps:**</span></span>

1. <span data-ttu-id="a1efd-232">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a1efd-234">V seznamu aplikací vyberte **UserVoice**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-234">In the applications list, select **UserVoice**.</span></span>

    ![V seznamu aplikací na odkaz UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. <span data-ttu-id="a1efd-236">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a1efd-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="a1efd-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a1efd-238">Click **Add** button.</span></span> <span data-ttu-id="a1efd-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1efd-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="a1efd-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a1efd-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a1efd-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1efd-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a1efd-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1efd-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a1efd-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1efd-244">Test single sign-on</span></span>

<span data-ttu-id="a1efd-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a1efd-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a1efd-246">Když kliknete na dlaždici služby UserVoice na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci UserVoice.</span><span class="sxs-lookup"><span data-stu-id="a1efd-246">When you click the UserVoice tile in the Access Panel, you should get automatically signed-on to your UserVoice application.</span></span>
<span data-ttu-id="a1efd-247">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a1efd-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a1efd-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a1efd-248">Additional resources</span></span>

* [<span data-ttu-id="a1efd-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a1efd-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a1efd-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a1efd-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

