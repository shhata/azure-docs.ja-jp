---
title: "ライセンスのセルフサービスによるパスワードのリセット - Azure Active Directory"
description: "Azure AD のセルフ サービスによるパスワード リセットのライセンス要件"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: ca6d7a6d2b7ffa49f3656510010cfb49a81e22d7
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2018
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Azure AD のセルフ サービスによるパスワード リセットのライセンス要件

Azure Active Directory (Azure AD) のパスワード リセットが機能するには、*組織で少なくとも 1 つのライセンスが割り当てられている必要があります*。 パスワード リセット エクスペリエンスでユーザーごとのライセンスを適用することはありません。 Microsoft ライセンス契約とのコンプライアンスを維持するには、プレミアム機能を使用するすべてのユーザーにライセンスを割り当てる必要があります。

* **クラウド ユーザーのみ**: Office 365 のすべての有料 SKU、または Azure AD Basic
* **クラウド**または**オンプレミス ユーザー**: Azure AD Premium P1 または P2、Enterprise Mobility + Security (EMS)、または Microsoft 365

## <a name="licenses-required-for-password-writeback"></a>パスワード ライトバックで必要なライセンス

パスワード ライトバックを使用するには、次のライセンスのいずれかがテナントに割り当てられている必要があります。

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 (Plan E3)
* Microsoft 365 (Plan E5)

> [!WARNING]
> スタンドアロンの Office 365 ライセンス プランは、*パスワード ライトバックをサポートしていません*。この機能を動作させるには、上記プランのいずれかが必要になります。
>

追加のライセンス情報(コストを含む)については、以下のページをご覧ください。

* [Azure Active Directory の価格](https://azure.microsoft.com/pricing/details/active-directory/)
* [Microsoft Azure Active Directory の機能と働き](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>グループまたはユーザー ベースのライセンスの有効化

現在、AzureAD はグループベースのライセンスをサポートしています。 管理者はライセンスを一度に 1 つずつユーザーに割り当てるのではなく、ライセンスをユーザーのグループに一括して割り当てることができます。 詳細については、[ライセンスの割り当て、検証、および問題の解決](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)に関するページを参照してください。

一部の Microsoft サービスは、すべての場所で利用できないことがあります。 ライセンスをユーザーに割り当てる前に、管理者はユーザーの **[利用場所]** プロパティを指定しておく必要があります。 ライセンスの割り当ては、Azure Portal の **[ユーザー]** > **[プロファイル]** > **[設定]** セクションで行います。 *グループ ライセンス割り当てを使用する場合に、利用場所が指定されていないユーザーは、ディレクトリの場所を継承します。*

## <a name="next-steps"></a>次の手順

* [SSPR のロールアウトを正常に完了する方法](active-directory-passwords-best-practices.md)
* [パスワードのリセットまたは変更](active-directory-passwords-update-your-own-password.md)
* [セルフサービスのパスワード リセットのための登録](active-directory-passwords-reset-register.md)
* [SSPR が使用するデータと、ユーザー用に事前設定が必要なデータ。](active-directory-passwords-data.md)
* [ユーザーが使用できる認証方法。](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR のポリシー オプション。](active-directory-passwords-policy.md)
* [パスワード ライトバックの概要とその必要性。](active-directory-passwords-writeback.md)
* [SSPR でアクティビティをレポートする方法。](active-directory-passwords-reporting.md)
* [SSPR のすべてのオプションとその意味。](active-directory-passwords-how-it-works.md)
* [不具合が発生していると思われる場合のSSPR のトラブルシューティング方法。](active-directory-passwords-troubleshoot.md)
* [質問したい内容に関する説明がどこにもない。](active-directory-passwords-faq.md)
