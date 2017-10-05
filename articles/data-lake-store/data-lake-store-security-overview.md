---
title: "Přehled zabezpečení v Data Lake Store | Microsoft Docs"
description: "Pochopit, jak Azure Data Lake Store je bezpečnější úložiště velkých objemů dat"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: a481a1169e189318406bc672eb560aeedb41750f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="security-in-azure-data-lake-store"></a><span data-ttu-id="45658-103">Zabezpečení v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="45658-103">Security in Azure Data Lake Store</span></span>
<span data-ttu-id="45658-104">Mnoho podniků jsou využívat výhod analýzy velkých objemů dat podnikových statistik pomáhá jim inteligentní rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="45658-104">Many enterprises are taking advantage of big data analytics for business insights to help them make smart decisions.</span></span> <span data-ttu-id="45658-105">Organizace může mít složitý a regulovaná prostředí s roste počet různých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="45658-105">An organization might have a complex and regulated environment, with an increasing number of diverse users.</span></span> <span data-ttu-id="45658-106">Je důležité pro organizace a ujistěte se, bezpečněji, uložení kritickými podnikovými daty s správné úrovně udělení přístupu k jednotlivým uživatelům.</span><span class="sxs-lookup"><span data-stu-id="45658-106">It is vital for an enterprise to make sure that critical business data is stored more securely, with the correct level of access granted to individual users.</span></span> <span data-ttu-id="45658-107">Azure Data Lake Store je navržená tak, abyste splňovat tyto požadavky na zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="45658-107">Azure Data Lake Store is designed to help meet these security requirements.</span></span> <span data-ttu-id="45658-108">V tomto článku se dozvíte o funkcích zabezpečení Data Lake Store, včetně:</span><span class="sxs-lookup"><span data-stu-id="45658-108">In this article, learn about the security capabilities of Data Lake Store, including:</span></span>

* <span data-ttu-id="45658-109">Authentication</span><span class="sxs-lookup"><span data-stu-id="45658-109">Authentication</span></span>
* <span data-ttu-id="45658-110">Autorizace</span><span class="sxs-lookup"><span data-stu-id="45658-110">Authorization</span></span>
* <span data-ttu-id="45658-111">Izolace sítě</span><span class="sxs-lookup"><span data-stu-id="45658-111">Network isolation</span></span>
* <span data-ttu-id="45658-112">Ochrana dat</span><span class="sxs-lookup"><span data-stu-id="45658-112">Data protection</span></span>
* <span data-ttu-id="45658-113">Auditování</span><span class="sxs-lookup"><span data-stu-id="45658-113">Auditing</span></span>

## <a name="authentication-and-identity-management"></a><span data-ttu-id="45658-114">Ověřování a identita správy</span><span class="sxs-lookup"><span data-stu-id="45658-114">Authentication and identity management</span></span>
<span data-ttu-id="45658-115">Ověřování je proces, podle kterého je ověřit identitu uživatele, když uživatel pracuje s Data Lake Store nebo s jakoukoli službu, která se připojuje k Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-115">Authentication is the process by which a user's identity is verified when the user interacts with Data Lake Store or with any service that connects to Data Lake Store.</span></span> <span data-ttu-id="45658-116">Pro správu identit a ověření Data Lake Store využívá [Azure Active Directory](../active-directory/active-directory-whatis.md), komplexní přístupu a identit a správy cloudové řešení, které zjednodušuje správu uživatelů a skupin.</span><span class="sxs-lookup"><span data-stu-id="45658-116">For identity management and authentication, Data Lake Store uses [Azure Active Directory](../active-directory/active-directory-whatis.md), a comprehensive identity and access management cloud solution that simplifies the management of users and groups.</span></span>

<span data-ttu-id="45658-117">Každé předplatné Azure může být přidružen k instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="45658-117">Each Azure subscription can be associated with an instance of Azure Active Directory.</span></span> <span data-ttu-id="45658-118">Pouze uživatele a identity služby, které jsou definované ve službě Azure Active Directory přístup účtu Data Lake Store pomocí portálu Azure, nástroje příkazového řádku, nebo prostřednictvím klientské aplikace sestavení vaší organizace pomocí Azure Data Lake Sada SDK úložiště.</span><span class="sxs-lookup"><span data-stu-id="45658-118">Only users and service identities that are defined in your Azure Active Directory service can access your Data Lake Store account, by using the Azure portal, command-line tools, or through client applications your organization builds by using the Azure Data Lake Store SDK.</span></span> <span data-ttu-id="45658-119">Klíčové výhody použití služby Azure Active Directory jako mechanismus řízení centralizované přístupu jsou:</span><span class="sxs-lookup"><span data-stu-id="45658-119">Key advantages of using Azure Active Directory as a centralized access control mechanism are:</span></span>

* <span data-ttu-id="45658-120">Zjednodušená správa životního cyklu identity.</span><span class="sxs-lookup"><span data-stu-id="45658-120">Simplified identity lifecycle management.</span></span> <span data-ttu-id="45658-121">Identity uživatele nebo služby (hlavní identity služby) můžete rychle vytvořit a rychle odvolán jednoduše odstranit nebo zakázat účet v adresáři.</span><span class="sxs-lookup"><span data-stu-id="45658-121">The identity of a user or a service (a service principal identity) can be quickly created and quickly revoked by simply deleting or disabling the account in the directory.</span></span>
* <span data-ttu-id="45658-122">Služba Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="45658-122">Multi-factor authentication.</span></span> <span data-ttu-id="45658-123">[Služba Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication.md) poskytuje další úroveň zabezpečení pro uživatelská přihlášení a transakce.</span><span class="sxs-lookup"><span data-stu-id="45658-123">[Multi-factor authentication](../multi-factor-authentication/multi-factor-authentication.md) provides an additional layer of security for user sign-ins and transactions.</span></span>
* <span data-ttu-id="45658-124">Ověřování z libovolného klienta přes standardní otevřete protokol, jako je například účtu OAuth nebo OpenID.</span><span class="sxs-lookup"><span data-stu-id="45658-124">Authentication from any client through a standard open protocol, such as OAuth or OpenID.</span></span>
* <span data-ttu-id="45658-125">Federaci se enterprise adresářových služeb a zprostředkovatelů identity cloudu.</span><span class="sxs-lookup"><span data-stu-id="45658-125">Federation with enterprise directory services and cloud identity providers.</span></span>

## <a name="authorization-and-access-control"></a><span data-ttu-id="45658-126">Autorizace a řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="45658-126">Authorization and access control</span></span>
<span data-ttu-id="45658-127">Po Azure Active Directory ověřuje uživatele tak, že má uživatel přístup Azure Data Lake Store, ovládací prvky autorizace přístupová oprávnění pro Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-127">After Azure Active Directory authenticates a user so that the user can access Azure Data Lake Store, authorization controls access permissions for Data Lake Store.</span></span> <span data-ttu-id="45658-128">Data Lake Store odděluje autorizace aktivity souvisejícím s účtem a data související s následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="45658-128">Data Lake Store separates authorization for account-related and data-related activities in the following manner:</span></span>

* <span data-ttu-id="45658-129">[Řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovaný platformou Azure pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="45658-129">[Role-based access control](../active-directory/role-based-access-control-what-is.md) (RBAC) provided by Azure for account management</span></span>
* <span data-ttu-id="45658-130">POSIX seznamu ACL pro přístup k datům v úložišti</span><span class="sxs-lookup"><span data-stu-id="45658-130">POSIX ACL for accessing data in the store</span></span>

### <a name="rbac-for-account-management"></a><span data-ttu-id="45658-131">RBAC pro správu účtu</span><span class="sxs-lookup"><span data-stu-id="45658-131">RBAC for account management</span></span>
<span data-ttu-id="45658-132">Čtyři základní role jsou definovány pro Data Lake Store ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="45658-132">Four basic roles are defined for Data Lake Store by default.</span></span> <span data-ttu-id="45658-133">Role umožňují různé operace v účtu Data Lake Store prostřednictvím portálu Azure, rutiny prostředí PowerShell a rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="45658-133">The roles permit different operations on a Data Lake Store account via the Azure portal, PowerShell cmdlets, and REST APIs.</span></span> <span data-ttu-id="45658-134">Role vlastník a Přispěvatel provádět celou řadu funkcí správy na účet.</span><span class="sxs-lookup"><span data-stu-id="45658-134">The Owner and Contributor roles can perform a variety of administration functions on the account.</span></span> <span data-ttu-id="45658-135">Můžete přiřadit role Čtenář pro uživatele, kteří pouze interagovat s daty.</span><span class="sxs-lookup"><span data-stu-id="45658-135">You can assign the Reader role to users who only interact with data.</span></span>

<span data-ttu-id="45658-136">![Role RBAC](./media/data-lake-store-security-overview/rbac-roles.png "role RBAC")</span><span class="sxs-lookup"><span data-stu-id="45658-136">![RBAC roles](./media/data-lake-store-security-overview/rbac-roles.png "RBAC roles")</span></span>

<span data-ttu-id="45658-137">Všimněte si, že i když role přiřazené pro správu účtu, některé role ovlivnit přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="45658-137">Note that although roles are assigned for account management, some roles affect access to data.</span></span> <span data-ttu-id="45658-138">Budete muset použít seznamy řízení přístupu k řízení přístupu k operace, které může uživatel provádět v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="45658-138">You need to use ACLs to control access to operations that a user can perform on the file system.</span></span> <span data-ttu-id="45658-139">Následující tabulka obsahuje souhrn práva pro správu a data přístupová práva pro výchozí role.</span><span class="sxs-lookup"><span data-stu-id="45658-139">The following table shows a summary of management rights and data access rights for the default roles.</span></span>

| <span data-ttu-id="45658-140">Role</span><span class="sxs-lookup"><span data-stu-id="45658-140">Roles</span></span> | <span data-ttu-id="45658-141">Práva pro správu</span><span class="sxs-lookup"><span data-stu-id="45658-141">Management rights</span></span> | <span data-ttu-id="45658-142">Data přístupová práva</span><span class="sxs-lookup"><span data-stu-id="45658-142">Data access rights</span></span> | <span data-ttu-id="45658-143">Vysvětlení</span><span class="sxs-lookup"><span data-stu-id="45658-143">Explanation</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="45658-144">Žádné přiřazenou roli</span><span class="sxs-lookup"><span data-stu-id="45658-144">No role assigned</span></span> |<span data-ttu-id="45658-145">Žádný</span><span class="sxs-lookup"><span data-stu-id="45658-145">None</span></span> |<span data-ttu-id="45658-146">Řídí seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="45658-146">Governed by ACL</span></span> |<span data-ttu-id="45658-147">Uživatel nemůže použít portál Azure nebo rutin prostředí Azure PowerShell procházet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-147">The user cannot use the Azure portal or Azure PowerShell cmdlets to browse Data Lake Store.</span></span> <span data-ttu-id="45658-148">Uživatel může použít jenom nástroje příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="45658-148">The user can use command-line tools only.</span></span> |
| <span data-ttu-id="45658-149">Vlastník</span><span class="sxs-lookup"><span data-stu-id="45658-149">Owner</span></span> |<span data-ttu-id="45658-150">Všechny</span><span class="sxs-lookup"><span data-stu-id="45658-150">All</span></span> |<span data-ttu-id="45658-151">Všechny</span><span class="sxs-lookup"><span data-stu-id="45658-151">All</span></span> |<span data-ttu-id="45658-152">Role vlastníka je superuživatele.</span><span class="sxs-lookup"><span data-stu-id="45658-152">The Owner role is a superuser.</span></span> <span data-ttu-id="45658-153">Tato role můžou spravovat všechno a má úplný přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="45658-153">This role can manage everything and has full access to data.</span></span> |
| <span data-ttu-id="45658-154">Čtenář</span><span class="sxs-lookup"><span data-stu-id="45658-154">Reader</span></span> |<span data-ttu-id="45658-155">jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="45658-155">Read-only</span></span> |<span data-ttu-id="45658-156">Řídí seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="45658-156">Governed by ACL</span></span> |<span data-ttu-id="45658-157">Role čtenáře můžou zobrazit všechno týkající se správy účtů, například která je přiřazena uživatele které role.</span><span class="sxs-lookup"><span data-stu-id="45658-157">The Reader role can view everything regarding account management, such as which user is assigned to which role.</span></span> <span data-ttu-id="45658-158">Role čtenáře nelze provést žádné změny.</span><span class="sxs-lookup"><span data-stu-id="45658-158">The Reader role can't make any changes.</span></span> |
| <span data-ttu-id="45658-159">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="45658-159">Contributor</span></span> |<span data-ttu-id="45658-160">Všechny kromě přidání a odebrání rolí</span><span class="sxs-lookup"><span data-stu-id="45658-160">All except add and remove roles</span></span> |<span data-ttu-id="45658-161">Řídí seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="45658-161">Governed by ACL</span></span> |<span data-ttu-id="45658-162">Role Přispěvatel můžete spravovat některé aspekty účtu, jako je například nasazení a vytváření a Správa výstrah.</span><span class="sxs-lookup"><span data-stu-id="45658-162">The Contributor role can manage some aspects of an account, such as deployments and creating and managing alerts.</span></span> <span data-ttu-id="45658-163">Role Přispěvatel nelze přidat nebo odebrat role.</span><span class="sxs-lookup"><span data-stu-id="45658-163">The Contributor role cannot add or remove roles.</span></span> |
| <span data-ttu-id="45658-164">Správce přístupu uživatelů</span><span class="sxs-lookup"><span data-stu-id="45658-164">User Access Administrator</span></span> |<span data-ttu-id="45658-165">Přidání a odebrání rolí</span><span class="sxs-lookup"><span data-stu-id="45658-165">Add and remove roles</span></span> |<span data-ttu-id="45658-166">Řídí seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="45658-166">Governed by ACL</span></span> |<span data-ttu-id="45658-167">Role správce přístupu uživatelů můžete spravovat přístup uživatelů k účtům.</span><span class="sxs-lookup"><span data-stu-id="45658-167">The User Access Administrator role can manage user access to accounts.</span></span> |

<span data-ttu-id="45658-168">Pokyny najdete v tématu [přiřadit uživatele nebo skupiny zabezpečení účtů Data Lake Store](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).</span><span class="sxs-lookup"><span data-stu-id="45658-168">For instructions, see [Assign users or security groups to Data Lake Store accounts](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).</span></span>

### <a name="using-acls-for-operations-on-file-systems"></a><span data-ttu-id="45658-169">Pomocí seznamů řízení přístupu pro operace v systémech souborů.</span><span class="sxs-lookup"><span data-stu-id="45658-169">Using ACLs for operations on file systems</span></span>
<span data-ttu-id="45658-170">Data Lake Store je systém souborů hierarchické jako Hadoop Distributed File System (HDFS) a podporuje [seznamy ACL POSIX](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists).</span><span class="sxs-lookup"><span data-stu-id="45658-170">Data Lake Store is a hierarchical file system like Hadoop Distributed File System (HDFS), and it supports [POSIX ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists).</span></span> <span data-ttu-id="45658-171">Ovládá pro čtení (r), zápis (w) a spouštět (oprávnění k prostředkům pro roli vlastníka pro vlastníky skupiny a pro ostatní uživatele a skupiny x).</span><span class="sxs-lookup"><span data-stu-id="45658-171">It controls read (r), write (w), and execute (x) permissions to resources for the Owner role, for the Owners group, and for other users and groups.</span></span> <span data-ttu-id="45658-172">V Data Lake Store Public Preview (aktuální verze) jde seznamy řízení přístupu zapnout u kořenové složky, podsložek a jednotlivých souborů.</span><span class="sxs-lookup"><span data-stu-id="45658-172">In the Data Lake Store Public Preview (the current release), ACLs can be enabled on the root folder, on subfolders, and on individual files.</span></span> <span data-ttu-id="45658-173">Další informace o fungování seznamů řízení přístupu v souvislosti s Data Lake Storem najdete v tématu [Řízení přístupu v Data Lake Storu](data-lake-store-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="45658-173">For more information on how ACLs work in context of Data Lake Store, see [Access control in Data Lake Store](data-lake-store-access-control.md).</span></span>

<span data-ttu-id="45658-174">Doporučujeme, abyste definovat seznamy ACL pro více uživatelů pomocí [skupiny zabezpečení](../active-directory/active-directory-accessmanagement-manage-groups.md).</span><span class="sxs-lookup"><span data-stu-id="45658-174">We recommend that you define ACLs for multiple users by using [security groups](../active-directory/active-directory-accessmanagement-manage-groups.md).</span></span> <span data-ttu-id="45658-175">Přidat uživatele do skupiny zabezpečení a pak mu přiřaďte seznamy ACL pro soubor nebo složku do této skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="45658-175">Add users to a security group, and then assign the ACLs for a file or folder to that security group.</span></span> <span data-ttu-id="45658-176">To je užitečné, pokud chcete zadat vlastní přístup, protože jste omezeni na přidání maximálně devět položek pro vlastní přístup.</span><span class="sxs-lookup"><span data-stu-id="45658-176">This is useful when you want to provide custom access, because you are limited to adding a maximum of nine entries for custom access.</span></span> <span data-ttu-id="45658-177">Další informace o tom, jak lépe zabezpečit data uložená v Data Lake Store pomocí skupin zabezpečení služby Azure Active Directory najdete v tématu [přiřadit uživatele nebo skupiny zabezpečení jako seznamy řízení přístupu k systému souborů Azure Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span><span class="sxs-lookup"><span data-stu-id="45658-177">For more information about how to better secure data stored in Data Lake Store by using Azure Active Directory security groups, see [Assign users or security group as ACLs to the Azure Data Lake Store file system](data-lake-store-secure-data.md#filepermissions).</span></span>

<span data-ttu-id="45658-178">![Standardní a vlastní přístup](./media/data-lake-store-security-overview/adl.acl.2.png "standardní a vlastní přístup")</span><span class="sxs-lookup"><span data-stu-id="45658-178">![List standard and custom access](./media/data-lake-store-security-overview/adl.acl.2.png "List standard and custom access")</span></span>

## <a name="network-isolation"></a><span data-ttu-id="45658-179">Izolace sítě</span><span class="sxs-lookup"><span data-stu-id="45658-179">Network isolation</span></span>
<span data-ttu-id="45658-180">Použití Data Lake Store můžete lépe řízení přístupu k úložišti dat na úrovni sítě.</span><span class="sxs-lookup"><span data-stu-id="45658-180">Use Data Lake Store to help control access to your data store at the network level.</span></span> <span data-ttu-id="45658-181">Můžete určit brány firewall a definovat rozsah IP adres pro klienty důvěryhodné.</span><span class="sxs-lookup"><span data-stu-id="45658-181">You can establish firewalls and define an IP address range for your trusted clients.</span></span> <span data-ttu-id="45658-182">Díky rozsah IP adres můžete připojit pouze klienti, kteří mají IP adresu v definovaném rozsahu do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-182">With an IP address range, only clients that have an IP address within the defined range can connect to Data Lake Store.</span></span>

<span data-ttu-id="45658-183">![Nastavení brány firewall a IP přístup](./media/data-lake-store-security-overview/firewall-ip-access.png "nastavení a IP adresu brány Firewall")</span><span class="sxs-lookup"><span data-stu-id="45658-183">![Firewall settings and IP access](./media/data-lake-store-security-overview/firewall-ip-access.png "Firewall settings and IP address")</span></span>

## <a name="data-protection"></a><span data-ttu-id="45658-184">Ochrana dat</span><span class="sxs-lookup"><span data-stu-id="45658-184">Data protection</span></span>
<span data-ttu-id="45658-185">Azure Data Lake Store chrání vaše data v průběhu jejich životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="45658-185">Azure Data Lake Store protects your data throughout its life cycle.</span></span> <span data-ttu-id="45658-186">Pro data během přenosu Data Lake Store využívá standardní protokol zabezpečení TLS (Transport Layer) k zabezpečení dat přes síť.</span><span class="sxs-lookup"><span data-stu-id="45658-186">For data in transit, Data Lake Store uses the industry-standard Transport Layer Security (TLS) protocol to secure data over the network.</span></span>

<span data-ttu-id="45658-187">![Šifrování v Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "šifrování v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="45658-187">![Encryption in Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "Encryption in Data Lake Store")</span></span>

<span data-ttu-id="45658-188">Data Lake Store také zajišťuje šifrování dat, která jsou uložená v účtu.</span><span class="sxs-lookup"><span data-stu-id="45658-188">Data Lake Store also provides encryption for data that is stored in the account.</span></span> <span data-ttu-id="45658-189">Můžete zvolit, aby se vaše data šifrovala, nebo zvolit možnost bez šifrování.</span><span class="sxs-lookup"><span data-stu-id="45658-189">You can chose to have your data encrypted or opt for no encryption.</span></span> <span data-ttu-id="45658-190">Pokud můžete vyjádřit výslovný souhlas pro šifrování, je před ukládání na trvalé média šifrovat data uložená v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-190">If you opt in for encryption, data stored in Data Lake Store is encrypted prior to storing on persistent media.</span></span> <span data-ttu-id="45658-191">V takovém případě Data Lake Store automaticky šifruje data před uložením a dešifruje data před načtení, takže je pro klienta přístupu k datům zcela transparentní.</span><span class="sxs-lookup"><span data-stu-id="45658-191">In such a case, Data Lake Store automatically encrypts data prior to persisting and decrypts data prior to retrieval, so it is completely transparent to the client accessing the data.</span></span> <span data-ttu-id="45658-192">Neexistuje žádná změna kódu vyžaduje k šifrování a dešifrování dat na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="45658-192">There is no code change required on the client side to encrypt/decrypt data.</span></span>

<span data-ttu-id="45658-193">Pro správu klíčů Data Lake Store poskytuje dva režimy pro správu hlavní šifrovacích klíčů (MEKs), které jsou požadovány pro dešifrování žádná data, která je uložená v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-193">For key management, Data Lake Store provides two modes for managing your master encryption keys (MEKs), which are required for decrypting any data that is stored in the Data Lake Store.</span></span> <span data-ttu-id="45658-194">Můžete je nechat buď Data Lake Store můžete spravovat MEKs nebo zachovejte vlastnictví MEKs pomocí účtu Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="45658-194">You can either let Data Lake Store manage the MEKs for you, or choose to retain ownership of the MEKs using your Azure Key Vault account.</span></span> <span data-ttu-id="45658-195">Zadejte režim správy klíčů při při vytváření účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="45658-195">You specify the mode of key management while while creating a Data Lake Store account.</span></span> <span data-ttu-id="45658-196">Další informace o tom, jak provést konfiguraci související se šifrováním, najdete v tématu [Začínáme s Azure Data Lake Storem pomocí webu Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="45658-196">For more information on how to provide encryption-related configuration, see [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span>

## <a name="auditing-and-diagnostic-logs"></a><span data-ttu-id="45658-197">Protokoly auditování a diagnostiky</span><span class="sxs-lookup"><span data-stu-id="45658-197">Auditing and diagnostic logs</span></span>
<span data-ttu-id="45658-198">Můžete použít protokoly auditování a diagnostiky, v závislosti na tom, jestli hledáte protokoly pro týkajících se správy aktivit nebo aktivit souvisejících s daty.</span><span class="sxs-lookup"><span data-stu-id="45658-198">You can use auditing or diagnostic logs, depending on whether you are looking for logs for management-related activities or data-related activities.</span></span>

* <span data-ttu-id="45658-199">Aktivity související s správy pomocí rozhraní API Správce Azure Resource Manager a jsou prezentované na portálu Azure přes protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="45658-199">Management-related activities use Azure Resource Manager APIs and are surfaced in the Azure portal via audit logs.</span></span>
* <span data-ttu-id="45658-200">Aktivity související s data pomocí rozhraní REST API WebHDFS a jsou prezentované na portálu Azure prostřednictvím diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="45658-200">Data-related activities use WebHDFS REST APIs and are surfaced in the Azure portal via diagnostic logs.</span></span>

### <a name="auditing-logs"></a><span data-ttu-id="45658-201">Protokoly auditování</span><span class="sxs-lookup"><span data-stu-id="45658-201">Auditing logs</span></span>
<span data-ttu-id="45658-202">Abyste dosáhli souladu s předpisy, organizace může vyžadovat záznamy odpovídající auditu, pokud potřebuje a dostanete se do konkrétní incidenty.</span><span class="sxs-lookup"><span data-stu-id="45658-202">To comply with regulations, an organization might require adequate audit trails if it needs to dig into specific incidents.</span></span> <span data-ttu-id="45658-203">Data Lake Store má integrované sledování a auditování a protokoluje všechny aktivity správy účtu.</span><span class="sxs-lookup"><span data-stu-id="45658-203">Data Lake Store has built-in monitoring and auditing, and it logs all account management activities.</span></span>

<span data-ttu-id="45658-204">Účet správy pro záznamy pro audit zobrazit a vybrat sloupce, které chcete protokolovat.</span><span class="sxs-lookup"><span data-stu-id="45658-204">For account management audit trails, view and choose the columns that you want to log.</span></span> <span data-ttu-id="45658-205">Protokoly auditu a také můžete exportovat do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="45658-205">You also can export audit logs to Azure Storage.</span></span>

<span data-ttu-id="45658-206">![Protokoly auditu](./media/data-lake-store-security-overview/audit-logs.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="45658-206">![Audit logs](./media/data-lake-store-security-overview/audit-logs.png "Audit logs")</span></span>

### <a name="diagnostic-logs"></a><span data-ttu-id="45658-207">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="45658-207">Diagnostic logs</span></span>
<span data-ttu-id="45658-208">Můžete nastavit záznamy auditu přístupu k datům na portálu Azure (v nastavení pro diagnostiku) a vytvoření účtu úložiště objektů Blob v Azure, kde jsou uloženy protokoly.</span><span class="sxs-lookup"><span data-stu-id="45658-208">You can set data access audit trails in the Azure portal (in Diagnostic Settings) and create an Azure Blob storage account where the logs are stored.</span></span>

<span data-ttu-id="45658-209">![Diagnostické protokoly](./media/data-lake-store-security-overview/diagnostic-logs.png "diagnostické protokoly")</span><span class="sxs-lookup"><span data-stu-id="45658-209">![Diagnostic logs](./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnostic logs")</span></span>

<span data-ttu-id="45658-210">Po dokončení konfigurace nastavení diagnostiky, můžete zobrazit protokoly na **diagnostické protokoly** kartě.</span><span class="sxs-lookup"><span data-stu-id="45658-210">After you configure diagnostic settings, you can view the logs on the **Diagnostic Logs** tab.</span></span>

<span data-ttu-id="45658-211">Další informace o práci s diagnostické protokoly s Azure Data Lake Store najdete v tématu [přístupu k diagnostickým protokolům pro Data Lake Store](data-lake-store-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="45658-211">For more information on working with diagnostic logs with Azure Data Lake Store, see [Access diagnostic logs for Data Lake Store](data-lake-store-diagnostic-logs.md).</span></span>

## <a name="summary"></a><span data-ttu-id="45658-212">Souhrn</span><span class="sxs-lookup"><span data-stu-id="45658-212">Summary</span></span>
<span data-ttu-id="45658-213">Podnikoví zákazníci potřebují cloudové platformy analýzy dat, která jsou v bezpečí a snadno použitelný.</span><span class="sxs-lookup"><span data-stu-id="45658-213">Enterprise customers demand a data analytics cloud platform that is secure and easy to use.</span></span> <span data-ttu-id="45658-214">Azure Data Lake Store je určena k usnadnění adresu, kterou tyto požadavky přes správu identit a ověření pomocí integrace služby Azure Active Directory, ověření na základě seznamu ACL, izolace sítě, šifrování dat při transitu i v rest (k dispozici v budoucnosti ) a auditování.</span><span class="sxs-lookup"><span data-stu-id="45658-214">Azure Data Lake Store is designed to help address these requirements through identity management and authentication via Azure Active Directory integration, ACL-based authorization, network isolation, data encryption in transit and at rest (coming in the future), and auditing.</span></span>

<span data-ttu-id="45658-215">Pokud chcete zobrazit nové funkce v Data Lake Store, pošlete nám svůj názor [Data Lake Store UserVoice fórum](https://feedback.azure.com/forums/327234-data-lake).</span><span class="sxs-lookup"><span data-stu-id="45658-215">If you want to see new features in Data Lake Store, send us your feedback in the [Data Lake Store UserVoice forum](https://feedback.azure.com/forums/327234-data-lake).</span></span>

## <a name="see-also"></a><span data-ttu-id="45658-216">Viz také</span><span class="sxs-lookup"><span data-stu-id="45658-216">See also</span></span>
* [<span data-ttu-id="45658-217">Přehled Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="45658-217">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="45658-218">Začínáme s Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="45658-218">Get started with Data Lake Store</span></span>](data-lake-store-get-started-portal.md)
* [<span data-ttu-id="45658-219">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="45658-219">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
