---
title: 'Kurz: Azure Active Directory integrace s PurelyHR | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PurelyHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: a9075b1759ebd39f164bfe288fb0a365acdcc44c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a><span data-ttu-id="c8ef9-103">Kurz: Azure Active Directory integrace s PurelyHR</span><span class="sxs-lookup"><span data-stu-id="c8ef9-103">Tutorial: Azure Active Directory integration with PurelyHR</span></span>

<span data-ttu-id="c8ef9-104">V tomto kurzu zjistěte, jak integrovat PurelyHR s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c8ef9-104">In this tutorial, you learn how to integrate PurelyHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8ef9-105">Integrace PurelyHR s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-105">Integrating PurelyHR with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c8ef9-106">Můžete řídit ve službě Azure AD, který má přístup k PurelyHR</span><span class="sxs-lookup"><span data-stu-id="c8ef9-106">You can control in Azure AD who has access to PurelyHR</span></span>
- <span data-ttu-id="c8ef9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k PurelyHR (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8ef9-107">You can enable your users to automatically get signed-on to PurelyHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c8ef9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c8ef9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c8ef9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8ef9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8ef9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c8ef9-110">Prerequisites</span></span>

<span data-ttu-id="c8ef9-111">Konfigurace integrace Azure AD s PurelyHR, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-111">To configure Azure AD integration with PurelyHR, you need the following items:</span></span>

- <span data-ttu-id="c8ef9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8ef9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8ef9-113">PurelyHR jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c8ef9-113">A PurelyHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8ef9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8ef9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8ef9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8ef9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8ef9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8ef9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c8ef9-118">Scenario description</span></span>
<span data-ttu-id="c8ef9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8ef9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8ef9-121">Přidání PurelyHR z Galerie</span><span class="sxs-lookup"><span data-stu-id="c8ef9-121">Adding PurelyHR from the gallery</span></span>
2. <span data-ttu-id="c8ef9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8ef9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-purelyhr-from-the-gallery"></a><span data-ttu-id="c8ef9-123">Přidání PurelyHR z Galerie</span><span class="sxs-lookup"><span data-stu-id="c8ef9-123">Adding PurelyHR from the gallery</span></span>
<span data-ttu-id="c8ef9-124">Při konfiguraci integrace PurelyHR do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS PurelyHR z galerie.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-124">To configure the integration of PurelyHR into Azure AD, you need to add PurelyHR from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c8ef9-125">**Pokud chcete přidat PurelyHR z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c8ef9-125">**To add PurelyHR from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c8ef9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c8ef9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c8ef9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c8ef9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c8ef9-133">Do vyhledávacího pole zadejte **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-133">In the search box, type **PurelyHR**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_search.png)

5. <span data-ttu-id="c8ef9-135">Na panelu výsledků vyberte **PurelyHR**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-135">In the results panel, select **PurelyHR**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c8ef9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8ef9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c8ef9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s PurelyHR podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c8ef9-138">In this section, you configure and test Azure AD single sign-on with PurelyHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c8ef9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PurelyHR je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PurelyHR is to a user in Azure AD.</span></span> <span data-ttu-id="c8ef9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PurelyHR musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-140">In other words, a link relationship between an Azure AD user and the related user in PurelyHR needs to be established.</span></span>

<span data-ttu-id="c8ef9-141">V PurelyHR, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-141">In PurelyHR, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c8ef9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PurelyHR, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-142">To configure and test Azure AD single sign-on with PurelyHR, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c8ef9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c8ef9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8ef9-145">**[Vytvoření zkušebního uživatele PurelyHR](#creating-a-purelyhr-test-user)**  – Pokud chcete mít protějšek Britta Simon v PurelyHR propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-145">**[Creating a PurelyHR test user](#creating-a-purelyhr-test-user)** - to have a counterpart of Britta Simon in PurelyHR that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8ef9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8ef9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c8ef9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8ef9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c8ef9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PurelyHR application.</span></span>

<span data-ttu-id="c8ef9-150">**Ke konfiguraci Azure AD jednotné přihlašování s PurelyHR, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c8ef9-150">**To configure Azure AD single sign-on with PurelyHR, perform the following steps:**</span></span>

1. <span data-ttu-id="c8ef9-151">Na portálu Azure na **PurelyHR** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-151">In the Azure portal, on the **PurelyHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c8ef9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. <span data-ttu-id="c8ef9-155">Na **PurelyHR domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-155">On the **PurelyHR Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    <span data-ttu-id="c8ef9-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyID>.purelyhr.com/sso-consume`</span><span class="sxs-lookup"><span data-stu-id="c8ef9-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyID>.purelyhr.com/sso-consume`</span></span>

4. <span data-ttu-id="c8ef9-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-158">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    <span data-ttu-id="c8ef9-160">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<companyID>.purelyhr.com/sso-initiate`</span><span class="sxs-lookup"><span data-stu-id="c8ef9-160">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<companyID>.purelyhr.com/sso-initiate`</span></span>
     
    > [!NOTE]
    > <span data-ttu-id="c8ef9-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-161">These values are not the real.</span></span> <span data-ttu-id="c8ef9-162">Tyto hodnoty aktualizujte skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-162">Update these values with the actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="c8ef9-163">Obraťte se na [tým podpory PurelyHR klienta](http://support.purelyhr.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-163">Contact [PurelyHR Client support team](http://support.purelyhr.com/) to get these values.</span></span> 

5. <span data-ttu-id="c8ef9-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. <span data-ttu-id="c8ef9-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="c8ef9-168">Na **PurelyHR konfigurace** klikněte na tlačítko **konfigurace PurelyHR** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-168">On the **PurelyHR Configuration** section, click **Configure PurelyHR** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c8ef9-169">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c8ef9-169">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. <span data-ttu-id="c8ef9-171">Konfigurace jednotného přihlašování na **PurelyHR** straně, přihlašovací údaje pro jejich web jako správce.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-171">To configure single sign-on on **PurelyHR** side, login to their website as an administrator.</span></span>

9. <span data-ttu-id="c8ef9-172">Otevřete **řídicí panel** z možností na panelu nástrojů a klikněte na **nastavení jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-172">Open the **Dashboard** from the options in the toolbar and click **SSO Settings**.</span></span>

10. <span data-ttu-id="c8ef9-173">Vložení hodnoty do polí, jak je popsáno níže-</span><span class="sxs-lookup"><span data-stu-id="c8ef9-173">Paste the values in the boxes as described below-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)    

    <span data-ttu-id="c8ef9-175">a.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-175">a.</span></span> <span data-ttu-id="c8ef9-176">Otevřete **Certificate(Bas64)** stáhli z portálu Azure v programu Poznámkový blok a zkopírujte hodnotu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-176">Open the **Certificate(Bas64)** downloaded from the Azure portal in notepad and copy the certificate value.</span></span> <span data-ttu-id="c8ef9-177">Vložit zkopírovaný hodnotu do **certifikát X.509** pole.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-177">Paste the copied value into the **X.509 Certificate** box.</span></span>

    <span data-ttu-id="c8ef9-178">b.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-178">b.</span></span> <span data-ttu-id="c8ef9-179">V **URL vystavitele Idp** pole, vložte **SAML Entity ID** zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-179">In the **Idp Issuer URL** box, paste the **SAML Entity ID** copied from the Azure portal.</span></span>

    <span data-ttu-id="c8ef9-180">c.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-180">c.</span></span> <span data-ttu-id="c8ef9-181">V **adresu URL koncového bodu Idp** pole, vložte **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-181">In the **Idp Endpoint URL** box, paste the **SAML Single Sign-On Service URL** copied from the Azure portal.</span></span> 

    <span data-ttu-id="c8ef9-182">d.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-182">d.</span></span> <span data-ttu-id="c8ef9-183">Zkontrolujte **automaticky vytvářet uživatelé** zaškrtávací políčko pro povolení automatického uživatele zřizování v PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-183">Check the **Auto-Create Users** checkbox to enable automatic user provisioning in PurelyHR.</span></span>

    <span data-ttu-id="c8ef9-184">e.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-184">e.</span></span> <span data-ttu-id="c8ef9-185">Klikněte na tlačítko **uložit změny** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-185">Click **Save Changes** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="c8ef9-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c8ef9-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c8ef9-187">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c8ef9-188">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8ef9-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c8ef9-189">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8ef9-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="c8ef9-190">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c8ef9-192">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c8ef9-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c8ef9-193">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c8ef9-195">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c8ef9-197">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c8ef9-199">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c8ef9-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c8ef9-201">a.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-201">a.</span></span> <span data-ttu-id="c8ef9-202">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8ef9-203">b.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-203">b.</span></span> <span data-ttu-id="c8ef9-204">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c8ef9-205">c.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-205">c.</span></span> <span data-ttu-id="c8ef9-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c8ef9-207">d.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-207">d.</span></span> <span data-ttu-id="c8ef9-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-208">Click **Create**.</span></span>
 
### <a name="creating-a-purelyhr-test-user"></a><span data-ttu-id="c8ef9-209">Vytvoření zkušebního uživatele PurelyHR</span><span class="sxs-lookup"><span data-stu-id="c8ef9-209">Creating a PurelyHR test user</span></span>

<span data-ttu-id="c8ef9-210">Pokud chcete povolit uživatelům Azure AD přihlášení k PurelyHR, musí být zřízená do PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-210">To enable Azure AD users to log in to PurelyHR, they must be provisioned into PurelyHR.</span></span> <span data-ttu-id="c8ef9-211">V PurelyHR zřizování je automatická úloha a nejsou potřeba žádné ruční kroky, pokud je povoleno automatické uživatele zřizování.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-211">In PurelyHR, provisioning is an automatic task and no manual steps are required when automatic user provisioning is enabled.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c8ef9-212">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8ef9-212">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c8ef9-213">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-213">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PurelyHR.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c8ef9-215">**Pokud chcete přiřadit Britta Simon PurelyHR, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c8ef9-215">**To assign Britta Simon to PurelyHR, perform the following steps:**</span></span>

1. <span data-ttu-id="c8ef9-216">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-216">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c8ef9-218">V seznamu aplikací vyberte **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-218">In the applications list, select **PurelyHR**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. <span data-ttu-id="c8ef9-220">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-220">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c8ef9-222">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-222">Click **Add** button.</span></span> <span data-ttu-id="c8ef9-223">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-223">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c8ef9-225">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-225">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c8ef9-226">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-226">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8ef9-227">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-227">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c8ef9-228">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c8ef9-228">Testing single sign-on</span></span>

<span data-ttu-id="c8ef9-229">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-229">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c8ef9-230">Klikněte na dlaždici vyrovná se se zatížením pro správu vzdělávacího procesu na přístupovém panelu, můžete získat automaticky přihlášení k aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-230">Click the Absorb LMS tile in the Access Panel, you get automatically signed-on to your Absorb LMS application.</span></span>

<span data-ttu-id="c8ef9-231">Další informace o na přístupovém panelu najdete v tématu.</span><span class="sxs-lookup"><span data-stu-id="c8ef9-231">For more information about the Access Panel, see.</span></span> <span data-ttu-id="c8ef9-232">[Úvod do přístupového panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="c8ef9-232">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8ef9-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c8ef9-233">Additional resources</span></span>

* [<span data-ttu-id="c8ef9-234">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c8ef9-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8ef9-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c8ef9-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_203.png

