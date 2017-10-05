---
title: 'Kurz: Azure Active Directory integrace s Atlassian cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Atlassian cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 2891838b56dd15cb5f97dcae391770143a80c781
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a><span data-ttu-id="7cdc7-103">Kurz: Azure Active Directory integrace s Atlassian cloudu</span><span class="sxs-lookup"><span data-stu-id="7cdc7-103">Tutorial: Azure Active Directory integration with Atlassian Cloud</span></span>

<span data-ttu-id="7cdc7-104">V tomto kurzu zjistěte, jak integrovat Atlassian cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7cdc7-104">In this tutorial, you learn how to integrate Atlassian Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7cdc7-105">Integrace Atlassian cloudu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-105">Integrating Atlassian Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7cdc7-106">Můžete řídit ve službě Azure AD, který má přístup do cloudu Atlassian</span><span class="sxs-lookup"><span data-stu-id="7cdc7-106">You can control in Azure AD who has access to Atlassian Cloud</span></span>
- <span data-ttu-id="7cdc7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Atlassian cloudu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7cdc7-107">You can enable your users to automatically get signed-on to Atlassian Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7cdc7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7cdc7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7cdc7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7cdc7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7cdc7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7cdc7-110">Prerequisites</span></span>

<span data-ttu-id="7cdc7-111">Ke konfiguraci integrace služby Azure AD s cloudem Atlassian, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-111">To configure Azure AD integration with Atlassian Cloud, you need the following items:</span></span>

- <span data-ttu-id="7cdc7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7cdc7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7cdc7-113">Cloudu Atlassian jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7cdc7-113">An Atlassian Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7cdc7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7cdc7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7cdc7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7cdc7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7cdc7-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7cdc7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7cdc7-118">Scenario description</span></span>
<span data-ttu-id="7cdc7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7cdc7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7cdc7-121">Přidání Atlassian cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="7cdc7-121">Adding Atlassian Cloud from the gallery</span></span>
2. <span data-ttu-id="7cdc7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7cdc7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atlassian-cloud-from-the-gallery"></a><span data-ttu-id="7cdc7-123">Přidání Atlassian cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="7cdc7-123">Adding Atlassian Cloud from the gallery</span></span>
<span data-ttu-id="7cdc7-124">Při konfiguraci integrace Atlassian cloudu do služby Azure AD, potřebujete přidat Atlassian cloudu z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-124">To configure the integration of Atlassian Cloud into Azure AD, you need to add Atlassian Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7cdc7-125">**Pokud chcete přidat Atlassian cloudu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7cdc7-125">**To add Atlassian Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7cdc7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7cdc7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7cdc7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7cdc7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7cdc7-133">Do vyhledávacího pole zadejte **Atlassian cloudu**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-133">In the search box, type **Atlassian Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. <span data-ttu-id="7cdc7-135">Na panelu výsledků vyberte **Atlassian cloudu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-135">In the results panel, select **Atlassian Cloud**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7cdc7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7cdc7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7cdc7-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Atlassian cloudu podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="7cdc7-138">In this section, you configure and test Azure AD single sign-on with Atlassian Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7cdc7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v cloudu Atlassian je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Atlassian Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="7cdc7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v cloudu Atlassian musí navázat.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-140">In other words, a link relationship between an Azure AD user and the related user in Atlassian Cloud needs to be established.</span></span>

<span data-ttu-id="7cdc7-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Atlassian Cloud.</span></span>

<span data-ttu-id="7cdc7-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atlassian cloudu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-142">To configure and test Azure AD single sign-on with Atlassian Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7cdc7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7cdc7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7cdc7-145">**[Vytváření testovacího uživatele cloudu Atlassian](#creating-an-atlassian-cloud-test-user)**  – Pokud chcete mít protějšek Britta Simon Atlassian cloudu, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-145">**[Creating an Atlassian Cloud test user](#creating-an-atlassian-cloud-test-user)** - to have a counterpart of Britta Simon in Atlassian Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7cdc7-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7cdc7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7cdc7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7cdc7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7cdc7-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Atlassian Cloud application.</span></span>

<span data-ttu-id="7cdc7-150">**Ke konfiguraci Azure AD jednotné přihlašování s Atlassian cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7cdc7-150">**To configure Azure AD single sign-on with Atlassian Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7cdc7-151">Na portálu Azure na **Atlassian cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-151">In the Azure portal, on the **Atlassian Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7cdc7-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. <span data-ttu-id="7cdc7-155">Na **Atlassian cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-155">On the **Atlassian Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    <span data-ttu-id="7cdc7-157">a.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-157">a.</span></span> <span data-ttu-id="7cdc7-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.atlassian.net/admin/saml/edit`</span><span class="sxs-lookup"><span data-stu-id="7cdc7-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.atlassian.net/admin/saml/edit`</span></span>

    <span data-ttu-id="7cdc7-159">b.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-159">b.</span></span> <span data-ttu-id="7cdc7-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL jako:`https://id.atlassian.com/login/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="7cdc7-160">In the **Reply URL** textbox, type a URL as: `https://id.atlassian.com/login/saml/acs`</span></span>

4. <span data-ttu-id="7cdc7-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-161">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    <span data-ttu-id="7cdc7-163">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.atlassian.net`</span><span class="sxs-lookup"><span data-stu-id="7cdc7-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.atlassian.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7cdc7-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-164">These values are not real.</span></span> <span data-ttu-id="7cdc7-165">Tyto hodnoty aktualizujte se skutečným identifikátorem a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-165">Update these values with the actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="7cdc7-166">Přesné hodnoty můžete získat z obrazovky konfigurace SAML Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-166">You can get the exact values from Atlassian Cloud SAML Configuration screen.</span></span>
 
5. <span data-ttu-id="7cdc7-167">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-167">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. <span data-ttu-id="7cdc7-169">Na **konfigurace cloudu Atlassian** klikněte na tlačítko **konfigurace cloudu Atlassian** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-169">On the **Atlassian Cloud Configuration** section, click **Configure Atlassian Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7cdc7-170">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7cdc7-170">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. <span data-ttu-id="7cdc7-172">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přihlášení k portálu Atlassian pomocí oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-172">To get SSO configured for your application, login to the Atlassian Portal using the administrator rights.</span></span>

8. <span data-ttu-id="7cdc7-173">V části ověřování navigaci vlevo klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-173">In the Authentication section of the left navigation click **Domains**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    <span data-ttu-id="7cdc7-175">a.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-175">a.</span></span> <span data-ttu-id="7cdc7-176">Do textového pole zadejte název domény a pak klikněte na tlačítko **přidáním domény**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-176">In the textbox, type your domain name, and then click **Add domain**.</span></span>
        
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    <span data-ttu-id="7cdc7-178">b.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-178">b.</span></span> <span data-ttu-id="7cdc7-179">Chcete-li ověřit doménu, klikněte na tlačítko **ověřte**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-179">To verify the domain, click **Verify**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    <span data-ttu-id="7cdc7-181">c.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-181">c.</span></span> <span data-ttu-id="7cdc7-182">Stáhněte si soubor html ověření domény, nahrajte ho do kořenové složky vaší domény webu a pak klikněte na tlačítko **ověřit doménu**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-182">Download the domain verification html file, upload it to the root folder of your domain's website, and then click **Verify domain**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    <span data-ttu-id="7cdc7-184">d.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-184">d.</span></span> <span data-ttu-id="7cdc7-185">Po ověření domény hodnotu **stav** pole je **ověřeno**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-185">Once the domain is verified, the value of the **Status** field is **Verified**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. <span data-ttu-id="7cdc7-187">V levém navigačním panelu klikněte na **SAML**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-187">In the left navigation bar, click **SAML**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. <span data-ttu-id="7cdc7-189">Vytvoření konfigurace SAML a přidejte konfigurace zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-189">Create a SAML Configuration and add the Identity provider configuration.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    <span data-ttu-id="7cdc7-191">a.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-191">a.</span></span> <span data-ttu-id="7cdc7-192">V **zprostředkovatele Identity Entity ID** textové pole, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-192">In the **Identity provider Entity ID** text box, paste the value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="7cdc7-193">b.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-193">b.</span></span> <span data-ttu-id="7cdc7-194">V **zprostředkovatele Identity URL jednotného přihlašování k** textové pole, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-194">In the **Identity provider SSO URL** text box, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="7cdc7-195">c.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-195">c.</span></span> <span data-ttu-id="7cdc7-196">Otevřete certifikát stažený z portálu Azure a kopírovat hodnoty bez Begin a End řádky a vložte jej do **X509 veřejný certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-196">Open the downloaded certificate from Azure portal and copy the values without the Begin and End lines and paste it in the **Public X509 certificate** box.</span></span>
    
    <span data-ttu-id="7cdc7-197">d.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-197">d.</span></span> <span data-ttu-id="7cdc7-198">Klikněte na tlačítko **uložit konfiguraci** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-198">Click **Save Configuration**  to Save the settings.</span></span>
     
11. <span data-ttu-id="7cdc7-199">Aktualizujte nastavení služby Azure AD a ujistěte se, že máte nastavený správný identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-199">Update the Azure AD settings to make sure that you have setup the correct Identifier URL.</span></span>
  
    <span data-ttu-id="7cdc7-200">a.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-200">a.</span></span> <span data-ttu-id="7cdc7-201">Kopírování **SP Identity ID** z SAML obrazovky a vložte ji ve službě Azure AD jako **identifikátor** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-201">Copy the **SP Identity ID** from the SAML screen and paste it in Azure AD as the **Identifier** value.</span></span>

    <span data-ttu-id="7cdc7-202">b.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-202">b.</span></span> <span data-ttu-id="7cdc7-203">Přihlašovací adresa URL je adresa URL klienta ve vašem cloudu Atlassian.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-203">Sign On URL is the tenant URL of your Atlassian Cloud.</span></span>   

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. <span data-ttu-id="7cdc7-205">Na portálu Azure klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-205">In the Azure portal, Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="7cdc7-207">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="7cdc7-207">You can now read a concise version of these instructions inside the [Azure  portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7cdc7-208">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-208">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7cdc7-209">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7cdc7-209">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7cdc7-210">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7cdc7-210">Creating an Azure AD test user</span></span>
<span data-ttu-id="7cdc7-211">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-211">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7cdc7-213">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7cdc7-213">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7cdc7-214">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-214">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7cdc7-216">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-216">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7cdc7-218">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-218">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7cdc7-220">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7cdc7-220">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7cdc7-222">a.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-222">a.</span></span> <span data-ttu-id="7cdc7-223">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-223">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7cdc7-224">b.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-224">b.</span></span> <span data-ttu-id="7cdc7-225">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-225">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7cdc7-226">c.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-226">c.</span></span> <span data-ttu-id="7cdc7-227">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-227">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7cdc7-228">d.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-228">d.</span></span> <span data-ttu-id="7cdc7-229">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-229">Click **Create**.</span></span>
 
### <a name="creating-an-atlassian-cloud-test-user"></a><span data-ttu-id="7cdc7-230">Vytváření testovacího uživatele Atlassian cloudu</span><span class="sxs-lookup"><span data-stu-id="7cdc7-230">Creating an Atlassian Cloud test user</span></span>

<span data-ttu-id="7cdc7-231">Pokud chcete povolit uživatelům Azure AD přihlášení do cloudu Atlassian, musí být zřízená do Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-231">To enable Azure AD users to log in to Atlassian Cloud, they must be provisioned into Atlassian Cloud.</span></span>  
<span data-ttu-id="7cdc7-232">V případě cloudu Atlassian zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-232">In case of Atlassian Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="7cdc7-233">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7cdc7-233">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="7cdc7-234">V části Správa lokality, klikněte **uživatelé** tlačítko</span><span class="sxs-lookup"><span data-stu-id="7cdc7-234">In the Site administration section, click the **Users** button</span></span>

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. <span data-ttu-id="7cdc7-236">Klikněte **vytvořit uživatele** tlačítko pro vytvoření uživatele v cloudu Atlassian</span><span class="sxs-lookup"><span data-stu-id="7cdc7-236">Click the **Create User** button to create a user in the Atlassian Cloud</span></span>

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. <span data-ttu-id="7cdc7-238">Zadejte uživatele **e-mailová adresa**, **uživatelské jméno**, a **úplný název** a přiřadit přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-238">Enter the user's **Email address**, **Username**, and **Full Name** and assign the application access.</span></span> 

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. <span data-ttu-id="7cdc7-240">Klikněte na tlačítko **vytvořit uživateli** tlačítko odešle e-mailová pozvánka pro uživatele a uživatel se bude po přijetí pozvánky v systému aktivní.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-240">Click **Create user** button, it will send the email invitation to the user and after accepting the invitation the user will be active in the system.</span></span> 

>[!NOTE] 
><span data-ttu-id="7cdc7-241">Můžete také vytvořit uživatele hromadné kliknutím **vytvořit hromadné** tlačítko v části uživatelé.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-241">You can also create the bulk users by clicking the **Bulk Create** button in the Users section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7cdc7-242">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7cdc7-242">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7cdc7-243">V této části povolíte Britta Simon používat tak, že udělíte přístup do cloudu Atlassian Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-243">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Atlassian Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7cdc7-245">**Přiřadit Britta Simon Atlassian cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7cdc7-245">**To assign Britta Simon to Atlassian Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7cdc7-246">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-246">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7cdc7-248">V seznamu aplikací vyberte **Atlassian cloudu**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-248">In the applications list, select **Atlassian Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. <span data-ttu-id="7cdc7-250">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-250">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7cdc7-252">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-252">Click **Add** button.</span></span> <span data-ttu-id="7cdc7-253">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-253">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7cdc7-255">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-255">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7cdc7-256">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-256">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7cdc7-257">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-257">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7cdc7-258">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7cdc7-258">Testing single sign-on</span></span>

<span data-ttu-id="7cdc7-259">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-259">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="7cdc7-260">Když kliknete na dlaždici Atlassian cloudu na přístupovém panelu, jste měli získat automaticky přihlášení k Atlassian cloudové aplikace.</span><span class="sxs-lookup"><span data-stu-id="7cdc7-260">When you click the Atlassian Cloud tile in the Access Panel, you should get automatically signed-on to your Atlassian Cloud application.</span></span> <span data-ttu-id="7cdc7-261">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7cdc7-261">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7cdc7-262">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7cdc7-262">Additional resources</span></span>

* [<span data-ttu-id="7cdc7-263">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7cdc7-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7cdc7-264">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7cdc7-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

