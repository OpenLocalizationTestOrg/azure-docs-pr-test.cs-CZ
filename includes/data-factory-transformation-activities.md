Azure Data Factory podporuje hello následující aktivit transformace, které může být přidané toopipelines buď jednotlivě nebo zřetězené s jinou aktivitou.

| Aktivita transformace dat | Výpočetní prostředí |
|:--- |:--- |
| [Hive](../articles/data-factory/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](../articles/data-factory/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](../articles/data-factory/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Streamování Hadoop](../articles/data-factory/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/data-factory-spark.md) | HDInsight [Hadoop] |
| [Aktivity Machine Learning: Dávkové spouštění a Aktualizace prostředku](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) |Virtuální počítač Azure |
| [Uložená procedura](../articles/data-factory/data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse nebo SQL Server |
| [U-SQL Data Lake Analytics](../articles/data-factory/data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](../articles/data-factory/data-factory-use-custom-activities.md) |HDInsight [Hadoop] nebo Azure Batch |

> [!NOTE]
> MapReduce aktivity toorun Spark programy můžete použít v clusteru HDInsight Spark. Podrobnosti najdete v článku [Vyvolání programů Spark ze služby Azure Data Factory](../articles/data-factory/data-factory-spark.md).
> Můžete vytvořit vlastní aktivity skriptů toorun R na clusteru HDInsight s R nainstalované. Viz [Spuštění skriptu jazyka R pomocí služby Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).
> 
> 

