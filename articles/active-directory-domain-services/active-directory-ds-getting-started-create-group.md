---
title: "Azure Active Directory Domain Services: Vytvořit skupinu Správci řadič domény Azure AD | Microsoft Docs"
description: "Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: 5ed0125e05928cf0f6d9941e099b433ecb46e6e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a><span data-ttu-id="a20ef-103">Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="a20ef-103">Enable Azure Active Directory Domain Services using the Azure classic portal</span></span>
<span data-ttu-id="a20ef-104">Tento článek popisuje a provede úlohy konfigurace, které jsou požadovány pro povolit Azure Active Directory Domain Services (Azure AD DS) pro klienta služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a20ef-104">This article describes and walks through the configuration tasks that are required for you to enable Azure Active Directory Domain Services (Azure AD DS) for your Azure Active Directory (Azure AD) tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="a20ef-105">[**Místo toho zkuste nové rozhraní portálu (preview) Azure**](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="a20ef-105">[**Try the new (preview) Azure portal experience instead**](active-directory-ds-getting-started.md).</span></span> 
>

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a><span data-ttu-id="a20ef-106">Úloha 1: vytvoření skupiny administrators řadič domény Azure AD</span><span class="sxs-lookup"><span data-stu-id="a20ef-106">Task 1: create the Azure AD DC administrators group</span></span>
<span data-ttu-id="a20ef-107">První úlohou je vytvoření skupiny pro správu v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a20ef-107">The first task is to create an administrative group in your Azure AD tenant.</span></span> <span data-ttu-id="a20ef-108">Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*.</span><span class="sxs-lookup"><span data-stu-id="a20ef-108">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="a20ef-109">Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně do Azure Active Directory Domain Services spravované domény.</span><span class="sxs-lookup"><span data-stu-id="a20ef-109">Members of this group are granted administrative permissions on machines that are domain-joined to the Azure Active Directory Domain Services-managed domain.</span></span> <span data-ttu-id="a20ef-110">Na počítačích připojených k doméně je této skupiny přidat do skupiny administrators.</span><span class="sxs-lookup"><span data-stu-id="a20ef-110">On domain-joined machines, this group is added to the administrators group.</span></span> <span data-ttu-id="a20ef-111">Členové této skupiny navíc můžete použít vzdálené plochy se vzdáleně připojit k doméně počítače.</span><span class="sxs-lookup"><span data-stu-id="a20ef-111">Additionally, members of this group can use Remote Desktop to connect remotely to domain-joined machines.</span></span>  

> [!NOTE]
> <span data-ttu-id="a20ef-112">Nemáte oprávnění správce domény nebo správce podnikové sítě na spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a20ef-112">You do not have Domain Administrator or Enterprise Administrator permissions on the managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="a20ef-113">Na spravovaných domén tato oprávnění jsou vyhrazené pomocí služby a nejsou k dispozici uživatelům v rámci klienta.</span><span class="sxs-lookup"><span data-stu-id="a20ef-113">On managed domains, these permissions are reserved by the service and are not made available to users within the tenant.</span></span> <span data-ttu-id="a20ef-114">Ale můžete vytvořit v této úloze konfigurace speciální skupinu pro správu provádět některé privilegované operace.</span><span class="sxs-lookup"><span data-stu-id="a20ef-114">However, you can use the special administrative group created in this configuration task to perform some privileged operations.</span></span> <span data-ttu-id="a20ef-115">Tyto operace zahrnují připojení počítače k doméně, které patří do skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="a20ef-115">These operations include joining computers to the domain, belonging to the administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="a20ef-116">V této úloze konfigurace vytvořte skupiny pro správu a přidejte jeden nebo více uživatelů ve vašem adresáři do skupiny.</span><span class="sxs-lookup"><span data-stu-id="a20ef-116">In this configuration task, you create the administrative group and add one or more users in your directory to the group.</span></span> <span data-ttu-id="a20ef-117">Pokud chcete vytvořit skupinu pro správu pro Azure Active Directory Domain Services, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a20ef-117">To create the administrative group for Azure Active Directory Domain Services, do the following:</span></span>

1. <span data-ttu-id="a20ef-118">Přejděte na [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="a20ef-118">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="a20ef-119">V levém podokně vyberte tlačítko **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a20ef-119">In the left pane, select the **Active Directory** button.</span></span>
3. <span data-ttu-id="a20ef-120">Vyberte klienta Azure AD (adresář), pro který chcete povolit Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a20ef-120">Select the Azure AD tenant (directory) for which you want to enable Azure Active Directory Domain Services.</span></span> <span data-ttu-id="a20ef-121">Můžete vytvořit pouze jednu doménu pro každý adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a20ef-121">You can create only one domain for each Azure AD directory.</span></span>

    ![Vyberte adresář Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="a20ef-123">Na **preview directory** klikněte na tlačítko **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="a20ef-123">On the **preview directory** page, click the **Groups** tab.</span></span>
5. <span data-ttu-id="a20ef-124">Chcete-li přidat skupinu ke klientovi Azure AD, v podokně úloh v dolní části okna klikněte na tlačítko **přidat skupinu**.</span><span class="sxs-lookup"><span data-stu-id="a20ef-124">To add a group to your Azure AD tenant, on the task pane at the bottom of the window, click **Add Group**.</span></span>

    ![Tlačítko Přidat skupinu](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. <span data-ttu-id="a20ef-126">V **přidat skupinu** dialogové okno pole, vytvořte skupinu s názvem **AAD řadič domény správci**a poté nastavte **typ skupiny** k **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="a20ef-126">In the **Add Group** dialog box, create a group named **AAD DC Administrators**, and then set **Group Type** to **Security**.</span></span>

   > [!WARNING]
   > <span data-ttu-id="a20ef-127">Pokud chcete povolit přístup v rámci Azure Active Directory Domain Services spravované domény, vytvořte skupinu s tímto názvem přesný.</span><span class="sxs-lookup"><span data-stu-id="a20ef-127">To enable access within your Azure Active Directory Domain Services-managed domain, create a group with this exact name.</span></span>
   >
   >

    ![Dialogové okno Přidat skupinu](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. <span data-ttu-id="a20ef-129">V **popis** zadejte popis, který umožňuje ostatním uživatelům pochopit, že této skupině uděluje oprávnění pro správu v rámci Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="a20ef-129">In the **Description** box, enter a description that enables others to understand that this group grants administrative permissions within Azure Active Directory Domain Services.</span></span>
8. <span data-ttu-id="a20ef-130">Po vytvoření skupiny, klikněte na název skupiny a zobrazte její vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a20ef-130">After you've created the group, click the group name to view its properties.</span></span>
9. <span data-ttu-id="a20ef-131">Chcete-li přidat uživatele jako členy skupiny v dolní části okna klikněte na tlačítko **přidat členy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a20ef-131">To add users as members of the group, at the bottom of the window, click the **Add Members** button.</span></span>

    ![Přidání tlačítka členy skupiny](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. <span data-ttu-id="a20ef-133">V **přidat členy** dialogovém okně vyberte uživatele, kteří by měl být členy této skupiny a pak klikněte na ikonu zaškrtnutí vpravo dole.</span><span class="sxs-lookup"><span data-stu-id="a20ef-133">In the **Add members** dialog box, select the users who should be members of this group, and then click the checkmark icon at the lower right.</span></span>

    ![Přidání uživatelů do skupiny administrators](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a><span data-ttu-id="a20ef-135">Další krok</span><span class="sxs-lookup"><span data-stu-id="a20ef-135">Next step</span></span>
[<span data-ttu-id="a20ef-136">Úloha 2: vytvoření nebo výběr virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="a20ef-136">Task 2: create or select an Azure virtual network</span></span>](active-directory-ds-getting-started-vnet.md)
