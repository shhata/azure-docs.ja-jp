---
title: "Azure Migrate のアセスメントの計算 | Microsoft Docs"
description: "Azure Migrate サービスにおけるアセスメントの計算の概要を説明します。"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: b8075f0e1149a6fc5194347fc34e2a16d5eb2ffc
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2018
---
# <a name="assessment-calculations"></a>評価の計算

[Azure Migrate](migrate-overview.md) は、オンプレミスのワークロードを評価することによって、Azure への移行を支援します。 この記事では、アセスメントの計算方法について説明します。



## <a name="overview"></a>概要

Azure Migrate のアセスメントには、3 つの段階があります。 アセスメントは適合性分析から始まり、次にパフォーマンスに基づくサイズ設定の見積もりが行われ、最後に月間コストの見積もりが行われます。 前の段階で合格したマシンだけが次の段階に進みます。 たとえば、マシンが Azure 適合性チェックで不合格になった場合、Azure に不適合とマークされ、サイズとコストは計算されません。 


## <a name="azure-suitability-analysis"></a>Azure 適合性分析

Azure に移行するマシンは、Azure の要件と制限を満たす必要があります。 たとえば、オンプレミスの VM ディスクが 4 TB を超えている場合、Azure でホストすることはできません。 Azure の準拠チェックの概要を次の表に示します。 

**確認事項** | **詳細**
--- | ---
**ブートの種類** | ゲスト OS ディスクのブートの種類は、UEFI ではなく、BIOS である必要があります。
**コア** | マシンのコア数は、Azure VM でサポートされている最大コア数 (32) 以下である必要があります。<br/><br/> パフォーマンス履歴が使用可能な場合、Azure Migrate では、使用されているコアの数が比較で考慮されます。 アセスメント設定で快適性係数が指定されている場合、使用されているコアの数に快適性係数が乗算されます。<br/><br/> パフォーマンス履歴がない場合は、快適性係数を適用せずに、割り当てられているコアの数が使用されます。
**メモリ** | マシンのメモリ サイズは、Azure VM で許容される最大メモリ (448 GB) 以下である必要があります。 <br/><br/> パフォーマンス履歴が使用可能な場合、Azure Migrate では、使用されているメモリが比較で考慮されます。 快適性係数が指定されている場合、使用されているメモリに快適性係数が乗算されます。<br/><br/> 履歴がない場合は、快適性係数を適用せずに、割り当てられているメモリが使用されます。<br/><br/> 
**Windows Server 2003 ～ 2008 R2** | 32 ビットおよび 64 ビットをサポートします。<br/><br/> Azure は最大限のサポートのみを提供します。
**Windows Server 2008 R2 および全 SP** | 64 ビットをサポートします。<br/><br/> Azure は完全サポートを提供します。
**Windows Server 2012 および全 SP** | 64 ビットをサポートします。<br/><br/> Azure は完全サポートを提供します。
**Windows Server 2012 R2 および全 SP** | 64 ビットをサポートします。<br/><br/> Azure は完全サポートを提供します。
**Windows Server 2016 および全 SP** | 64 ビットをサポートします。<br/><br/> Azure は完全サポートを提供します。
**Windows Client 7 以降** | 64 ビットをサポートします。<br/><br/> Azure は、Visual Studio サブスクリプションでのみサポートを提供します。
**Linux** | 64 ビットをサポートします。<br/><br/> Azure はこれらの[オペレーティング システム](../virtual-machines/linux/endorsed-distros.md)を完全にサポートします。
**ストレージ ディスク** | ディスクの割り当てサイズは、4 TB (4,096 GB) 以下である必要があります。<br/><br/> マシンに接続されているディスクの数は、OS ディスクを含めて 65 個以下である必要があります。 
**ネットワーク** | マシンに接続されている NIC の数は、32 個以下である必要があります。


## <a name="performance-based-sizing"></a>パフォーマンス ベースのサイズ設定

マシンが Azure に適合とマークされると、Azure Migrate は次の基準を使用して Azure の VM サイズにマップします。

- **ストレージ チェック**: Azure Migrate は、マシンに接続されているすべてのディスクを Azure のディスクにマップしようとします。
    - Azure Migrate で、1 秒あたりの I/O (IOPS) に快適性係数が乗算されます。 また、各ディスクのスループット (Mbps) にも快適性係数が乗算されます。 これにより、有効なディスク IOPS とスループットが得られます。 これに基づいて、Azure Migrate はディスクを Azureの Standard または Premium ディスクにマップします。
      - 必要な IOPS とスループットのディスクが見つからない場合、そのマシンは Azure に不適合とマークされます。
      - 一連の適切なディスクが見つかった場合は、ストレージ冗長化と、アセスメント設定で指定された場所をサポートするディスクが選択されます。
      - 対象となるディスクが複数ある場合は、最も低コストのディスクが選択されます。
- **ストレージ ディスク スループット**: ディスクおよび VM あたりの Azure の制限の詳細については、[こちら](../azure-subscription-service-limits.md#storage-limits)をご覧ください。
- **ディスクの種類**: Azure Migrate では、管理ディスクのみをサポートします。
- **ネットワーク チェック**: Azure Migrate は、オンプレミスのマシンの NIC の数をサポートできる Azure VM を見つけることを試みます。
    - そのために、マシンのすべての NIC から 1 秒間に送信されたデータ (Mbps) (ネットワーク送信) が集計され、集計結果に快適性係数が適用されます。 この数値は、必要なネットワーク パフォーマンスをサポートできる Azure VM を見つけるために使用されます。
    - Azure Migrate は、VM からネットワーク設定を取得し、データセンターの外部のネットワークと見なします。
    - ネットワーク パフォーマンス データが使用できない場合は、VM のサイズ設定で NIC 数だけが考慮されます。
- **コンピューティング チェック**: ストレージとネットワークの要件が計算されたら、Azure Migrate はコンピューティング要件を検討します。
    - VM でパフォーマンス データが使用可能な場合、使用されているコアの数とメモリが確認され、快適性係数が適用されます。 その数値に基づいて、Azure の適切な VM サイズを見つけることを試みます。
    - 適切なサイズが見つからない場合、そのマシンは Azure に不適合とマークされます。
    - 適切なサイズが見つかった場合は、ストレージとネットワークの計算が適用されます。 その後、場所と価格レベルの設定が適用され、VM サイズの最終的な推奨事項が作成されます。


## <a name="monthly-cost-estimation"></a>月間コスト見積もり

サイズ設定に関する推奨事項が完成すると、Azure Migrate は移行後のコンピューティング コストとストレージ コストを計算します。

- **コンピューティング コスト**: Azure Migrate は、Billing API を使用して、推奨される Azure VM サイズで VM の月間コストを計算します。 この計算では、オペレーティング システム、ソフトウェア アシュアランス、場所、および通貨の設定が考慮されます。 すべてのマシンのコストが集計され、月間コンピューティング コストの合計が計算されます。
- **ストレージ コスト**: マシンの月間ストレージ コストは、マシンに接続されているすべてのディスクの月間コストを集計することによって計算されます。 Azure Migrate は、すべてのマシンのストレージ コストを集計して、月間ストレージ コストの合計を計算します。 現在、この計算では、アセスメント設定で指定されたプランは考慮されません。

コストは、アセスメント設定で指定された通貨で表示されます。 


## <a name="next-steps"></a>次の手順

[オンプレミスの VMware VM のアセスメントを設定する](tutorial-assessment-vmware.md)
