---
title: "Kurz: Azure Active Directory integrace s hostované grafitová | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a hostované grafitová."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f6ed02cc67be4090402a115c30819ff6cff99c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a><span data-ttu-id="b8b76-103">Kurz: Azure Active Directory integrace s hostované grafitová</span><span class="sxs-lookup"><span data-stu-id="b8b76-103">Tutorial: Azure Active Directory integration with Hosted Graphite</span></span>

<span data-ttu-id="b8b76-104">V tomto kurzu zjistěte, jak integrovat grafitová hostované v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b8b76-104">In this tutorial, you learn how to integrate Hosted Graphite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b8b76-105">Integrace hostované grafitová s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b8b76-105">Integrating Hosted Graphite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b8b76-106">Můžete řídit ve službě Azure AD, který má přístup k hostované grafitová</span><span class="sxs-lookup"><span data-stu-id="b8b76-106">You can control in Azure AD who has access to Hosted Graphite</span></span>
- <span data-ttu-id="b8b76-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k hostované grafitová (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8b76-107">You can enable your users to automatically get signed-on to Hosted Graphite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b8b76-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b8b76-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b8b76-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b8b76-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8b76-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8b76-110">Prerequisites</span></span>

<span data-ttu-id="b8b76-111">Konfigurace integrace Azure AD s hostované grafitová, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b8b76-111">To configure Azure AD integration with Hosted Graphite, you need the following items:</span></span>

- <span data-ttu-id="b8b76-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8b76-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b8b76-113">Hostované grafitová jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b8b76-113">A Hosted Graphite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b8b76-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8b76-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b8b76-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b8b76-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b8b76-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b8b76-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b8b76-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8b76-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b8b76-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b8b76-118">Scenario description</span></span>
<span data-ttu-id="b8b76-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8b76-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b8b76-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b8b76-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b8b76-121">Přidání hostované grafitová z Galerie</span><span class="sxs-lookup"><span data-stu-id="b8b76-121">Adding Hosted Graphite from the gallery</span></span>
2. <span data-ttu-id="b8b76-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8b76-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hosted-graphite-from-the-gallery"></a><span data-ttu-id="b8b76-123">Přidání hostované grafitová z Galerie</span><span class="sxs-lookup"><span data-stu-id="b8b76-123">Adding Hosted Graphite from the gallery</span></span>
<span data-ttu-id="b8b76-124">Při konfiguraci integrace hostované grafitu do služby Azure AD, potřebujete přidat hostované grafitová z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b8b76-124">To configure the integration of Hosted Graphite into Azure AD, you need to add Hosted Graphite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b8b76-125">**Chcete-li přidat hostované grafitová z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="b8b76-125">**To add Hosted Graphite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b8b76-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b8b76-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b8b76-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b8b76-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b8b76-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8b76-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b8b76-133">Do vyhledávacího pole zadejte **hostované grafitová**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-133">In the search box, type **Hosted Graphite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

5. <span data-ttu-id="b8b76-135">Na panelu výsledků vyberte **hostované grafitová**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b8b76-135">In the results panel, select **Hosted Graphite**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b8b76-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8b76-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b8b76-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s hostované grafitová podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b8b76-138">In this section, you configure and test Azure AD single sign-on with Hosted Graphite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b8b76-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v hostované grafitová je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8b76-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Hosted Graphite is to a user in Azure AD.</span></span> <span data-ttu-id="b8b76-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v hostované grafitová musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b8b76-140">In other words, a link relationship between an Azure AD user and the related user in Hosted Graphite needs to be established.</span></span>

<span data-ttu-id="b8b76-141">V hostované grafitová přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b8b76-141">In Hosted Graphite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b8b76-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s hostované grafitová, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b8b76-142">To configure and test Azure AD single sign-on with Hosted Graphite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b8b76-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b8b76-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b8b76-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8b76-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b8b76-145">**[Vytvoření zkušebního uživatele hostované grafitová](#creating-a-hosted-graphite-test-user)**  – Pokud chcete mít protějšek Britta Simon v hostované grafitová propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b8b76-145">**[Creating a Hosted Graphite test user](#creating-a-hosted-graphite-test-user)** - to have a counterpart of Britta Simon in Hosted Graphite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b8b76-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8b76-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b8b76-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b8b76-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b8b76-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8b76-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b8b76-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci hostované grafitová.</span><span class="sxs-lookup"><span data-stu-id="b8b76-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Hosted Graphite application.</span></span>

<span data-ttu-id="b8b76-150">**Ke konfiguraci Azure AD jednotné přihlašování s hostované grafitová, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8b76-150">**To configure Azure AD single sign-on with Hosted Graphite, perform the following steps:**</span></span>

1. <span data-ttu-id="b8b76-151">Na portálu Azure na **hostované grafitová** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-151">In the Azure portal, on the **Hosted Graphite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b8b76-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8b76-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

3. <span data-ttu-id="b8b76-155">Na **hostované grafitová domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b8b76-155">On the **Hosted Graphite Domain and URLs** section, if you wish to configure the application in **IDP initiated mode**, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    <span data-ttu-id="b8b76-157">a.</span><span class="sxs-lookup"><span data-stu-id="b8b76-157">a.</span></span> <span data-ttu-id="b8b76-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.hostedgraphite.com/metadata/<user id>`</span><span class="sxs-lookup"><span data-stu-id="b8b76-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.hostedgraphite.com/metadata/<user id>`</span></span>

    <span data-ttu-id="b8b76-159">b.</span><span class="sxs-lookup"><span data-stu-id="b8b76-159">b.</span></span> <span data-ttu-id="b8b76-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.hostedgraphite.com/complete/saml/<user id>`</span><span class="sxs-lookup"><span data-stu-id="b8b76-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.hostedgraphite.com/complete/saml/<user id>`</span></span>

4. <span data-ttu-id="b8b76-161">Na **hostované grafitová domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **SP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b8b76-161">On the **Hosted Graphite Domain and URLs** section, if you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    <span data-ttu-id="b8b76-163">a.</span><span class="sxs-lookup"><span data-stu-id="b8b76-163">a.</span></span> <span data-ttu-id="b8b76-164">Klikněte na **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="b8b76-164">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="b8b76-165">b.</span><span class="sxs-lookup"><span data-stu-id="b8b76-165">b.</span></span> <span data-ttu-id="b8b76-166">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.hostedgraphite.com/login/saml/<user id>/`</span><span class="sxs-lookup"><span data-stu-id="b8b76-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://www.hostedgraphite.com/login/saml/<user id>/`</span></span>   

    > [!NOTE] 
    > <span data-ttu-id="b8b76-167">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b8b76-167">Please note that these are not the real values.</span></span> <span data-ttu-id="b8b76-168">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="b8b76-168">You have to update these values with the actual Identifier, Reply URL and Sign On URL.</span></span> <span data-ttu-id="b8b76-169">Chcete-li získat tyto hodnoty, můžete přejít na přístup -> Nastavení SAML na straně aplikace nebo kontaktujte [tým podpory hostované grafitová](mailto:help@hostedgraphite.com).</span><span class="sxs-lookup"><span data-stu-id="b8b76-169">To get these values, you can go to Access->SAML setup on your Application side or Contact [Hosted Graphite support team](mailto:help@hostedgraphite.com).</span></span>
    >
 
5. <span data-ttu-id="b8b76-170">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="b8b76-170">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

6. <span data-ttu-id="b8b76-172">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8b76-172">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="b8b76-174">Na **hostované konfigurace grafitová** klikněte na tlačítko **konfigurace hostované grafitová** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b8b76-174">On the **Hosted Graphite Configuration** section, click **Configure Hosted Graphite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b8b76-175">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b8b76-175">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

8. <span data-ttu-id="b8b76-177">Přihlášení ke klientovi hostované grafitová jako správce.</span><span class="sxs-lookup"><span data-stu-id="b8b76-177">Sign-on to your Hosted Graphite tenant as an administrator.</span></span>

9. <span data-ttu-id="b8b76-178">Přejděte na **stránce instalace SAML** na bočním panelu (**přístup -> Nastavení SAML**).</span><span class="sxs-lookup"><span data-stu-id="b8b76-178">Go to the **SAML Setup page** in the sidebar (**Access -> SAML Setup**).</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

10. <span data-ttu-id="b8b76-180">Potvrďte tyto adresy URL neodpovídá konfiguraci provést na **hostované grafitová domény a adresy URL** části portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8b76-180">Confirm these URls match your configuration done on the **Hosted Graphite Domain and URLs** section of the Azure portal.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

11. <span data-ttu-id="b8b76-182">V **Entity nebo ID vystavitele** a **adresu URL pro přihlášení SSO** textových polí, vložte hodnotu **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8b76-182">In  **Entity or Issuer ID** and **SSO Login URL** textboxes, paste the value of **SAML Entity ID** and **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

12. <span data-ttu-id="b8b76-184">Vyberte "**jen pro čtení**" jako **výchozí Role uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-184">Select "**Read-only**" as **Default User Role**.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

13. <span data-ttu-id="b8b76-186">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b8b76-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

14. <span data-ttu-id="b8b76-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8b76-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="b8b76-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b8b76-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b8b76-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b8b76-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b8b76-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b8b76-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b8b76-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8b76-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="b8b76-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8b76-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b8b76-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8b76-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b8b76-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b8b76-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b8b76-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b8b76-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8b76-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b8b76-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b8b76-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b8b76-204">a.</span><span class="sxs-lookup"><span data-stu-id="b8b76-204">a.</span></span> <span data-ttu-id="b8b76-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b8b76-206">b.</span><span class="sxs-lookup"><span data-stu-id="b8b76-206">b.</span></span> <span data-ttu-id="b8b76-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b8b76-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b8b76-208">c.</span><span class="sxs-lookup"><span data-stu-id="b8b76-208">c.</span></span> <span data-ttu-id="b8b76-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b8b76-210">d.</span><span class="sxs-lookup"><span data-stu-id="b8b76-210">d.</span></span> <span data-ttu-id="b8b76-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-211">Click **Create**.</span></span>
 
### <a name="creating-a-hosted-graphite-test-user"></a><span data-ttu-id="b8b76-212">Vytvoření zkušebního uživatele hostované grafitová</span><span class="sxs-lookup"><span data-stu-id="b8b76-212">Creating a Hosted Graphite test user</span></span>

<span data-ttu-id="b8b76-213">Cílem této části je vytvoření uživatele volal Britta Simon v hostované grafitová.</span><span class="sxs-lookup"><span data-stu-id="b8b76-213">The objective of this section is to create a user called Britta Simon in Hosted Graphite.</span></span> <span data-ttu-id="b8b76-214">Hostované grafitová podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="b8b76-214">Hosted Graphite supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b8b76-215">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="b8b76-215">There is no action item for you in this section.</span></span> <span data-ttu-id="b8b76-216">Vytvoří se nový uživatel během pokusu o přístup k hostované grafitová, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b8b76-216">A new user will be created during an attempt to access Hosted Graphite if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="b8b76-217">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat tým podpory hostované grafitová prostřednictvím < mailto:help@hostedgraphite.com >.</span><span class="sxs-lookup"><span data-stu-id="b8b76-217">If you need to create a user manually, you need to contact the Hosted Graphite support team via <mailto:help@hostedgraphite.com>.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b8b76-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8b76-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b8b76-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k hostované grafitová.</span><span class="sxs-lookup"><span data-stu-id="b8b76-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Hosted Graphite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b8b76-221">**Pokud chcete přiřadit Britta Simon hostované grafitová, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8b76-221">**To assign Britta Simon to Hosted Graphite, perform the following steps:**</span></span>

1. <span data-ttu-id="b8b76-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b8b76-224">V seznamu aplikací vyberte **hostované grafitová**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-224">In the applications list, select **Hosted Graphite**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

3. <span data-ttu-id="b8b76-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b8b76-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b8b76-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8b76-228">Click **Add** button.</span></span> <span data-ttu-id="b8b76-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8b76-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b8b76-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b8b76-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b8b76-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8b76-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b8b76-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8b76-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b8b76-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8b76-234">Testing single sign-on</span></span>

<span data-ttu-id="b8b76-235">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="b8b76-235">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="b8b76-236">Když kliknete na dlaždici grafitová hostované na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci hostované grafitová.</span><span class="sxs-lookup"><span data-stu-id="b8b76-236">When you click the Hosted Graphite tile in the Access Panel, you should get automatically signed-on to your Hosted Graphite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8b76-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b8b76-237">Additional resources</span></span>

* [<span data-ttu-id="b8b76-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b8b76-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b8b76-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b8b76-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png

