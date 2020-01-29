---
title: 「エンタープライズアプリケーション」と「アプリの登録」の違いについて
date: 2019-12-06
tags:
  - Azure AD
  - Application
---

> 本記事は Technet Blog の更新停止に伴い https://blogs.technet.microsoft.com/jpazureid/2018/12/28/enterprise-applications-app-registrations/ の内容を移行したものです。
> 元の記事の最新の更新情報については、本内容をご参照ください。

こんにちは、Azure Identity チームの宮林です。

今回は Azure Active Directory の管理項目にある、「エンタープライズアプリケーション」と「アプリの登録」のそれぞれの違いについて紹介します。  
初めてアプリケーションの登録を行おうとした際にどちらで設定すれば良いのかと、気になった方も多いのではないでしょうか。  
この記事では、そのような疑問に対する答えとして、それぞれの違いについて説明します。

# 「エンタープライズアプリケーション」と「アプリの登録」の違い

アプリケーションをテナント (Azure AD) に登録する際に「エンタープライズアプリケーションの登録」と「アプリの登録」それぞれの方法では、登録する目的が異なります。  
端的に説明すれば以下のようになります。

「エンタープライズアプリケーション」：既存の SaaS アプリケーション (オンプレミスの環境で提供しているアプリを含む) をテナントに統合する場合に用いる。

「アプリの登録」：ユーザーやテナントの情報にアクセスして動作するために開発したアプリケーションを公開、もしくは利用するために用いる。

(ここで、統合が指す意味は、SaaS アプリケーションに対する シングルサインオン (SSO) の構成や、ユーザーのプロビジョニング構成を行うことなどを指します。)

それぞれの項目で、アプリケーションの登録を行う場合の目的の違いについて説明します。

## 「エンタープライズアプリケーション」からアプリケーションの登録を行う場合
<!-- textlint-disable -->
例えば、GitHub、Salesforce、Slack や G Suite など、[Azure Marketplace](https://azuremarketplace.microsoft.com/ja-jp) もしくは、Azure ポータルより [Azure Active Directory] - [エンタープライズアプリケーション] - [+新しいアプリケーション] を選択した際に表示されるページで公開されている、事前に統合された SaaS アプリケーションをテナントに追加して利用するために使用します。  
<!-- textlint-enable -->
テナントに追加されたアプリケーションをユーザーに対して割り当てることで、アクセス可能なユーザーを限定することなどの管理も「エンタープライズアプリケーション」の項目から行います。  
それぞれの SaaS アプリケーションに対する SSO、ユーザーのプロビジョニングの設定方法は以下のチュートリアルを参照ください。

SaaS アプリケーションと Azure Active Directory の統合に関するチュートリアル  
https://docs.microsoft.com/ja-jp/azure/active-directory/saas-apps/tutorial-list

事前に統合された SaaS アプリケーションの他にも、次の目的で Azure AD と外部のアプリケーションを連携する場合には、「エンタープライズアプリケーション」の項目から設定を行い、ユーザーの割り当てなどによって、アクセス許可の管理をすることができます。

 - オンプレミス環境で利用しているアプリケーションを、Azure AD Application Proxy 経由で利用する。
 - SAML に対応した外部のアプリケーションとのシングルサインオンを構成する。
 - SCIM に対応した外部のアプリケーションに対してユーザー プロビジョニングを構成する。

詳細については、以下の公開情報をご参照ください。

オンプレミス アプリケーションへの安全なリモート アクセスを実現する方法  
https://docs.microsoft.com/ja-jp/azure/active-directory/manage-apps/application-proxy

ギャラリー以外のアプリケーションに SAML ベースのシングル サインオンを構成する  
https://docs.microsoft.com/ja-jp/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications

Azure Active Directory による SaaS アプリへのユーザー プロビジョニングとプロビジョニング解除の自動化  
https://docs.microsoft.com/ja-jp/azure/active-directory/manage-apps/user-provisioning

## 「アプリの登録」からアプリケーションの登録を行う場合
Microsoft Graph API などを利用して、ユーザーやテナントの情報にアクセスして動作するために、開発したアプリケーションの公開、もしくは利用するために、「アプリの登録」の項目を利用します。  
「アプリの登録」の画面では、開発しているアプリが必要とする API のアクセス許可を設定したり、アプリが Azure AD へ接続する際に利用する証明書やシークレットの管理を行います。  
その他にも、ユーザーがアプリに対してサインインを行い、認証応答とともに Azure AD からリダイレクトされる URL の登録など、開発しているアプリの認証に関する設定を行います。  
Azure AD には、認証応答が意図しないエンドポイント (URL) に渡されないように、登録されたリダイレクト URL 以外にはリダイレクトしません。  
登録されていない URL にリダイレクトするような要求である場合は、"AADSTS50011" のエラーコードとともにエラー画面が表示されます。


「アプリの登録」の画面は、従来の設定画面から更新されました。  
これにより、利用できるエンドポイントが v2.0 と v1.0 の両方を利用いただけるようになりました。  
v2.0 のエンドポイントと、v1.0 のエンドポイントの比較と制限事項については次の公開情報をご参照ください。

Azure AD v2.0 エンドポイントと v1.0 エンドポイントの比較  
\- 制限事項  
https://docs.microsoft.com/ja-jp/azure/active-directory/develop/azure-ad-endpoint-comparison#limitations

「アプリの登録」からアプリを登録すると、テナントにアプリケーションが統合 (登録) された状態となりますので、「エンタープライズ アプリケーション」にもその結果としての同名のオブジェクトが表示されます。  
この追加されたアプリに対して、ユーザーに割り当てを行い、アクセス許可を設定する場合には、「エンタープライズアプリケーション」の項目から設定します。

### 詳細情報

「エンタープライズアプリケーション」と「アプリの登録」の一覧に表示されているオブジェクトは、その種類が異なります。  
それぞれの対応については、以下のとおりです。

 - 「エンタープライズアプリケーション」の一覧に表示されるオブジェクト : サービス プリンシパル オブジェクト
 - 「アプリの登録」の一覧に表示されるオブジェクト : アプリケーション オブジェクト

アプリケーション オブジェクトは、サービスプリンシパル オブジェクトの雛形のようなもので、この雛形を基に各テナント内にサービスプリンシパル オブジェクトが作成されます。  
雛形 (アプリケーション オブジェクト) には、要求する API のアクセス許可や、Azure AD から発行されるトークンに含む追加の情報などがマニュフェストに定義されています。  
アプリケーション オブジェクトは「アプリの登録」を行ったテナントに一つだけ作成されます。

雛形を基に、各テナントはそのテナント内でアプリが動作するためのサービスプリンシパル オブジェクトを作成します。このため、サービスプリンシパル オブジェクトは、登録されたアプリを使用するすべてのテナントに作成されます (アプリケーションオブジェクトとサービスプリンシパルは一対多の関係)。  
サービスプリンシパル オブジェクトには、承諾した API のアクセス許可などテナント毎の情報が保存され、ユーザーの割り当てを行うことでアクセス許可を定義することもできます。


より詳細な説明については、以下の公開情報をご確認ください。

Azure Active Directory のアプリケーション オブジェクトとサービス プリンシパル オブジェクト  
https://docs.microsoft.com/ja-jp/azure/active-directory/develop/app-objects-and-service-principals

「エンタープライズアプリケーション」と「アプリの登録」の管理項目の違いについては以上となります。

### 終わりに
「アプリの登録」からアプリケーションを登録することは、特に権限が付与されていない一般ユーザーでも行うことができます。  
制限を加えることもできますが、既定の状態としておくことを推奨しています。推奨される理由については以下の投稿も併せてご参照ください。

[「ユーザーはアプリケーションを登録できる」の設定について](../azure-active-directory/users-can-register-applications.md)  

ご不明な点がございましたら弊社サポートまでお気軽にお問い合わせください。

上記内容が少しでも皆様の参考となりますと幸いです。  
※本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。