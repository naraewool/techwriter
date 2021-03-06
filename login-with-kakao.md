---
description: Kakao Developers (English ver.)
---

# LOGIN WITH KAKAO

                                                                                                         You are in **\[REST API\]** &gt; **\[LOGIN WITH KAKAO\]**

\*\*\*\*

This page explains how to integrate **Login with Kakao** on your app.

> **Table of Contents**
>
> * Overview
> * Login Flow
> * Before you begin
> * Integrating Kakao Login
> * After Login
> * Frequent Questions and Answers
> * Release Note
> * Glossary

## **Overview**

Kakao Login API enables users to log in your application with their kakao accounts in a fast, simple, and secure way. Users do not need to sign up or log in a third application with the easy-to-use functions, which helps you to keep users on your application and maximize the number of people using your application.



### **Login Flow** 

**\(Or Conceptual Overview\)**

Kakao Login API complies with OAuth 2.0. The Open Authorization\(OAuth\) is a standard authorization framework that allows a third-party application to access limited information with an access token without user’s ID and password. Here is the process of OAuth authentication.

![Login Flow](.gitbook/assets/image%20%2820%29.png)

1. A User clicks the \[Login with Kakao\] button.

2. Once the user logs in with the user’s Kakao ID and password, the user is redirected to a screen that asks Users’ consent to access the limited information. 

3. Once the user approves permissions, the Kakao authorization server validates the user’s credentials and issues Authorization Code. The user is redirected back to your app via `redirect_uri` with the Authorization Code.

4. Once your application validates the Authorization code, your app requests Access Token and Refresh Token.

5. The Kakao authorization server validates and issues Access Token and Refresh Token based on the Authorization code,  and provides authentication.

  
****

## **Before you begin**

### **1.**     **Create an App**

'시작하기 &gt; [앱 생성](https://developers.kakao.com/docs/restapi/getting-started#앱-생성)' 항목

### **2.**     **Configure an App**

'시작하기 &gt; [사용자 관리 설정](https://developers.kakao.com/docs/restapi/getting-started#사용자-관리-설정)' 항목 



#### Step 1: Activate \[Login with Kakao\]

![](.gitbook/assets/image%20%289%29.png)

#### Step 2: Specify Items for Permissions 

![](.gitbook/assets/image%20%281%29.png)

#### Step 3: Set redirect URL

![](.gitbook/assets/image%20%2839%29.png)



## Integrating Kakao Login

### 1. Authentication

Kakao Login API complies with OAuth 2.0. The Open Authorization\(OAuth\) is a standard authorization framework that allows a third-party application to access limited information with an access token without user’s ID and password. For the details about OAuth 2.0, click [here](https://tools.ietf.org/html/rfc6749).

> Step 1: Get an Authorization code &gt;  Step 2: Get an Access Token &gt;  
> Step 3: Refresh an Access Token &gt;  Step 4: Validate an Access Token & Get Information

#### 

#### Step 1: Get an Authorization code



#### Step 2: Get an Access Token

After a user has obtained the Authorization code, the user can get Access Token and Refresh Token via the Authorization code used to make API calls.

**\[Request\]**

```text
POST /oauth/token HTTP/1.1
Host: kauth.kakao.com
Content-type: application/x-www-form-urlencoded;charset=utf-8
```

You can request with the following query parameters in the request body with POST method in a form-urlencoded format.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Parameters</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
      <th style="text-align:left"><b>Type</b>
      </th>
      <th style="text-align:left"><b>Required</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">grant_type</td>
      <td style="text-align:left"><code>authorization_code</code> (fixed)</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">O</td>
    </tr>
    <tr>
      <td style="text-align:left">client_id</td>
      <td style="text-align:left">
        <p>A REST API key that is an identifier for an application and issued by
          Kakao when creating an application.</p>
        <p>You can check this key in [Settings] &gt; [User Management].</p>
      </td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">O</td>
    </tr>
    <tr>
      <td style="text-align:left">redirect_uri</td>
      <td style="text-align:left">Callback URL that users are redirected to.
        <br /><b>!! This URL must coincide with the designated URL in [Settings]</b> &gt;
        [<b>User Management]</b> &gt; [<b>Login Redirect URI]</b>.</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">O</td>
    </tr>
    <tr>
      <td style="text-align:left">code</td>
      <td style="text-align:left"><code>authorization_code </code>that is issued in the [Step 1: Get an
        Authorization code] to get an access token.</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">O</td>
    </tr>
    <tr>
      <td style="text-align:left">client_secret</td>
      <td style="text-align:left">
        <p><code>client_secret code</code>that is issued by Kakao when creating an
          application.</p>
        <p>You can check this in [Settings] &gt; [Advanced] &gt; [Client Secret].</p>
      </td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">X</td>
    </tr>
  </tbody>
</table>For example, if you request with POST method as follows:

{% tabs %}
{% tab title="Example Request " %}
```text
curl -v -X POST https://kauth.kakao.com/oauth/token \
 -d 'grant_type=authorization_code' \
 -d 'client_id={app_key}' \
 -d 'redirect_uri={redirect_uri}' \
 -d 'code={authorize_code}'
```
{% endtab %}
{% endtabs %}

\*\*\*\*

**\[Response\]**

It returns the resources in JSON format if successful.

{% tabs %}
{% tab title="Example Response" %}
```text
Content-Type: application/json;charset=UTF-8
{
    "access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "token_type":"bearer",
    "refresh_token":"yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy",
    "expires_in":43199,
    "scope":"Basic_Profile"
}
```
{% endtab %}
{% endtabs %}

The response body includes the following parameters.

| **Property** | **Description** | **Type** |
| :--- | :--- | :--- |
| access\_token | A token used to call API | String |
| token\_type | `bearer` \(fixed\), a type of a token. | String |
| refresh\_token | A token used to get a new access token | String |
| expire\_in | Amount of time in _seconds_ until the access token expires | Integer |
| scope | Permission to granted by the user | String |

&gt;표 추가.



#### Step 3: Refresh an Access Token

'사용자 토큰 갱신하기' 항목 



#### Step 4: Validate an Access Token & Get Information

'사용자 토큰 유효성 검사 및 정보 얻기' 항목

### 

### 2. Add a Login Button

개발자 입장에서 순차적으로 로그인 API를 구현하도록 재배치  
: 인증과정\(Authentication\) 후 , \[카카오 로그인\] 버튼 추가하도록 배치 변경



#### Button Download

[https://developers.kakao.com/buttons\#Login\_Buttons](https://developers.kakao.com/buttons#Login_Buttons)

![](.gitbook/assets/image%20%2852%29.png)

#### 

#### Design guidelines

일관된 디자인의 버튼 제공을 위하여 버튼 디자인 가이드라인 항목 추가



#### Best Practices

**\(Or User Cases\)**

\[카카오 로그인\] 버튼을 효과적으로 사용할 수 있는 방법**\(Tip\)**과 예시 추가 

  


## After Login

**\(Or User Management/ Advanced\)**

\*\*\*\*

### Log out of Kakao

'로그아웃' 항목

### Disconnect an App

'연동 해제' 항목

### Save User Profile

'사용자 정보 저장' 항목

### Retrieve User Profile 

'사용자 정보 요청' 항목

### Retrieve User **ID** List  

'사용자 ID 리스트 요청' 항목

### Make an Additional Request 

'동적동의' 항목

### 

### \*\*\*\*

## Frequent Asked Questions

자주 묻는 질문에 대한 트러블슈팅 내용 추가



## Change Log

**\(Or Release Note\)**

'v1 사용자 정보 요청과 달라진 점'을 Change Log\(또는 Release Note\)로 이동. 변경 이력을 표로 정리 



* **Updates to 'Retrieve User Profile'**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Version</th>
      <th style="text-align:left">Release Date</th>
      <th style="text-align:left">Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>v2</p>
        <p>(current)</p>
      </td>
      <td style="text-align:left">yyyymmdd</td>
      <td style="text-align:left">
        <ul>
          <li>Changed the parameter name from <code>propertyKeys</code>to<code>property_keys</code>that
            is used to get a specific property only.</li>
          <li>Added new values of kakao account information --- profile, age_range,
            birthday, and gender.</li>
          <li>Fixed a bug that ~~~</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">v1</td>
      <td style="text-align:left">yyyymmdd</td>
      <td style="text-align:left">
        <ul>
          <li>Published a new set of REST API Developer Guide.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Glossary

개발가이드에 나오는 용어를 정리한 용어집\(Glossary\) 항목 새로 추가 





\*\*\*\*

{% hint style="info" %}
**Is this page helpful?**

 **Yes / Kind of / No**
{% endhint %}

\*\*\*\*

 **&gt;&gt; Next steps:** Kakao Talk

