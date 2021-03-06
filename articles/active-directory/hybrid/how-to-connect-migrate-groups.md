---
title: 'Azure AD Connect: Migrace skupin z jedné doménové struktury do druhé'
description: Tento článek popisuje kroky potřebné k úspěšné migraci skupin z jedné doménové struktury do druhé pro Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 04/02/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ef693a48dc52854e4e1fd8359ef24f65ce236f7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85358578"
---
# <a name="migrate-groups-from-one-forest-to-another-for-azure-ad-connect"></a>Migrace skupin z jedné doménové struktury na jinou pro Azure AD Connect

Tento článek popisuje, jak migrovat skupiny z jedné doménové struktury do druhé, aby migrované objekty skupiny odpovídaly existujícím objektům v cloudu.

## <a name="prerequisites"></a>Požadavky

- Azure AD Connect verze 1.5.18.0 nebo novější
- Atribut zdrojového ukotvení nastavený na`mS-DS-ConsistencyGuid`

## <a name="migrate-groups"></a>Migrace skupin

Počínaje verzí 1.5.18.0 Azure AD Connect podporuje použití `mS-DS-ConsistencyGuid` atributu pro skupiny. Pokud se rozhodnete `mS-DS-ConsistencyGuid` jako atribut zdrojového ukotvení a hodnota se naplní ve službě Active Directory, Azure AD Connect použije hodnotu `mS-DS-ConsistencyGuid` jako `immutableId` . V opačném případě se vrátí k použití `objectGUID` . Všimněte si ale, že Azure AD Connect nezapisuje hodnotu zpět do `mS-DS-ConsistencyGuid` atributu ve službě Active Directory.

Při přesunu mezi doménovými strukturami se při přesunu objektu skupiny z jedné doménové struktury (řekněte F1) do jiné doménové struktury (řekněme F2) musí zkopírovat buď `mS-DS-ConsistencyGuid` hodnotu (Pokud je přítomná), nebo `objectGUID` hodnotu z objektu v doménové struktuře F1 na `mS-DS-ConsistencyGuid` atribut objektu ve F2.

Pomocí následujících skriptů se naučíte migrovat jednu skupinu z jedné doménové struktury do druhé. Tyto skripty můžete použít také jako vodítko pro migraci více skupin. Skripty používají název doménové struktury F1 pro zdrojovou doménovou strukturu a F2 pro cílovou doménovou strukturu.

Nejprve získáme `objectGUID` a `mS-DS-ConsistencyGuid` objekt skupiny v doménové struktuře F1. Tyto atributy jsou exportovány do souboru CSV.
```
<#
DESCRIPTION
============
This script will take DN of a group as input.
It then copies the objectGUID and mS-DS-ConsistencyGuid values along with other attributes of the given group to a CSV file.

This CSV file can then be used as input to the Export-Group script.
#>
Param(
       [ValidateNotNullOrEmpty()]
       [string]
       $dn,

       [ValidateNotNullOrEmpty()]
       [string]
       $outputCsv
)

$defaultProperties = @('samAccountName', 'distinguishedName', 'objectGUID', 'mS-DS-ConsistencyGuid')
$group  = Get-ADGroup -Filter "DistinguishedName -eq '$dn'" -Properties $defaultProperties -ErrorAction Stop
$results = @()
if ($group -eq $null)
{
       Write-Error "Group not found"
}
else
{
       $objectGUIDValue = [GUID]$group.'objectGUID'
       $mSDSConsistencyGuidValue = "N/A"
       if ($group.'mS-DS-ConsistencyGuid' -ne $null)
       {
              $mSDSConsistencyGuidValue = [GUID]$group.'mS-DS-ConsistencyGuid'
       }
       $adgroup = New-Object -TypeName PSObject
       $adgroup | Add-Member -MemberType NoteProperty -Name samAccountName -Value $($group.'samAccountName')
       $adgroup | Add-Member -MemberType NoteProperty -Name distinguishedName -Value $($group.'distinguishedName')
       $adgroup | Add-Member -MemberType NoteProperty -Name objectGUID -Value $($objectGUIDValue)
       $adgroup | Add-Member -MemberType NoteProperty -Name mS-DS-ConsistencyGuid -Value $($mSDSConsistencyGuidValue)
       $results += $adgroup
}

Write-Host "Exporting group to output file"
$results | Export-Csv "$outputCsv" -NoTypeInformation

```

V dalším kroku použijeme vygenerovaný výstupní soubor CSV k razítku `mS-DS-ConsistencyGuid` atributu cílového objektu v doménové struktuře F2:


```
<#
DESCRIPTION
============
This script will take DN of a group as input and the CSV file that was generated by the Import-Group script.
It copies either the objectGUID or the mS-DS-ConsistencyGuid value from the CSV file to the given object.

#>
Param(
       [ValidateNotNullOrEmpty()]
       [string]
       $dn,

       [ValidateNotNullOrEmpty()]
       [string]
       $inputCsv
)

$group  = Get-ADGroup -Filter "DistinguishedName -eq '$dn'" -ErrorAction Stop
if ($group -eq $null)
{
       Write-Error "Group not found"
}

$csvFile = Import-Csv -Path $inputCsv -ErrorAction Stop
$msDSConsistencyGuid = $csvFile.'mS-DS-ConsistencyGuid'
$objectGuid = [GUID] $csvFile.'objectGUID'
$targetGuid = $msDSConsistencyGuid

if ($msDSConsistencyGuid -eq "N/A")
{
       $targetGuid = $objectGuid
}

Set-ADGroup -Identity $dn -Replace @{'mS-DS-ConsistencyGuid'=$targetGuid} -ErrorAction Stop

```

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [integraci místních identit s Azure Active Directory](whatis-hybrid-identity.md).
