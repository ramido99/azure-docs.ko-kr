---
title: "Azure Active Directory에서 MyApps portal에 대한 도움이 필요합니까 | Microsoft Docs"
description: "액세스 패널에서 일반적인 작업을 수행하기 위한 지침을 확인합니다."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 9bec51e1d49308baecc76143ec80902d2da418e8
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2018
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>MyApps portal에 대한 도움이 필요합니까?

아마도 MyApps portal을 사용할 때 안타깝게도 문제가 발생했기 때문에 이 페이지에 도달했을 것입니다. 문제를 해결하기 위해 기술 지원팀 또는 관리자에게 문의해야 하는 경우 도움이 될 수 있는 몇 가지 문제 해결 항목은 다음과 같습니다.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>MyApps portal에 로그인하는 데 문제가 발생했습니다.

확인해야 할 일반적인 문제:

- 올바른 URL([https://myapps.microsoft.com](https://myapps.microsoft.com))에 로그인되어 있는지 확인합니다.

- 브라우저의 신뢰할 수 있는 사이트에 URL을 추가해보세요.

- 암호가 만료되거나 기억나지 않는지 확인합니다. 암호를 업데이트하는 방법에 대한 자세한 내용은 [여기](active-directory-passwords-update-your-own-password.md)를 확인합니다.

- 인증 연락처가 최신 정보이고 사용자의 액세스를 차단하지 않는지 확인합니다. 인증 정보를 설정하는 방법에 대한 자세한 내용은 [여기](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user)를 확인합니다.

- 브라우저의 쿠키를 지우고 다시 로그인해봅니다.

로그인을 시도하는 동안 문제가 계속 발생하는 경우 자세한 도움말은 관리자에게 문의하세요.


## <a name="how-do-i-update-my-password"></a>암호를 업데이트하려면 어떻게 할까요?

암호를 잊어버린 경우, IT 부서에서 암호를 받지 못한 경우, 계정에서 암호가 잠긴 경우 또는 암호를 변경하려는 경우 자세한 내용은 [Azure AD 암호를 잊어버렸어요. 도와주세요!](active-directory-passwords-update-your-own-password.md)를 참조하세요.

## <a name="how-do-i-register-for-password-reset"></a>암호 재설정을 위해 등록하려면 어떻게 할까요?

최종 사용자로서 SSPR(셀프 서비스 암호 재설정)을 사용하는 사람에게 문의하지 않고도 암호를 재설정하거나 계정 잠금을 해제할 수 있습니다. 이 기능을 사용하려면 먼저 인증 방법을 등록하거나 관리자가 입력한 미리 정의된 인증 방법을 확인해야 합니다. 자세한 내용은 [셀프 서비스 암호 재설정 등록](active-directory-passwords-reset-register.md)을 참조하세요.


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>내 앱 보안 로그인 확장을 설치하는 데 문제가 있습니다.

브라우저 요구 사항을 충족하는지를 확인합니다.

- 포털에는 JavaScript를 지원하고 CSS를 사용하도록 설정한 브라우저가 필요합니다. 암호 기반 Single Sign-On 앱을 사용하는 경우 포함되는 확장도 설치해야 합니다. 사용자가 암호 기반 Single Sign-On 앱에 구성된 응용 프로그램을 시작할 때 이 확장이 자동으로 다운로드됩니다.

- 확장에 대한 브라우저 요구 사항은 다음과 같습니다.
    - Windows 10 Anniversary Edition 이상 Edge
    - Windows 7 이상 및 Mac OS X 이상 Chrome
    - Windows XP SP2 이상 및 Mac OS X 10.6 이상 Firefox 26.0 이상
    - Windows 7 이상에서 Internet Explorer 8, 9, 10, 11(지원 제한)

아래와 같은 직접 링크에서 Chrome 및 Edge에 대한 확장을 다운로드할 수 있습니다.

- [Chrome 확장](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Edge 확장](https://www.microsoft.com/store/apps/9pc9sckkzk84)

설치한 후에 문제가 발생하는 경우 다음 단계를 시도합니다.

- 확장을 사용할 수 있는 브라우저 확장 설정을 확인합니다.

- 브라우저를 다시 시작하고 My Apps portal에 로그합니다.

- 브라우저의 쿠키를 지우고 My Apps portal에 로그인합니다.
- 진단 도구 및 IE용 확장을 구성하는 방법에 대한 단계별 지침에 액세스하려면 [Internet Explorer의 액세스 패널 확장 문제 해결](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) 가이드를 따릅니다.

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>내 앱 보안 로그인 확장을 사용하려면 어떻게 해야 하나요?
확장에 대한 내 앱 기본 URL 변경

Https://myapps.microsoft.com이 아닌 다른 앱 URL을 사용하는 경우 다음 단계를 통해 기본 URL을 구성해야 합니다.
1. 확장에 로그인되지 않은 상태에서 확장 아이콘을 **마우스 오른쪽 단추로 클릭**합니다.
2. 메뉴에서 **내 앱 URL 선택**을 클릭합니다.
3. 기본 URL을 **선택**합니다.
4. 확장 아이콘을 클릭합니다.
5. **시작하려면 로그인하세요.**를 선택하여 확장에 로그인합니다.

브라우저에서 앱에 직접 로그인
1. 확장을 설치한 후에 **시작하려면 로그인하세요.**를 선택하여 확장에 로그인합니다.
2. 로그인하려면 앱의 **로그온 URL**로 이동합니다. 이 URL은 일반적으로 로그인 양식을 표시하는 앱의 URL입니다.
3. 확장 상태가 변경되고 암호를 사용할 수 있다고 표시되면 **확장 아이콘**을 클릭하여 로그인합니다.

확장에서 앱 시작
1. 확장을 설치한 후에 **시작하려면 로그인하세요.**를 선택하여 확장에 로그인합니다.
2. 확장 아이콘을 클릭하여 해당 메뉴를 엽니다.
3. 내 앱 포털에서 사용할 수 있는 앱을 **검색**합니다.
4. **검색 결과**에서 앱을 클릭하여 시작합니다.
5. 시작된 마지막 3개 앱은 **최근에 사용됨** 바로 가기 목록에도 표시됩니다.

> [!NOTE]
> 이러한 옵션은 Edge, Chrome, Firefox에서만 사용할 수 있습니다.

## <a name="how-do-i-add-a-new-app"></a>새 앱을 추가하려면 어떻게 할까요?

1.  **앱** 페이지에서 **앱 추가**를 클릭합니다.

2.  추가하려는 앱을 검색한 다음 **추가**를 클릭합니다.

**설명**

- 관리자가 해당 계정에 대해 이를 활성화한 경우 이 옵션에 대한 액세스만을 가집니다.

- 앱에 권한이 필요한 경우 관리자 승인을 기다려야 할 수 있습니다.


## <a name="how-do-i-manage-my-group-memberships"></a>그룹 멤버 자격을 관리하려면 어떻게 할까요?

1. **그룹** 타일을 클릭합니다. 
2. 그룹을 만들려면 내가 소유한 그룹에서 **그룹 만들기**를 클릭하고 지침을 따릅니다.
3. 그룹에 가입하려면 내가 속한 그룹에서 **그룹 조인**을 클릭하고 지침을 따릅니다.

**설명**

- 관리자가 해당 계정에 대해 이를 활성화한 경우 이 옵션에 대한 액세스만을 가집니다.

- 멤버로 속한 그룹을 사용하면 세부 정보를 보고 그룹을 탈퇴할 수 있습니다.

- 소유한 그룹을 사용하면 세부 정보를 보고, 멤버를 추가하거나 제거하고, 그룹을 탈퇴할 수 있습니다.


## <a name="next-steps"></a>다음 단계

문제 해결 관련 정보는 [응용 프로그램 액세스 패널 웹 사이트 또는 모바일 응용 프로그램을 사용하는 문제](active-directory-application-access-panel-content-map.md)를 참조하세요.

