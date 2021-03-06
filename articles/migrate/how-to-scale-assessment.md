---
title: "Azure Migrate での大規模な検出と評価 | Microsoft Docs"
description: "Azure Migrate サービスを使用して、多数のオンプレミス マシンを評価する方法について説明します。"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 67661e03e65cde3ec2f1aafd5ef755899cf0c77b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>大規模な VMware 環境の検出と評価

この記事では、[Azure Migrate](migrate-overview.md) を使用して、多数のオンプレミスの仮想マシン (VM) を評価する方法について説明します。 Azure Migrate は、コンピューターが Azure への移行に適しているかどうかを確認します。 このサービスで、Azure でコンピューターを実行する場合の規模とコストの見積もりがわかります。

## <a name="prerequisites"></a>前提条件

- **VMware**: 移行する予定の VM は、vCenter Server バージョン 5.5、6.0、または 6.5 で管理する必要があります。 また、コレクター VM を展開するには、バージョン 5.0 以降を実行している 1 つの ESXi ホストが必要です。
- **vCenter アカウント**: vCenter Server にアクセスするには、読み取り専用アカウントが必要です。 Azure Migrate はこのアカウントを使ってオンプレミスの VM を検出します。
- **アクセス許可**: vCenter Server で、.OVA 形式でファイルをインポートして VM を作成するためのアクセス許可が必要です。
- **統計情報の設定**: デプロイを始める前に、vCenter Server の統計設定をレベル 3 に設定する必要があります。 レベルが 3 未満の場合、評価は機能しますが、ストレージとネットワークのパフォーマンス データが収集されません。 この場合のサイズは、CPU とメモリのパフォーマンス データと、ディスクおよびネットワーク アダプターの構成データに基づいて勧められます。

## <a name="plan-azure-migrate-projects"></a>Azure Migrate プロジェクトの計画

以下の制限に基づいて、検出と評価を計画します。

| **エンティティ** | **マシンの制限** |
| ---------- | ----------------- |
| Project    | 1,500              | 
| 探索  | 1,000              |
| 評価 | 400               |

- 検出および評価するマシンが 400 台未満の場合、必要になるのは 1 つのプロジェクトと 1 つの検出です。 要件に応じて、1 つの評価ですべてのマシンを評価するか、複数の評価にマシンを分けることができます。 
- 400 台から 1,000 台のマシンを検出するには、1 つのプロジェクトと 1 つの検出が必要です。 ただし、1 つの評価で保持できるのは最大 400 台のマシンなので、これらのマシンを評価するには複数の評価が必要です。
- マシンの数が 1,001 台から 1,500 台であれば、1 つのプロジェクトと、そのプロジェクト内での 2 つの検出が必要になります。
- マシンの数が 1,500 台を超える場合は、要件に従って複数のプロジェクトを作成し、複数の検出を実行する必要があります。 例: 
    - マシンの数が 3,000 台の場合、2 つのプロジェクトでそれぞれ 2 つの検出をセットアップするか、3 つのプロジェクトで 1 つの検出をセットアップするかを選択できます。
    - 5,000 台のマシンがある場合は、2 台で 1,500 台のマシンを検出し、もう 1 台で 500 台のマシンを検出することで、4 つのプロジェクトを設定できます。 あるいは、5 つのプロジェクトそれぞれに検出を 1 つセットアップすることもできます。 

## <a name="plan-multiple-discoveries"></a>複数の検出を計画する

同じ Azure Migrate コレクターを使用して、1 つ以上のプロジェクトに対して複数の検出を行うことができます。 このような計画の場合は、以下の事項を考慮してください。
 
- Azure Migrate コレクターを使用して検出を行う場合、検出範囲を vCenter Server フォルダー、データセンター、クラスター、またはホストに設定できます。
- 2 つ以上の検出を行うには、検出を行う VM が、マシンの数が 1000 台という制限に対応しているフォルダー、データセンター、クラスター、またはホスト内にあることを、vCenter Server で確認します。
- 評価を目的としている場合は、複数のマシンを同じプロジェクトと同じ評価内にまとめ、相互に依存関係にある状態にすることをお勧めします。 そして vCenter Server で、依存関係にあるマシンが評価のために同じフォルダー、データセンター、またはクラスター内にあることを確認してください。


## <a name="create-a-project"></a>プロジェクトの作成

要件に従って Azure Migrate プロジェクトを作成します。

1. Azure Portal で、**[リソースの作成]** を選択します。
2. 「**Azure Migrate**」を検索し、検索結果でサービス **Azure Migrate (プレビュー)** を選択します。 **[作成]** を選択します。
3. プロジェクト名およびプロジェクトの Azure サブスクリプションを指定します。
4. 新しいリソース グループを作成します。
5. プロジェクトを作成する場所を指定し、**[作成]** を選択します。 別のターゲット場所の場合でも VM を評価することができます。 プロジェクト用に指定された場所は、オンプレミスの VM から収集されたメタデータを格納するために使用します。

## <a name="set-up-the-collector-appliance"></a>コレクター アプライアンスの設定

Azure Migrate は、コレクター アプライアンスと呼ばれるオンプレミスの VM を作成します。 この VM はオンプレミスの VMware VM を検出し、それについてのメタデータを Azure Migrate サービスに送信します。 コレクター アプライアンスをセットアップするには、.OVA ファイルをダウンロードしてオンプレミスの vCenter Server インスタンスにインポートします。

### <a name="download-the-collector-appliance"></a>コレクター アプライアンスをダウンロードする

複数のプロジェクトがある場合でも、コレクター アプライアンスをダウンロードして vCenter Server にインポートするのは一度だけでかまいません。 アプライアンスをダウンロードしてセットアップした後、それを各プロジェクトで実行し、一意のプロジェクト ID とキーを指定します。

1. Azure Migrate プロジェクトで、**[開始]** > **[検出と評価]** > **[マシンの検出]** の順に選択します。
2. **[マシンの検出]** で **[ダウンロード]** を選択し、OVA ファイルをダウンロードします。
3. **[プロジェクトの資格情報をコピーします]** で、プロジェクトの ID とキーをコピーします。 これらの情報はコレクターを構成するときに必要になります。

   
### <a name="verify-the-collector-appliance"></a>コレクター アプライアンスを確認する

OVA ファイルをデプロイする前に、そのファイルが安全であることを確認します。

1. ファイルをダウンロードしたマシンで、管理者用のコマンド ウィンドウを開きます。
2. 次のコマンドを実行して、OVA のハッシュを生成します。

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   使用例: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. 生成されたハッシュが次の設定と一致することを確認します。
 
    OVA バージョン 1.0.8.49 の場合

    **アルゴリズム** | **ハッシュ値**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    OVA バージョン 1.0.8.40 の場合:

    **アルゴリズム** | **ハッシュ値**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>コレクター VM を作成する

ダウンロードしたファイルを vCenter Server にインポートします。

1. vSphere Client コンソールで、**[File]\(ファイル\)** > **[Deploy OVF Template]\(OVF テンプレートのデプロイ\)** の順に選択します。

    ![OVF をデプロイする](./media/how-to-scale-assessment/vcenter-wizard.png)

2. [Deploy OVF Template]\(OVF テンプレートのデプロイ\) ウィザードで **[Source]\(ソース\)** を選択し、OVA ファイルの場所を指定します。
3. **[Name]\(名前\)** と **[Location]\(場所\)** で、コレクター VM のフレンドリ名と、VM がホストされるインベントリ オブジェクトを指定します。
5. **[Host/Cluster]\(ホスト/クラスター\)** で、コレクター VM が実行するホストまたはクラスターを指定します。
7. ストレージで、コレクター VM の保存先を指定します。
8. **[Disk Format]\(ディスク フォーマット\)** で、ディスクの種類とサイズを指定します。
9. **[Network Mapping]\(ネットワーク マッピング\)** で、コレクター VM の接続先となるネットワークを指定します。 このネットワークには、Azure にメタデータを送信するためのインターネット接続が必要です。 
10. 設定を確認し、**[Finish]\(完了\)** を選択します。

## <a name="identify-the-id-and-key-for-each-project"></a>各プロジェクトの ID とキーの特定

プロジェクトが複数ある場合は、各プロジェクトの ID とキーを特定する必要があります。 キーは、コレクターを実行して VM を検出するときに必要になります。

1. プロジェクト内で、**[開始]** > **[検出と評価]** > **[マシンの検出]** の順に選択します。
2. **[プロジェクトの資格情報をコピーします]** で、プロジェクトの ID とキーをコピーします。 
    ![[プロジェクトの資格情報をコピーします]](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>vCenter の統計レベルを設定する
次の表は、検出中に収集されるパフォーマンス カウンターの一覧です。 カウンターは、既定では vCenter Server のさまざまなレベルで使用可能です。 

すべてのカウンターが正しく収集されるように、統計レベルを一般の最高レベル (3) に設定することをお勧めします。 vCenter を低いレベルに設定した場合、完全な形で収集できるカウンターがわずかになり、残りのカウンターがレベル 0 に設定したのと同じ結果になってしまう可能性があります。 その結果、評価が不完全なデータを示す場合があります。 

また、次の表では、特定のカウンターが収集されなかった場合に影響を受ける評価結果を示しています。

|カウンター                                  |Level    |デバイスごとのレベル  |評価の影響                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |該当なし                |推奨される VM サイズとコスト                    |
|mem.usage.average                        | 1       |該当なし                |推奨される VM サイズとコスト                    |
|virtualDisk.read.average                 | 2       |2                 |ディスク サイズ、ストレージ コスト、VM サイズ         |
|virtualDisk.write.average                | 2       |2                 |ディスク サイズ、ストレージ コスト、VM サイズ         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |ディスク サイズ、ストレージ コスト、VM サイズ         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |ディスク サイズ、ストレージ コスト、VM サイズ         |
|net.received.average                     | 2       |3                 |VM サイズとネットワーク コスト                        |
|net.transmitted.average                  | 2       |3                 |VM サイズとネットワーク コスト                        |

> [!WARNING]
> 統計レベルを高く設定した場合は、パフォーマンス カウンターを生成するのに最大 1 日かかります。 そのため、1 日経ってから検出を実行することをお勧めします。

## <a name="run-the-collector-to-discover-vms"></a>コレクターを実行して VM を検出する

実行する必要があるそれぞれの検出について、コレクターを実行して指定のスコープの VM を検出します。 1 つずつ検出を実行します。 検出の同時実行はサポートされておらず、各検出は異なるスコープである必要があります。

1. vSphere Client コンソールで、[VM] を右クリックして **[Open Console]\(コンソールを開く\)** を選択します。
2. アプライアンスの設定で、言語、タイム ゾーン、パスワードを指定します。
3. デスクトップにある **[コレクターの実行]** ショートカットをクリックします。
4. Azure Migrate コレクターで、**[Set up prerequisites]\(前提条件の設定\)** を開きます。

   a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが Pluralsight アプリケーションへのサインオンに使用する次の URL を入力します。 ライセンス条項に同意し、サード パーティの情報を確認します。

   VM がインターネットにアクセスできることをコレクターがチェックします。
   
   b. VM がプロキシ経由でインターネットにアクセスしている場合は、**[Proxy settings]\(プロキシの設定\)** を選択し、プロキシ アドレスとリスニング ポートを指定します。 プロキシで認証が必要な場合は、資格情報を指定します。

   コレクター サービスが実行されていることをコレクターがチェックします。 このサービスは、既定でコレクター VM にインストールされています。

   c. VMware PowerCLI をダウンロードしてインストールします。

5. **[Specify vCenter Server details]\(vCenter Server 詳細の指定\)** で、次の操作を行います。
    - vCenter Server の名前 (FQDN) または IP アドレスを指定します。
    - **[ユーザー名]** と **[パスワード]** で、コレクターが vCenter Server の VM を検出するために使用する読み取り専用の資格情報を指定します。
    - **[Select scope]\(スコープの選択\)** で、VM 検出のスコープを選択します。 コレクターは、指定されたスコープ内の VM のみを検出できます。 スコープは、指定のフォルダー、データセンター、またはクラスターに設定できます。 VM の数は 1,000 台を超えないようにします。 

6. **[Specify migration project]\(移行プロジェクトの指定\)** で、プロジェクトの ID とキーを指定します。 ID とキーをコピーしなかった場合は、コレクター VM から Azure Portal を開きます。 プロジェクトの **[概要]** ページで、**[マシンの検出]** を選択して値をコピーします。  
7. **[View collection progress]\(収集の進行状況の表示\)** で検出プロセスを監視し、VM から収集されたメタデータがスコープ内にあることを確認します。 コレクターがおおよその検出時間を表示します。


### <a name="verify-vms-in-the-portal"></a>ポータル内での VM の特定

検出時間は検出している VM の数によって異なります。 通常、VM が 100 台の場合、コレクターが実行を終了した後、検出が完了するまで約 1 時間かかります。 

1. Azure Migrate プロジェクト内で、**[管理]** > **[マシン]** の順に選択します。
2. 検出対象の VM がポータルに表示されていることを確認します。


## <a name="next-steps"></a>次の手順

- 評価のために[グループを作成する](how-to-create-a-group.md)方法を確認します。
- 評価の計算方法の[詳細を確認](concepts-assessment-calculation.md)します。
