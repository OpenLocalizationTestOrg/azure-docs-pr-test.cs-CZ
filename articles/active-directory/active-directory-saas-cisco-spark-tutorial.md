---
title: 'Kurz: Azure Active Directory integrace s Cisco Spark | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Cisco Spark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a0a221622afe1c801a331e2319f3a7ace3111dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a><span data-ttu-id="617b3-103">Kurz: Azure Active Directory integrace s Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="617b3-103">Tutorial: Azure Active Directory integration with Cisco Spark</span></span>

<span data-ttu-id="617b3-104">V tomto kurzu zjistěte, jak integrovat Cisco Spark v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="617b3-104">In this tutorial, you learn how to integrate Cisco Spark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="617b3-105">Integrace Cisco Spark s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="617b3-105">Integrating Cisco Spark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="617b3-106">Můžete řídit ve službě Azure AD, který má přístup k Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="617b3-106">You can control in Azure AD who has access to Cisco Spark</span></span>
- <span data-ttu-id="617b3-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Cisco Spark (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="617b3-107">You can enable your users to automatically get signed-on to Cisco Spark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="617b3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="617b3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="617b3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="617b3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="617b3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="617b3-110">Prerequisites</span></span>

<span data-ttu-id="617b3-111">Ke konfiguraci integrace služby Azure AD s Cisco Spark, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="617b3-111">To configure Azure AD integration with Cisco Spark, you need the following items:</span></span>

- <span data-ttu-id="617b3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="617b3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="617b3-113">Cisco Spark jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="617b3-113">A Cisco Spark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="617b3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="617b3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="617b3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="617b3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="617b3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="617b3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="617b3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="617b3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="617b3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="617b3-118">Scenario description</span></span>
<span data-ttu-id="617b3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="617b3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="617b3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="617b3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="617b3-121">Přidání Cisco Spark z Galerie</span><span class="sxs-lookup"><span data-stu-id="617b3-121">Adding Cisco Spark from the gallery</span></span>
2. <span data-ttu-id="617b3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="617b3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cisco-spark-from-the-gallery"></a><span data-ttu-id="617b3-123">Přidání Cisco Spark z Galerie</span><span class="sxs-lookup"><span data-stu-id="617b3-123">Adding Cisco Spark from the gallery</span></span>
<span data-ttu-id="617b3-124">Při konfiguraci integrace Spark Cisco do služby Azure AD, potřebujete přidat Cisco Spark z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="617b3-124">To configure the integration of Cisco Spark into Azure AD, you need to add Cisco Spark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="617b3-125">**Pokud chcete přidat Cisco Spark z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="617b3-125">**To add Cisco Spark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="617b3-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="617b3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="617b3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="617b3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="617b3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="617b3-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="617b3-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="617b3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="617b3-133">Do vyhledávacího pole zadejte **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="617b3-133">In the search box, type **Cisco Spark**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. <span data-ttu-id="617b3-135">Na panelu výsledků vyberte **Cisco Spark**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="617b3-135">In the results panel, select **Cisco Spark**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="617b3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="617b3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="617b3-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cisco Spark podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="617b3-138">In this section, you configure and test Azure AD single sign-on with Cisco Spark based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="617b3-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Cisco Spark je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="617b3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cisco Spark is to a user in Azure AD.</span></span> <span data-ttu-id="617b3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Cisco Spark je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="617b3-140">In other words, a link relationship between an Azure AD user and the related user in Cisco Spark needs to be established.</span></span>

<span data-ttu-id="617b3-141">V Cisco Spark přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="617b3-141">In Cisco Spark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="617b3-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cisco Spark, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="617b3-142">To configure and test Azure AD single sign-on with Cisco Spark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="617b3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="617b3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="617b3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="617b3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="617b3-145">**[Vytvoření zkušebního uživatele Cisco Spark](#creating-a-cisco-spark-test-user)**  – Pokud chcete mít protějšek Britta Simon v Cisco Spark, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="617b3-145">**[Creating a Cisco Spark test user](#creating-a-cisco-spark-test-user)** - to have a counterpart of Britta Simon in Cisco Spark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="617b3-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="617b3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="617b3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="617b3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="617b3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="617b3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="617b3-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="617b3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cisco Spark application.</span></span>

<span data-ttu-id="617b3-150">**Ke konfiguraci Azure AD jednotné přihlašování s Cisco Spark, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="617b3-150">**To configure Azure AD single sign-on with Cisco Spark, perform the following steps:**</span></span>

1. <span data-ttu-id="617b3-151">Na portálu Azure na **Cisco Spark** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="617b3-151">In the Azure portal, on the **Cisco Spark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="617b3-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="617b3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. <span data-ttu-id="617b3-155">Na **Cisco Spark domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="617b3-155">On the **Cisco Spark Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    <span data-ttu-id="617b3-157">a.</span><span class="sxs-lookup"><span data-stu-id="617b3-157">a.</span></span> <span data-ttu-id="617b3-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://web.ciscospark.com/#/signin`</span><span class="sxs-lookup"><span data-stu-id="617b3-158">In the **Sign-on URL** textbox, type a URL as: `https://web.ciscospark.com/#/signin`</span></span>

    <span data-ttu-id="617b3-159">b.</span><span class="sxs-lookup"><span data-stu-id="617b3-159">b.</span></span> <span data-ttu-id="617b3-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://idbroker.webex.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="617b3-160">In the **Identifier** textbox, type a URL using the following pattern: `https://idbroker.webex.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="617b3-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="617b3-161">This value is not real.</span></span> <span data-ttu-id="617b3-162">Aktualizujte tato hodnota se skutečným identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="617b3-162">Update this value with the actual Identifier.</span></span> <span data-ttu-id="617b3-163">Obraťte se na [tým podpory Cisco Spark klienta](https://support.ciscospark.com/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="617b3-163">Contact [Cisco Spark Client support team](https://support.ciscospark.com/) to get this value.</span></span> 
 
4. <span data-ttu-id="617b3-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="617b3-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. <span data-ttu-id="617b3-166">Aplikací Spark Cisco očekává kontrolní výrazy SAML tak, aby obsahovala určité atributy.</span><span class="sxs-lookup"><span data-stu-id="617b3-166">Cisco Spark application expects the SAML assertions to contain specific attributes.</span></span> <span data-ttu-id="617b3-167">Nakonfigurujte následující atributy pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="617b3-167">Configure the following attributes  for this application.</span></span> <span data-ttu-id="617b3-168">Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="617b3-168">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="617b3-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="617b3-169">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. <span data-ttu-id="617b3-171">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="617b3-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="617b3-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="617b3-172">Attribute Name</span></span>  | <span data-ttu-id="617b3-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="617b3-173">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    |   <span data-ttu-id="617b3-174">UID</span><span class="sxs-lookup"><span data-stu-id="617b3-174">uid</span></span>    | <span data-ttu-id="617b3-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="617b3-175">user.userprincipalname</span></span> |   

    <span data-ttu-id="617b3-176">a.</span><span class="sxs-lookup"><span data-stu-id="617b3-176">a.</span></span> <span data-ttu-id="617b3-177">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="617b3-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="617b3-180">b.</span><span class="sxs-lookup"><span data-stu-id="617b3-180">b.</span></span> <span data-ttu-id="617b3-181">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="617b3-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="617b3-182">c.</span><span class="sxs-lookup"><span data-stu-id="617b3-182">c.</span></span> <span data-ttu-id="617b3-183">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="617b3-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="617b3-184">d.</span><span class="sxs-lookup"><span data-stu-id="617b3-184">d.</span></span> <span data-ttu-id="617b3-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="617b3-185">Click **Ok**.</span></span>

7. <span data-ttu-id="617b3-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="617b3-186">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="617b3-188">Přihlaste se k [správu spolupráce cloudu Cisco](https://admin.ciscospark.com/) pomocí svých přihlašovacích údajů správce s úplnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="617b3-188">Sign in to [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

9. <span data-ttu-id="617b3-189">Vyberte **nastavení** a v části **ověřování** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="617b3-189">Select **Settings** and under the **Authentication** section, click **Modify**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. <span data-ttu-id="617b3-191">Vyberte **integrovat zprostředkovatele identity 3. stran. (Rozšířené)**  a přejděte na další obrazovce.</span><span class="sxs-lookup"><span data-stu-id="617b3-191">Select **Integrate a 3rd-party identity provider. (Advanced)** and go to the next screen.</span></span>

11. <span data-ttu-id="617b3-192">Na **importovat Idp Metadata** stránky, buď přetažení a odstraňte soubor metadat Azure AD na stránku nebo použijte možnost prohlížeče souboru najděte a nahrajte soubor metadat Azure AD.</span><span class="sxs-lookup"><span data-stu-id="617b3-192">On the **Import Idp Metadata** page, either drag and drop the Azure AD metadata file onto the page or use the file browser option to locate and upload the Azure AD metadata file.</span></span> <span data-ttu-id="617b3-193">Pak vyberte **vyžadovat certifikát podepsaný certifikační autoritou v metadatech (bezpečnější)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="617b3-193">Then, select **Require certificate signed by a certificate authority in Metadata (more secure)** and click **Next**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. <span data-ttu-id="617b3-195">Vyberte **Test jednotného přihlašování k připojení**a když se otevře novou kartu prohlížeče, ověřování s Azure AD po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="617b3-195">Select **Test SSO Connection**, and when a new browser tab opens, authenticate with Azure AD by signing in.</span></span>

13. <span data-ttu-id="617b3-196">Vraťte se na **správu spolupráce cloudu Cisco** kartu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="617b3-196">Return to the **Cisco Cloud Collaboration Management** browser tab.</span></span> <span data-ttu-id="617b3-197">Pokud byl test úspěšný, vyberte **tento test proběhl úspěšně. Možnost jednotného přihlašování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="617b3-197">If the test was successful, select **This test was successful. Enable Single Sign-On option** and click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="617b3-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="617b3-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="617b3-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="617b3-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="617b3-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="617b3-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="617b3-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="617b3-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="617b3-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="617b3-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="617b3-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="617b3-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="617b3-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="617b3-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="617b3-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="617b3-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="617b3-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="617b3-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="617b3-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="617b3-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="617b3-213">a.</span><span class="sxs-lookup"><span data-stu-id="617b3-213">a.</span></span> <span data-ttu-id="617b3-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="617b3-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="617b3-215">b.</span><span class="sxs-lookup"><span data-stu-id="617b3-215">b.</span></span> <span data-ttu-id="617b3-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="617b3-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="617b3-217">c.</span><span class="sxs-lookup"><span data-stu-id="617b3-217">c.</span></span> <span data-ttu-id="617b3-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="617b3-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="617b3-219">d.</span><span class="sxs-lookup"><span data-stu-id="617b3-219">d.</span></span> <span data-ttu-id="617b3-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="617b3-220">Click **Create**.</span></span>
 
### <a name="creating-a-cisco-spark-test-user"></a><span data-ttu-id="617b3-221">Vytvoření zkušebního uživatele Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="617b3-221">Creating a Cisco Spark test user</span></span>

<span data-ttu-id="617b3-222">V této části vytvoříte uživatele volal Britta Simon v Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="617b3-222">In this section, you create a user called Britta Simon in Cisco Spark.</span></span> <span data-ttu-id="617b3-223">V této části vytvoříte uživatele volal Britta Simon v Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="617b3-223">In this section, you create a user called Britta Simon in Cisco Spark.</span></span>

1. <span data-ttu-id="617b3-224">Přejděte na [správu spolupráce cloudu Cisco](https://admin.ciscospark.com/) pomocí svých přihlašovacích údajů správce s úplnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="617b3-224">Go to the [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

2. <span data-ttu-id="617b3-225">Klikněte na tlačítko **uživatelé** a potom **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="617b3-225">Click **Users** and then **Manage Users**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. <span data-ttu-id="617b3-227">V **spravovat uživatelská** vyberte **ručně přidat nebo změnit uživatele** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="617b3-227">In the **Manage User** window, select **Manually add or modify users** and click **Next**.</span></span>

4. <span data-ttu-id="617b3-228">Vyberte **názvy a e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="617b3-228">Select **Names and Email address**.</span></span> <span data-ttu-id="617b3-229">Potom zadejte do textového pole následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="617b3-229">Then, fill out the textbox as follows:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    <span data-ttu-id="617b3-231">a.</span><span class="sxs-lookup"><span data-stu-id="617b3-231">a.</span></span> <span data-ttu-id="617b3-232">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="617b3-232">In the **First Name** textbox, type **Britta**.</span></span> 
    
    <span data-ttu-id="617b3-233">b.</span><span class="sxs-lookup"><span data-stu-id="617b3-233">b.</span></span> <span data-ttu-id="617b3-234">V **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="617b3-234">In the **Last Name** textbox, type **Simon**.</span></span>
    
    <span data-ttu-id="617b3-235">c.</span><span class="sxs-lookup"><span data-stu-id="617b3-235">c.</span></span> <span data-ttu-id="617b3-236">V **e-mailová adresa** textovému poli, typ  **britta.simon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="617b3-236">In the **Email address** textbox, type **britta.simon@contoso.com**.</span></span>

5. <span data-ttu-id="617b3-237">Klikněte na symbol plus přidat Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="617b3-237">Click the plus sign to add Britta Simon.</span></span> <span data-ttu-id="617b3-238">Pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="617b3-238">Then, click **Next**.</span></span>

6. <span data-ttu-id="617b3-239">V **přidat služby pro uživatele** okně klikněte na tlačítko **Uložit** a potom **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="617b3-239">In the **Add Services for Users** window, click **Save** and then **Finish**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="617b3-240">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="617b3-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="617b3-241">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="617b3-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cisco Spark.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="617b3-243">**Pokud chcete přiřadit Britta Simon Cisco Spark, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="617b3-243">**To assign Britta Simon to Cisco Spark, perform the following steps:**</span></span>

1. <span data-ttu-id="617b3-244">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="617b3-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="617b3-246">V seznamu aplikací vyberte **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="617b3-246">In the applications list, select **Cisco Spark**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. <span data-ttu-id="617b3-248">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="617b3-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="617b3-250">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="617b3-250">Click **Add** button.</span></span> <span data-ttu-id="617b3-251">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="617b3-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="617b3-253">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="617b3-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="617b3-254">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="617b3-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="617b3-255">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="617b3-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="617b3-256">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="617b3-256">Testing single sign-on</span></span>

<span data-ttu-id="617b3-257">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="617b3-257">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="617b3-258">Když kliknete na dlaždici Cisco Spark na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="617b3-258">When you click the Cisco Spark tile in the Access Panel, you should get automatically signed-on to your Cisco Spark application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="617b3-259">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="617b3-259">Additional resources</span></span>

* [<span data-ttu-id="617b3-260">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="617b3-260">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="617b3-261">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="617b3-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

