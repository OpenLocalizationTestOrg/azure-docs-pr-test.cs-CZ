---
title: "Spolehlivé serializace objektu kolekce v Azure Service Fabric | Microsoft Docs"
description: "Serializace objektu Azure Service Fabric spolehlivé kolekce"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: c14794b71ce7340d9e90a56d781c712e247ded06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a><span data-ttu-id="17b23-103">Spolehlivé serializace objektu kolekce v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="17b23-103">Reliable Collection object serialization in Azure Service Fabric</span></span>
<span data-ttu-id="17b23-104">Spolehlivé kolekce replikovat a zachovat jejich položky a ujistěte se, že jsou odolné napříč selhání počítače a výpadky napájení.</span><span class="sxs-lookup"><span data-stu-id="17b23-104">Reliable Collections' replicate and persist their items to make sure they are durable across machine failures and power outages.</span></span>
<span data-ttu-id="17b23-105">Jak chcete replikovat a zachovat položky, k serializaci je nutné spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="17b23-105">Both to replicate and to persist items, Reliable Collections' need to serialize them.</span></span>

<span data-ttu-id="17b23-106">Spolehlivé kolekce získat odpovídající serializátor pro daný typ z spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="17b23-106">Reliable Collections' get the appropriate serializer for a given type from Reliable State Manager.</span></span>
<span data-ttu-id="17b23-107">Správce spolehlivé stavu obsahuje integrovanou serializátorů a umožňuje vlastní serializátorů k registraci pro daného typu.</span><span class="sxs-lookup"><span data-stu-id="17b23-107">Reliable State Manager contains built-in serializers and allows custom serializers to be registered for a given type.</span></span>

## <a name="built-in-serializers"></a><span data-ttu-id="17b23-108">Předdefinované Serializátorů</span><span class="sxs-lookup"><span data-stu-id="17b23-108">Built-in Serializers</span></span>

<span data-ttu-id="17b23-109">Správce spolehlivé stavu zahrnuje integrovanou serializátor pro některé běžné typy tak, aby se Dal serializovat efektivně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="17b23-109">Reliable State Manager includes built-in serializer for some common types, so that they can be serialized efficiently by default.</span></span> <span data-ttu-id="17b23-110">Pro ostatní typy spolehlivé správce stavu se vrátí k použití [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="17b23-110">For other types, Reliable State Manager falls back to use the [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span></span>
<span data-ttu-id="17b23-111">Předdefinované serializátorů jsou efektivnější, protože vědí, nelze změnit jejich typy a není potřeba zahrnovat informace o typu jako její název typu.</span><span class="sxs-lookup"><span data-stu-id="17b23-111">Built-in serializers are more efficient since they know their types cannot change and they do not need to include information about the type like its type name.</span></span>

<span data-ttu-id="17b23-112">Správce spolehlivé stavu má integrovanou serializátor pro následující typy:</span><span class="sxs-lookup"><span data-stu-id="17b23-112">Reliable State Manager has built-in serializer for following types:</span></span> 
- <span data-ttu-id="17b23-113">Identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="17b23-113">Guid</span></span>
- <span data-ttu-id="17b23-114">BOOL</span><span class="sxs-lookup"><span data-stu-id="17b23-114">bool</span></span>
- <span data-ttu-id="17b23-115">Bajtů</span><span class="sxs-lookup"><span data-stu-id="17b23-115">byte</span></span>
- <span data-ttu-id="17b23-116">SByte –</span><span class="sxs-lookup"><span data-stu-id="17b23-116">sbyte</span></span>
- <span data-ttu-id="17b23-117">Byte]</span><span class="sxs-lookup"><span data-stu-id="17b23-117">byte[]</span></span>
- <span data-ttu-id="17b23-118">Char</span><span class="sxs-lookup"><span data-stu-id="17b23-118">char</span></span>
- <span data-ttu-id="17b23-119">Řetězec</span><span class="sxs-lookup"><span data-stu-id="17b23-119">string</span></span>
- <span data-ttu-id="17b23-120">Decimal</span><span class="sxs-lookup"><span data-stu-id="17b23-120">decimal</span></span>
- <span data-ttu-id="17b23-121">Double</span><span class="sxs-lookup"><span data-stu-id="17b23-121">double</span></span>
- <span data-ttu-id="17b23-122">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="17b23-122">float</span></span>
- <span data-ttu-id="17b23-123">celá čísla</span><span class="sxs-lookup"><span data-stu-id="17b23-123">int</span></span>
- <span data-ttu-id="17b23-124">uint</span><span class="sxs-lookup"><span data-stu-id="17b23-124">uint</span></span>
- <span data-ttu-id="17b23-125">dlouhá</span><span class="sxs-lookup"><span data-stu-id="17b23-125">long</span></span>
- <span data-ttu-id="17b23-126">ulong –</span><span class="sxs-lookup"><span data-stu-id="17b23-126">ulong</span></span>
- <span data-ttu-id="17b23-127">krátký</span><span class="sxs-lookup"><span data-stu-id="17b23-127">short</span></span>
- <span data-ttu-id="17b23-128">ushort –</span><span class="sxs-lookup"><span data-stu-id="17b23-128">ushort</span></span>

## <a name="custom-serialization"></a><span data-ttu-id="17b23-129">Vlastní serializace</span><span class="sxs-lookup"><span data-stu-id="17b23-129">Custom Serialization</span></span>

<span data-ttu-id="17b23-130">Vlastní serializátorů se běžně používají, pokud chcete zvýšit výkon, nebo k šifrování dat přenášených v síti a na disku.</span><span class="sxs-lookup"><span data-stu-id="17b23-130">Custom serializers are commonly used to increase performance or to encrypt the data over the wire and on disk.</span></span> <span data-ttu-id="17b23-131">Mezi z jiných důvodů jsou často efektivnější než obecné serializátor vlastní serializátorů, vzhledem k tomu nepotřebují k serializaci informace o typu.</span><span class="sxs-lookup"><span data-stu-id="17b23-131">Among other reasons, custom serializers are commonly more efficient than generic serializer since they don't need to serialize information about the type.</span></span> 

<span data-ttu-id="17b23-132">[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) slouží k registraci vlastní serializátor pro daný typ T. Tato registrace musí dojít při sestavování StatefulServiceBase, před zahájením obnovení, všechny spolehlivé kolekce získali přístup k příslušné serializátor číst jejich trvalá data.</span><span class="sxs-lookup"><span data-stu-id="17b23-132">[IReliableStateManager.TryAddStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) is used to register a custom serializer for the given type T. This registration should happen in the construction of the StatefulServiceBase to ensure that before recovery starts, all Reliable Collections have access to the relevant serializer to read their persisted data.</span></span>

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed to set OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> <span data-ttu-id="17b23-133">Vlastní serializátorů dané priority nad předdefinované serializátorů.</span><span class="sxs-lookup"><span data-stu-id="17b23-133">Custom serializers are given precedence over built-in serializers.</span></span> <span data-ttu-id="17b23-134">Například když je zaregistrovaný vlastní serializátor int, použije se k serializaci celých čísel namísto integrované serializátor pro int.</span><span class="sxs-lookup"><span data-stu-id="17b23-134">For example, when a custom serializer for int is registered, it is used to serialize integers instead of the built-in serializer for int.</span></span>

### <a name="how-to-implement-a-custom-serializer"></a><span data-ttu-id="17b23-135">Jak implementovat vlastní serializátor</span><span class="sxs-lookup"><span data-stu-id="17b23-135">How to implement a custom serializer</span></span>

<span data-ttu-id="17b23-136">Vlastní serializátor musí implementovat [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="17b23-136">A custom serializer needs to implement the [IStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.</span></span>

> [!NOTE]
> <span data-ttu-id="17b23-137">IStateSerializer<T> zahrnuje přetížení pro zápis a čtení, která přebírá další t označuje základní hodnota.</span><span class="sxs-lookup"><span data-stu-id="17b23-137">IStateSerializer<T> includes an overload for Write and Read that takes in an additional T called base value.</span></span> <span data-ttu-id="17b23-138">Toto rozhraní API je pro rozdílové serializace.</span><span class="sxs-lookup"><span data-stu-id="17b23-138">This API is for differential serialization.</span></span> <span data-ttu-id="17b23-139">Aktuálně nebude vystavena, funkce rozdílové serializace.</span><span class="sxs-lookup"><span data-stu-id="17b23-139">Currently differential serialization feature is not exposed.</span></span> <span data-ttu-id="17b23-140">Proto nejsou tyto dvě přetížení volá dokud rozdílové serializace je vystavený a povolený.</span><span class="sxs-lookup"><span data-stu-id="17b23-140">Hence, these two overloads are not called until differential serialization is exposed and enabled.</span></span>

<span data-ttu-id="17b23-141">Následuje příklad vlastního typu, v názvem OrderKey, který obsahuje čtyři vlastnosti</span><span class="sxs-lookup"><span data-stu-id="17b23-141">Following is an example custom type called OrderKey that contains four properties</span></span>

```C#
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

<span data-ttu-id="17b23-142">Následuje příklad implementace IStateSerializer<OrderKey>.</span><span class="sxs-lookup"><span data-stu-id="17b23-142">Following is an example implementation of IStateSerializer<OrderKey>.</span></span>
<span data-ttu-id="17b23-143">Všimněte si, že čtení a zápis přetížení, které provést ve baseValue, jejich příslušné přetížení volání pro předávání kompatibility.</span><span class="sxs-lookup"><span data-stu-id="17b23-143">Note that Read and Write overloads that take in baseValue, call their respective overload for forwards compatibility.</span></span>

```C#
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a><span data-ttu-id="17b23-144">Zdokonalovány</span><span class="sxs-lookup"><span data-stu-id="17b23-144">Upgradability</span></span>
<span data-ttu-id="17b23-145">V [vrácení upgradu aplikace](service-fabric-application-upgrade.md), upgradu se použije pro dílčí sadu uzlů, jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="17b23-145">In a [rolling application upgrade](service-fabric-application-upgrade.md), the upgrade is applied to a subset of nodes, one upgrade domain at a time.</span></span> <span data-ttu-id="17b23-146">Během tohoto procesu několik domén upgradu bude v novější verzi aplikace a některé domén upgradu bude na starší verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="17b23-146">During this process, some upgrade domains will be on the newer version of your application, and some upgrade domains will be on the older version of your application.</span></span> <span data-ttu-id="17b23-147">Při zavedení nová verze aplikace musí být možné číst stará verze vaše data a starší verzi aplikace musí být možné číst novou verzi vaše data.</span><span class="sxs-lookup"><span data-stu-id="17b23-147">During the rollout, the new version of your application must be able to read the old version of your data, and the old version of your application must be able to read the new version of your data.</span></span> <span data-ttu-id="17b23-148">Pokud formát dat není vpřed a zpětně kompatibilní, upgrade může selhat nebo horší, mohou být data ztrátě nebo poškození.</span><span class="sxs-lookup"><span data-stu-id="17b23-148">If the data format is not forward and backward compatible, the upgrade may fail, or worse, data may be lost or corrupted.</span></span>

<span data-ttu-id="17b23-149">Pokud používáte integrované serializátor, nemáte se starat o kompatibilitě.</span><span class="sxs-lookup"><span data-stu-id="17b23-149">If you are using  built-in serializer, you do not have to worry about compatibility.</span></span>
<span data-ttu-id="17b23-150">Ale pokud používáte vlastní serializátor nebo objektu DataContractSerializer, data mají být nekonečnou zpět a vpřed kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="17b23-150">However, if you are using a custom serializer or the DataContractSerializer, the data have to be infinitely backwards and forwards compatible.</span></span>
<span data-ttu-id="17b23-151">Jinými slovy každou verzi serializátor musí být schopen serializovat a deserializovat všechny verze typu.</span><span class="sxs-lookup"><span data-stu-id="17b23-151">In other words, each version of serializer needs to be able to serialize and de-serialize any version of the type.</span></span>

<span data-ttu-id="17b23-152">Uživatelé kontraktu dat postupujte podle pravidla dobře definovaný Správa verzí pro přidání, odebrání a změna pole.</span><span class="sxs-lookup"><span data-stu-id="17b23-152">Data Contract users should follow the well-defined versioning rules for adding, removing, and changing fields.</span></span> <span data-ttu-id="17b23-153">Kontrakt dat má také podpora zabývají neznámé pole, zapojování do procesu serializace a deserializace a plánování práce s dědičnosti tříd.</span><span class="sxs-lookup"><span data-stu-id="17b23-153">Data Contract also has support for dealing with unknown fields, hooking into the serialization and deserialization process, and dealing with class inheritance.</span></span> <span data-ttu-id="17b23-154">Další informace najdete v tématu [pomocí kontrakt dat](https://msdn.microsoft.com/library/ms733127.aspx).</span><span class="sxs-lookup"><span data-stu-id="17b23-154">For more information, see [Using Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).</span></span>

<span data-ttu-id="17b23-155">Vlastní serializátor uživatelé by měl splňovat pokyny serializátoru, který používají zajistěte, aby je zpětné a předává kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="17b23-155">Custom serializer users should adhere to the guidelines of the serializer they are using to make sure it is backwards and forwards compatible.</span></span>
<span data-ttu-id="17b23-156">Běžný způsob podpory všechny verze je přidání informací o velikosti od začátku a jenom přidávání volitelné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="17b23-156">Common way of supporting all versions is adding size information at the beginning and only adding optional properties.</span></span>
<span data-ttu-id="17b23-157">Tímto způsobem jednotlivých verzí může číst mnohem můžete a přejít přes zbývající část datového proudu.</span><span class="sxs-lookup"><span data-stu-id="17b23-157">This way each version can read as much it can and jump over the remaining part of the stream.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17b23-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17b23-158">Next steps</span></span>
  * [<span data-ttu-id="17b23-159">Serializace a upgrade</span><span class="sxs-lookup"><span data-stu-id="17b23-159">Serialization and upgrade</span></span>](service-fabric-application-upgrade-data-serialization.md)
  * [<span data-ttu-id="17b23-160">Referenční informace pro vývojáře pro spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="17b23-160">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * <span data-ttu-id="17b23-161">[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17b23-161">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>
  * <span data-ttu-id="17b23-162">[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="17b23-162">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>
  * <span data-ttu-id="17b23-163">Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="17b23-163">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>
  * <span data-ttu-id="17b23-164">Další informace o použití pokročilých funkcí při upgradu vaší aplikace tím, že odkazuje na [Pokročilá témata](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="17b23-164">Learn how to use advanced functionality while upgrading your application by referring to [Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
  * <span data-ttu-id="17b23-165">Řešení běžných potíží v upgradů aplikací podle kroků v části [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="17b23-165">Fix common problems in application upgrades by referring to the steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
