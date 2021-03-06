---
title: "ターゲットを準備する (物理サーバーから Azure) | Microsoft Docs"
description: "この記事では、Windows または Linux を実行している物理サーバーを Azure にレプリケートする前に、Azure 環境を準備する方法について説明します。"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 11/23/2017
ms.author: bsiva
ms.openlocfilehash: 2c5377f7193f8357a7e99ed1ef1a61b066b8ce5f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/21/2017
---
# <a name="prepare-target-vmware-to-azure"></a>ターゲットを準備する (VMware から Azure)
> [!div class="op_single_selector"]
> * [VMware から Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [物理から Azure](./site-recovery-prepare-target-physical-to-azure.md)

この記事では、Windows または Linux を実行している物理サーバー (x64)を Azure にレプリケートする前に、Azure 環境を準備する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事は以下を前提としています。
- 物理サーバーを保護するための Recovery Services コンテナーを作成してあること。 Recovery Services コンテナーは [Azure Portal](http://portal.azure.com "Azure Portal") から作成できます。
- 物理サーバーを Azure にレプリケートできるように[オンプレミス環境をセットアップ](./site-recovery-set-up-physical-to-azure.md)してあること。

## <a name="prepare-target"></a>ターゲットを準備する

**ステップ 1: 保護の目標の選択**と**ステップ 2: ソースの準備**を完了した後は、**ステップ 3: ターゲットの準備**を行います。

![ターゲットを準備する](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **サブスクリプション:** ドロップダウン メニューから、物理サーバーのレプリケート先のサブスクリプションを選びます。
2. **デプロイメント モデル:** デプロイメント モデルを選びます (クラシックまたは Resource Manager)。

選んだデプロイメント モデルに基づいて検証が実行され、物理サーバーのレプリケートおよびフェールオーバー先のターゲット サブスクリプションに、互換性のあるストレージ アカウントと仮想ネットワークが少なくとも 1 つはあることが確認されます。

検証が正常に完了した後、[OK] をクリックして次の手順に進みます。

互換性のある Resource Manager ストレージ アカウントまたは仮想ネットワークがない場合は、ページの上部にある **[+ ストレージ アカウント]** または **[+ ネットワーク]** ボタンをクリックして作成できます。

## <a name="next-steps"></a>次のステップ
[レプリケーションの設定を構成する](./site-recovery-setup-replication-settings-vmware.md)。
