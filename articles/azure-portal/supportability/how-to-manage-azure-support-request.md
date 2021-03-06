---
title: Správa žádosti o podporu Azure
description: Popisuje, jak zobrazit žádosti o podporu, odesílat zprávy, měnit úroveň závažnosti žádosti, sdílet diagnostické informace s podporou Azure, znovu otevřít uzavřenou žádost o podporu a nahrát soubory.
author: mgblythe
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.service: azure-supportability
ms.topic: how-to
ms.date: 06/30/2020
ms.author: mblythe
ms.openlocfilehash: f3b4806bf46750d74a54f68bd2ab58e402e75091
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852295"
---
# <a name="manage-an-azure-support-request"></a>Správa žádosti o podporu Azure

Po [Vytvoření žádosti o podporu Azure](how-to-create-azure-support-request.md)ji můžete spravovat v [Azure Portal](https://portal.azure.com), který je popsaný v tomto článku. Žádosti můžete také vytvářet a spravovat programově pomocí [lístku podpory Azure REST API](/rest/api/support).

## <a name="view-support-requests"></a>Zobrazení žádostí o podporu

Podrobnosti a stav žádostí o podporu si můžete zobrazit tak, že přejdete na **help + podpora**  >   **všech žádostí o podporu**.

:::image type="content" source="media/how-to-manage-azure-support-request/all-requests-lower.png" alt-text="Všechny žádosti o podporu":::

Na této stránce můžete vyhledat, filtrovat a seřadit žádosti o podporu. Vyberte žádost o podporu pro zobrazení podrobností, včetně její závažnosti a všech zpráv přidružených k žádosti.

## <a name="send-a-message"></a>Odeslat zprávu

1. Na stránce **všechny žádosti o podporu** vyberte žádost o podporu.

1. Na stránce **žádost o podporu** vyberte možnost **Nová zpráva**.

1. Zadejte zprávu a vyberte **Odeslat**.

## <a name="change-the-severity-level"></a>Změna úrovně závažnosti

> [!NOTE]
> Maximální úroveň závažnosti závisí na [plánu podpory](https://azure.microsoft.com/support/plans).
>

1. Na stránce **všechny žádosti o podporu** vyberte žádost o podporu.

1. Na stránce **žádost o podporu** vyberte **změnit**.

    :::image type="content" source="media/how-to-manage-azure-support-request/change-severity.png" alt-text="Změna závažnosti žádosti o podporu":::

1. Azure Portal zobrazuje jednu ze dvou obrazovek v závislosti na tom, jestli je vaše žádost už přiřazená pracovníkovi podpory:

    - Pokud se vaše žádost nepřiřadila, zobrazí se obrazovka podobná této. Vyberte novou úroveň závažnosti a pak vyberte **změnit**.

        :::image type="content" source="media/how-to-manage-azure-support-request/unassigned-can-change-severity.png" alt-text="Vybrat novou úroveň závažnosti":::

    - Pokud se vaše žádost přiřadila, zobrazí se obrazovka podobná této. Vyberte **OK**a pak vytvořte [novou zprávu](#send-a-message) pro vyžádání změny úrovně závažnosti.

        :::image type="content" source="media/how-to-manage-azure-support-request/assigned-cant-change-severity.png" alt-text="Nejde vybrat novou úroveň závažnosti.":::

## <a name="share-diagnostic-information-with-azure-support"></a>Sdílet diagnostické informace s podporou Azure

Když vytvoříte žádost o podporu, ve výchozím nastavení je vybraná možnost **Sdílet diagnostické informace** . Díky tomu může podpora Azure shromažďovat [diagnostické informace](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) z vašich prostředků Azure:

* Po vytvoření žádosti tuto možnost nemůžete vymazat.

* Pokud jste při vytváření žádosti tuto možnost vymazali, můžete ji po vytvoření žádosti vybrat.

    1. Na stránce **všechny žádosti o podporu** vyberte žádost o podporu.
    
    1. Na stránce **žádost o podporu** vyberte **udělit oprávnění**a pak vyberte **Ano** a **OK**.
    
        :::image type="content" source="media/how-to-manage-azure-support-request/grant-permission-manage.png" alt-text="Udělení oprávnění pro diagnostické informace":::

## <a name="upload-files"></a>Nahrání souborů

Možnost nahrávání souborů můžete použít k nahrání diagnostických souborů nebo jakýchkoli dalších souborů, které považujete za relevantní pro žádost o podporu.

1. Na stránce **všechny žádosti o podporu** vyberte žádost o podporu.

1. Na stránce **žádost o podporu** vyhledejte soubor a pak vyberte **Odeslat**. Pokud máte více souborů, opakujte tento postup.

    :::image type="content" source="media/how-to-manage-azure-support-request/file-upload.png" alt-text="Nahrání souboru":::

### <a name="file-upload-guidelines"></a>Pokyny pro nahrávání souborů

Při použití možnosti nahrávání souborů postupujte podle těchto pokynů:

* K ochraně vašich osobních údajů nezahrnujte do nahrávání žádné osobní údaje.
* Název souboru nesmí být delší než 110 znaků.
* Nemůžete nahrát víc než jeden soubor.
* Soubory nemohou být větší než 4 MB.
* Všechny soubory musí mít příponu názvu souboru, například *. docx* nebo *. xlsx*. Následující tabulka uvádí přípony názvů souborů, které jsou povoleny pro nahrání.

| 0-9, A-C    | D – G   | H-M         | N-P   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7z         | . dat  | .hwl        | . ODX  | . rar     | .tdb       | .xlam   |
| . a          | . DB   | . ICS        | . oft  | . RDL     | .tdf       | .xlr    |
| . ABC        | . DMP  | . ini        | . old  | . rdlc    | . text      | .xls    |
| . adm        | . do_  | .java       | . One  | . re_     | .thmx      | .xlsb   |
| . aspx       | .doc  | .jpg        | . OSD  | . reg     | .tif       | .xlsm   |
| . ATF        | .docm | . LDF        | . MIMO  | . Remove  | . trc       | .xlsx   |
| . b          | .docx | . hlavičkový formát | . P1   | . Ren     | . TTD       | .xlt    |
| . ba_        | .dotm | . lnk        | .pcap | . rename  | . tx_       | .xltx   |
| . bak        | .dotx | . lo_        | soubor. pdb  | .rft     | .txt       | .xml    |
| .bat        | .dtsx | . log        | .pdf  | . rpt     | .uccapilog | . XMLA   |
| . blg        | . EDS  | .lpk        | .piz  | .rte     | .uccplog   | .xps    |
| . CA_        | . EMF  | . manifest   | .pmls | .rtf     | .udcx      | . xsd    |
| . SOUBORŮ        | . eml  | . Master     | .png  | . Run     | . vb_       | . xsn    |
| . Cap        | .emz  | . mdmp       | .potx | .saz     | . vbs_      | . xxx    |
| .catx       | . err  | . mof        | .ppt  | SQL     | . vcf       | . z_     |
| . Priorita        | . ETL  | . mp3        | .pptm | .sqlplan | . vsd       | .z01    |
| . komprimovaná | . evt  | . mpg        | .pptx | . STP     | . wdb       | .z02    |
| . Konfigurace     | .evtx | . ms_        | . prn  | .svclog  | . WKS       | . Zi     |
| .cpk        | . DODATEČNÉ   | . msg        | . PSV  |   -       | . WMA       | . zi_    |
| . cpp        | . ex_  | .msi        | PST  |  -        | . wmv       | .zip    |
| .cs         | .ex0  | soubor. mso        | . pub  | -         | . WMZ       | . zip_   |
| . Formát        | . FRD  | . msu        | -      |-          | . WPS       | .zipp   |
| .cvr        | .gif  | . nfo        | -      |-          | .wpt       | . zip |
| -            | identifikátor. GUID | -            | -      | -         | . WSDL      | .zippy  |
| -            | . gz   | -            | -      | -         | . wsp       | .zipx   |
| -            | -      | -            | -      | -         | .wtl       | .zit    |
| -            | -      | -            | -      | -         |     -       | .zix    |
| -            | -      | -            | -      | -         |  -          | . zzz    |

## <a name="reopen-a-closed-request"></a>Znovu otevřít uzavřenou žádost

Pokud potřebujete znovu otevřít uzavřenou žádost o podporu, vytvořte [novou zprávu](#send-a-message), která automaticky znovu otevře požadavek.

## <a name="next-steps"></a>Další kroky

[Jak vytvořit žádost o podporu Azure](how-to-create-azure-support-request.md)

[Rozhraní REST API lístku podpory Azure](/rest/api/support)
