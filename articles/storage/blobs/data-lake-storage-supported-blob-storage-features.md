---
title: Funkce BLOB Storage dostupné v Azure Data Lake Storage Gen2 | Microsoft Docs
description: Zjistěte, které funkce služby Blob Storage můžete použít s Azure Data Lake Storage Gen2
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: c79bca86ba253217012abd24d509d365a076b183
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87086026"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Funkce BLOB Storage dostupné v Azure Data Lake Storage Gen2

Blob Storage funkce, jako je [protokolování diagnostiky](../common/storage-analytics-logging.md), [úrovně přístupu](storage-blob-storage-tiers.md)a [zásady správy životního cyklu BLOB Storage](storage-lifecycle-management-concepts.md) , teď fungují s účty, které mají hierarchický obor názvů. Proto můžete v účtech BLOB Storage povolit hierarchické obory názvů bez ztráty přístupu k těmto funkcím.

V této tabulce jsou uvedené funkce služby Blob Storage, které můžete použít s Azure Data Lake Storage Gen2. Položky, které se zobrazí v těchto tabulkách, se v průběhu času změní, dokud je podpora stále rozbalená. Další informace o konkrétních problémech přidružených ke stavu verze Preview funkce najdete v článku [známé problémy](data-lake-storage-known-issues.md) .

## <a name="supported-blob-storage-features"></a>Podporované funkce Blob Storage

> [!NOTE]
> Úroveň podpory odkazuje jenom na to, jak je funkce podporovaná s Data Lake Storage Gen2. 
>
> [Účty BlockBlobStorage úrovně Premium-Performance](storage-blob-create-account-block-blob.md) pro data Lake Storage Gen2 jsou momentálně ve verzi Public Preview. Úrovně podpory pro tyto typy účtů se zobrazí ve sloupci **BlockBlobStorage (Premium)** .

|Blob Storage funkce |Obecné účely v2 |BlockBlobStorage (Premium) |Související články |
|---------------|-------------------|---|
|Horká vrstva přístupu|Obecná dostupnost|Nepodporováno|[Azure Blob Storage: Horká, studená a archivní úroveň přístupu](storage-blob-storage-tiers.md)|
|Studená vrstva přístupu|Obecná dostupnost|Nepodporováno|[Azure Blob Storage: Horká, studená a archivní úroveň přístupu](storage-blob-storage-tiers.md)|
|Události|Obecná dostupnost|Preview|[Reakce na události služby Blob Storage](storage-blob-event-overview.md)|
|Metriky (klasické)|Obecná dostupnost|Nepodporováno|[Metriky Azure Storage Analytics (klasické)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Metriky na platformě Azure Monitor|Obecná dostupnost|Preview|[Metriky Azure Storage ve službě Azure Monitor](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Příkazy prostředí PowerShell pro úložiště objektů BLOB|Obecná dostupnost|Preview|[Rychlý Start: nahrání, stažení a výpis objektů BLOB pomocí PowerShellu](storage-quickstart-blobs-powershell.md)|
|Příkazy Azure CLI služby Blob Storage|Obecná dostupnost|Preview|[Rychlý Start: vytvoření, stažení a výpis objektů BLOB pomocí Azure CLI](storage-quickstart-blobs-cli.md)|
|Rozhraní API pro úložiště objektů BLOB|Obecná dostupnost|Preview|[Rychlý start: Klientská knihovna úložiště objektů blob Azure verze 12 pro .NET](storage-quickstart-blobs-dotnet.md)<br>[Rychlý Start: Správa objektů BLOB pomocí sady Java V12 SDK](storage-quickstart-blobs-java.md)<br>[Rychlý Start: Správa objektů BLOB pomocí sady Python V12 SDK](storage-quickstart-blobs-python.md)<br>[Rychlý Start: Správa objektů BLOB pomocí sady JavaScript V12 SDK v Node.js](storage-quickstart-blobs-nodejs.md)|
|Diagnostické protokoly|Obecná dostupnost|Preview <div role="complementary" aria-labelledby="diagnostic-logging"><sup>1</sup></div> |[Protokolování Analýzy úložiště Azure](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Archivní úroveň přístupu|Obecná dostupnost|Nepodporováno|[Azure Blob Storage: Horká, studená a archivní úroveň přístupu](storage-blob-storage-tiers.md)|
|Přihlašování Azure Monitor|Preview |Zatím nepodporováno|[Azure Storage monitorování](../common/monitor-storage.md)|
|Snímky|Preview<div role="complementary" aria-labelledby="preview-form"><sup>3</sup></div>|Zatím nepodporováno|[Snímky objektů BLOB](snapshots-overview.md)|
|Statické weby|Preview<div role="complementary" aria-labelledby="preview-form"><sup>3</sup></div>|Zatím nepodporováno|[Hostování statického webu ve službě Azure Storage](storage-blob-static-website.md)|
|Neměnné úložiště|Preview<div role="complementary" aria-labelledby="preview-form"><sup>3</sup></div>|Zatím nepodporováno|[Ukládání důležitých podnikových dat objektů BLOB s neměnném úložištěm](storage-blob-immutable-storage.md)|
|Zásady správy životního cyklu|Preview|Zatím nepodporováno|[Správa životního cyklu úložiště objektů blob v Azure](storage-lifecycle-management-concepts.md)|
|Blobfuse|Preview|Zatím nepodporováno|[Postup připojení úložiště objektů BLOB jako systému souborů pomocí blobfuse](storage-how-to-mount-container-linux.md)|
|Změna kanálu|Zatím nepodporováno|Zatím nepodporováno|[Změna podpory kanálu ve službě Azure Blob Storage](storage-blob-change-feed.md)|
|Převzetí služeb při selhání účtu|Zatím nepodporováno|Zatím nepodporováno|[Zotavení po havárii a převzetí služeb při selhání účtu](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Seznam ACL kontejneru objektů BLOB|Zatím nepodporováno<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|Zatím nepodporováno<div role="complementary" aria-labelledby="blob-container-ACL"><sup>2</sup></div>|[Nastavení seznamu ACL kontejneru](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Vlastní domény|Zatím nepodporováno|Zatím nepodporováno|[Mapování vlastní domény na koncový bod služby Azure Blob Storage](storage-custom-domain-name.md)|
|Obnovitelné odstranění|Zatím nepodporováno|Zatím nepodporováno|[Obnovitelné odstranění objektů blob služby Azure Storage](storage-blob-soft-delete.md)|

<div id="diagnostic-logging"><sup>1</sup> pro účty úložiště blob bloku úrovně Premium se diagnostické protokoly (Classic) nedají povolit pomocí Azure Portal. Povolte je pomocí PowerShellu.</div><br>

<div id="blob-container-ACL"><sup>2</sup> seznamy ACL můžete nastavit v kořenové složce kontejneru, ale ne v samotném kontejneru.</div><br>

<div id="preview-form"><sup>3</sup> . Pokud chcete používat snímky, neměnné úložiště nebo statické weby s Data Lake Storage Gen2, musíte se zaregistrovat ve verzi Preview, a to tak, že tento <a href=https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VUOUc3NTNQSUdOTjgzVUlVT1pDTzU4WlRKRy4u>formulář</a>dokončíte.  </div>

## <a name="see-also"></a>Viz také

- [Známé problémy s Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Služby Azure, které podporují Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md)
- [Open Source platformy, které podporují Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md)
- [Přístup k více protokolům na Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)