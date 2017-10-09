Objekt pro vytváření dat je víceklientské služby, který má hello následující výchozí omezení v místě toomake se, že předplatná zákazníka jsou chráněny z druhé strany úlohy. Řadu hello omezení lze snadno zvýšit pro vaše předplatné až maximální limit toohello kontaktováním podpory.

| **Prostředek** | **Výchozí omezení** | **Maximální omezení** |
| --- | --- | --- |
| datové továrny v předplatné Azure |50 |[Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| kanály v rámci objekt pro vytváření dat |2500 |[Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| datové sady v rámci objekt pro vytváření dat |5000 |[Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| souběžné řezy za datové sady |10 |10 |
| počet bajtů za objektu pro objekty kanálu <sup>1</sup> |200 KB |200 KB |
| počet bajtů za pro datovou sadu a propojené služby objekty <sup>1</sup> |100 KB |2000 KB |
| HDInsight na vyžádání clusteru jader v rámci předplatného <sup>2</sup> |60 |[Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Jednotka přesunu dat cloudové <sup>3</sup> |32 |[Kontaktování podpory](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Pro běh aktivit kanálu počet opakování |1000 |MaxInt (32 bitů) |

<sup>1</sup> kanálu, datové sady a propojené služby objekty představují logické seskupení vašich úloh. Limity pro tyto objekty se netýkají tooamount dat můžete přesunout a zpracovat hello služby Azure Data Factory. Objekt pro vytváření dat je navrženou tooscale toohandle petabajty dat.

<sup>2</sup> jader na vyžádání HDInsight jsou přiděleny mimo hello předplatné, které obsahuje objekt pro vytváření dat hello. V důsledku toho hello přesahuje omezení je hello Data Factory vynucené základní limit pro počet jader na vyžádání HDInsight a se liší od hello základní omezení spojená s předplatným Azure.

<sup>3</sup> jednotky přesun dat cloudu (DMU) se používá v rámci cloudu cloudové kopírování operace. Se jedná o míru, která reprezentuje hello výkon (kombinaci procesoru, paměti a přidělení prostředků sítě) v objektu pro vytváření dat na jednu jednotku. S využitím více DMUs v některých případech můžete dosáhnout vyšší propustnost kopírování. Odkazovat příliš[jednotky přesun dat v cloudu](../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) části na podrobnosti.

| **Prostředek** | **Výchozí limit nižší** | **Minimální omezení** |
| --- | --- | --- |
| Plánování intervalu |15 minut |15 minut |
| Interval mezi pokusy o opakování |1 sekunda |1 sekunda |
| Opakujte hodnotu časového limitu |1 sekunda |1 sekunda |

### <a name="web-service-call-limits"></a>Omezení volání webové služby
Azure Resource Manager má omezení pro volání rozhraní API. Můžete provádět volání rozhraní API s rychlostí v rámci hello [rozhraní API Správce prostředků Azure omezuje](../articles/azure-subscription-service-limits.md#resource-group-limits).
