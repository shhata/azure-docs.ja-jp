---
title: "Azure での Linux VM の可用性管理 | Microsoft Docs"
description: "複数の仮想マシンを使って Azure の Linux アプリケーションの高い可用性を確保する方法について説明します。"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 891c852a-84c0-4940-a61e-ada6e185bf37
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac83373c1c8a8077a235be2a7b5f5df9353add45
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/08/2017
---
# <a name="manage-the-availability-of-linux-virtual-machines"></a>Linux 仮想マシンの可用性管理

複数の仮想マシンを設定および管理して、Azure の Linux アプリケーションの高い可用性を確保する方法について説明します。 [Windows 仮想マシンの可用性を管理する](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)こともできます。

Resource Manager デプロイメント モデルで CLI を使用して可用性セットを作成する方法の詳細については、「[Azure 可用性セット: 可用性セットを管理するコマンド](../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets)」を参照してください。

[!INCLUDE [virtual-machines-common-manage-availability](../../../includes/virtual-machines-common-manage-availability.md)]

## <a name="next-steps"></a>次のステップ
仮想マシンの負荷分散の詳細については、 [仮想マシンの負荷分散](../virtual-machines-linux-load-balance.md)に関するページをご覧ください。

