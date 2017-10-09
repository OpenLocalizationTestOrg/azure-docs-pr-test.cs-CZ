---
title: "aaaAzure portál pro zásady prostředků | Microsoft Docs"
description: "Popisuje, jak toouse Azure portálu toocreate a spravovat zásady Resource Manager. Zásady můžete použít na hello předplatné nebo prostředek skupiny."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: ce6413386317e532b63761a24458b85c996af4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-tooassign-and-manage-resource-policies"></a>Pomocí portálu Azure tooassign a spravovat zásady prostředků
Hello portál Azure umožňuje vám tooassign zásady tooresource skupiny prostředků a předplatná. Hello uživatelské rozhraní umožňuje snadno tooselect hello zásady má tooassign a zadejte hodnoty parametrů pro nastavení této zásady hello toocustomize zásad. 

tooassign zásadu prostřednictvím portálu hello definice zásady hello již musí existovat v rámci vašeho předplatného. Vaše předplatné má několik předdefinovaných zásad definice, které jsou připravené pro vás tooassign tooresource skupiny nebo předplatných. Zobrazí tyto předdefinované zásady a jakékoli vlastní zásady, které jste definovali při použití zásady portálu tooassign hello. Toopolicies úvod a jak toodefine vlastní zásady najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).

Zásady jsou zdědí všechny podřízené prostředky. Proto pokud je zásada použitá tooa skupinu prostředků, je použít tooall hello prostředky v příslušné skupině prostředků. V tomto článku hello termín **oboru** odkazuje toohello skupinu prostředků nebo předplatného, které je přiřazen hello zásad. 

Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.

## <a name="assign-a-policy"></a>Přiřadit zásady

1. tooassign tooeither zásad skupiny prostředků nebo předplatného, vyberte tuto skupinu prostředků nebo předplatného. V nastavení hello vyberte **zásady**.

   ![Vyberte zásady](./media/resource-manager-policy-portal/select-policies.png)

2. Vyberte toocreate přiřazení zásady pro tento rozsah **přidat přiřazení**.

   ![Přidat přiřazení](./media/resource-manager-policy-portal/add-assignment.png)

3. Vyberte zásady hello chcete tooassign. I když nepřidali jste žádné zásady definice tooyour předplatné, zobrazí hello integrovaných zásad, které jsou k dispozici pro přiřazení. Tyto integrované zásady zahrnují mnoho běžných scénářů.

   ![Vyberte definice](./media/resource-manager-policy-portal/select-definition.png)

4. Po výběru zásady, zobrazí popis zásad hello a parametry pro tuto zásadu. Například hello následující obrázek ukazuje hello **povolené umístění** parametr, který je vyžadován pro hello zásad, které omezují hello dostupná umístění.

   ![Zobrazit parametry](./media/resource-manager-policy-portal/show-parameters.png)

5. Hello uživatelském rozhraní vyberte hello toospecify hodnoty pro parametry zásad hello (například hello umístění, které lze použít pro nasazení).

   ![Vyberte hodnoty parametru](./media/resource-manager-policy-portal/select-parameters.png)

6. Zadejte hodnoty pro hello další parametry. Hello je automaticky přiřazen obor podle hello okna, se při spuštění přiřazení zásady hello. Až to budete mít, vyberte **OK**.

   ![Definujte parametry](./media/resource-manager-policy-portal/define-parameters.png)

  Přiřadili jste hello zásad toohello zadaný obor.

## <a name="view-policy-assignments"></a>Zobrazení přiřazení zásad.

Po přiřazení zásady, najdete v seznamu hello zásad pro tento obor. Hello **podrobnosti** karta zobrazuje souhrnné údaje o přiřazení zásady hello.

![Zobrazit podrobnosti](./media/resource-manager-policy-portal/show-details.png)

Hello **pravidlo přiřazení** kartě se zobrazují hello JSON definice zásady hello.

![Zobrazit pravidlo přiřazení](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Změnit existující přiřazení zásady

Vyberte zásadu, toochange **upravit přiřazení** nebo **odstranit**

![upravit nebo odstranit přiřazení](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Přiřazení vlastních zásad

Pokud jste definovali vlastní zásady v rámci vašeho předplatného, tyto zásady jsou k dispozici pro přiřazení prostřednictvím portálu hello. Tyto zásady jsou uvedena **[vlastní]**

![vlastní zásady](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Další kroky
* toolearn o hello syntaxe JSON pro definování zásad, najdete v části [přehled zásad prostředků](resource-manager-policy.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).
* schéma zásad Hello je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

