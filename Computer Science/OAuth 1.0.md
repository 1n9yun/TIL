# OAuth 1.0

> Reference
>
> https://tools.ietf.org/html/rfc5849

> Description
>
> RFC5849를 읽으며 번/의역했으며 불필요해보이는 정보는 제외했을 수 있으므로 원문도 확인!
>
> 또한 서투른 영어 실력으로 번역체와 직독직해가 난무함!



## Abstract

OAuth는 클라이언트에게 서버 리소스 소유자를 대신하여 서버 리소스에 액세스할 방법을 제공합니다.

> It also provides a process for end-users to authorize third-party access to their server resources without sharing their credentials (typically, a username and password pair), using user-agent redirections.
>
> 유저-에이전트 리디렉션을 통해 사용자의 인증 정보를 공유하지 않고 서드파티 리소스에 액세스할 권한을 부여하는 과정도 제공한다는 의미같은데
>
> 정확한 해석이라는 생각이 들면 한글로 수정!



## Status of this Memo

이 문서는 ietf의 문서이며 표준은 아니고 정보 제공 목적으로 작성되었다고 한다.



## Copyright Notice

> Copyright (c) 2010 IETF Trust and the persons identified as the document authors.  All rights reserved.
>
> This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents (http://trustee.ietf.org/license-info) in effect on the date of publication of this document.  Please review these documents carefully, as they describe your rights and restrictions with respect to this document.  Code Components extracted from this document must include Simplified BSD License text as described in Section 4.e of the Trust Legal Provisions and are provided without warranty as described in the Simplified BSD License.



## 1. Introduction

OAuth 프로토콜은 2007년 10월에 1.0버전이 stabilize 되었으며 2009년에 수정(Revision A)되었다.

> OAuth 1.0a (Revision A) - <http://oauth.net/core/1.0a>

이 문서는 OAuth Core 1.0 Revision A의 정보를 제공하며 그 이후로 보고된 erreta와 수정 내용을 제공한다.

전통적인 client-server 인증 모델에서, 클라이언트는 서버에서 호스팅하는 리소스에 액세스하기 위해 credentials를 사용한다.

분산 웹 서비스 및 클라우드 사용이 증가함에 따라 서드파티 어플리케이션은 이러한 server-hosted 리소스에 액세스해야 한다.

OAuth는 전통적인 client-server 인증 모델에 세 번째 역할인 리소스 소유자를 도입한다.

OAuth 모델에서 클라이언트(리소스 소유자는 아니지만 대신 작동하는)는 리소스 소유자가 제어하지만 서버에서 호스팅하는 리소스에 대한 액세스을 요청한다.

게다가, OAuth를 통해 리소스 소유자의 권한뿐만 아니라 요청하는 클라이언트의 신원도 확인할 수 있다.



OAuth는 클라이언트에게 서버 리소스 소유자를 대신하여 서버 리소스에 액세스할 방법을 제공합니다.
또한 유저-에이전트 리디렉션을 통해 사용자의 인증 정보를 공유하지 않고 서드파티 리소스에 액세스할 수 있는 권한을 부여하는 과정도 제공한다.

예를 들어 웹 사용자 (자원 소유자)는 사용자 이름과 암호를 인쇄 서비스와 공유하지 않고 사진 공유 서비스 (서버)에 저장된 개인 사진에 대한 인쇄 서비스 (클라이언트) 액세스 권한을 부여 할 수 있습니다. 

> Instead, she authenticates directly with the photo sharing service which issues the printing service delegation-specific credentials.
> 
> 무슨 말일까



클라이언트가 리소스에 접근하기 위해서는 일단 리소스 소유자로부터 허가를 얻어야 한다. 이 허가는 토큰이나 matching shared-secret의 형태로 표현된다. 

토큰의 목적은 리소스 소유자가 자격증명을 클라이언트와 공유할 필요를 없애기 위함이다. 

리소스 소유자 credentials와는 다르게 토큰은 제한된 scope, limited lifetime, revoked independetly와 함께 발행된다.


> This specification consists of two parts. The first part defines a redirection-based user-agent process for end-users to authorize client access to their resources, by authenticating directly with the server and provisioning tokens to the client for use with the authentication method.  The second part defines a method for making authenticated HTTP [RFC2616] requests using two sets of credentials, one identifying the client making the request, and a second identifying the resource owner on whose behalf the request is being made.
>
> The use of OAuth with any transport protocol other than [RFC2616] is undefined.

라고도 한다.



### 1.1 Terminology

**client**

- OAuth-authenticated requests를 할 수 있는 HTTP 클라이언트

**server**

* OAuth-authenticated requests를 수락할 수 있는 HTTP 서버

**protected resource**

* OAuth-authenticated requests를 통해 서버에서 얻을 수 있는 접근 제한 리소스

**resource owner**

* credentials를 사용하여 서버에 인증함으로써 protected resources에 접근하고 제어할 수  있는 Entity 

**credentials**

* shared secret과 매칭되는 고유한 식별자 쌍
* OAuth는 client, temporary, token의 세 가지 credentials 클래스를 정의한다.
* 각각 요청을 만드는 클라이언트, 권한부여 요청, 액세스 권한을 식별하고 인증하는데 사용한다.

**token**

* server로부터 발행되는 고유한 식별자이며 client가 인증 요청과 resource owner(client에 의해 인증을 얻거나 인증을 요청받은)를 연관시키기(?) 위해 사용한다.

* 클라이언트가 토큰의 소유권을 설정하는데 사용되는 shared secret과 resource owner를 나타내는 권한을 가지고 있다.

The original community specification는 다른 terminology를 사용했으며 아래와 같이 매칭된다.

* Consumer : client
* Service Provider: server
* User: resource owner
* Comsumer Key and Secret : client credentials
* Request Token and Secret : temporary credentials
* Access Token and Secret : token credentials



### 1.2 Example



### 1.3 Notational Conventions

> [RFC2119](https://tools.ietf.org/html/rfc2119)
>
> **MUST** (Required, Shall) 은 절대적으로 필요한 specification
>
> **MUST NOT**(Shall Not) 은 절대적으로 금지되는 specification
>
> **SHOULD**(Recommended) 는 특정 상황에서 특정 항목을 무시할 타당한 이유가 있을 수 있지만 다른 방법을 선택하기 전에 전체 의미를 이해하고 신중하게 검토해야 함을 의미
>
> **SHOULD NOT**(Not Recommended) 은 특정 동작이 수용가능하거나 심지어 유용할 때 특정 상황에서 타당한 이유가 있을 수 있지만 이 Label로 설명된 모든 행동을 구현하기 이전에 전체 의미를 이하고 신중하게 검토해야 함을 의미
>
> **MAY**(Optional) 는 말그대로 옵션인 항목이며 옵션을 포함하지 않는 구현체는 옵션을 포함하는 구현체와의 상호운용을 기능상의 축소가 있더라도 반드시 준비해야 한다. 그 반대의 경우에도 다른 구현과 상호 운용되도록 준비해야 한다. (물론 옵션이 제공하는 기능에 대해서는 제외)
