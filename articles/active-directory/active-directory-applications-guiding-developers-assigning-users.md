---
title: "Azure AD とアプリケーション: アプリケーションへのユーザーの割り当て | Microsoft Docs"
description: "Azure アプリケーションへのユーザーの割り当てを実装する方法です。"
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
editor: 
ms.assetid: 97ce69c1-4034-4e38-bd82-8caf984f6b98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: 1f25f5e750b705fda387fbaeed7d4e67710e0a18
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-and-applications-assigning-users-to-an-application"></a>Azure AD とアプリケーション: アプリケーションへのユーザーの割り当て
ユーザーとグループをアプリケーションに割り当てる前に、ユーザー割り当てを要求する必要があります。  ユーザー割り当てを要求する方法については、「 [ユーザー割り当ての要求](active-directory-applications-guiding-developers-requiring-user-assignment.md) 」を参照してください。

## <a name="assigning-users-to-an-application"></a>アプリケーションへのユーザーの割り当て
1. 管理者アカウントを使用して、Azure Portal にログインします。
2. メイン メニューの **[すべてのアイテム]** をクリックします。
3. アプリケーションに使用するディレクトリを選択します。
4. **[アプリケーション]** タブをクリックします。
5. このディレクトリに関連付けられているアプリケーションの一覧から、アプリケーションを選択します。
6. **[ユーザーとグループ]** タブをクリックします。
7. アプリケーションに割り当てるユーザーを選択します。
8. **[割り当て]**をクリックします。
9. 確認を求めるメッセージが表示されたら、 **[はい]** をクリックします。

## <a name="next-steps"></a>次のステップ
[!INCLUDE [guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

