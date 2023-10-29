### ▶️ TLS ≠ SSL?

TLS와 SSL은 동일한 프로토콜의 다른 버전이라고 할 수 있다.

- SSL
  - Secure Sockets Layer
  - 통신 시 오고가는 데이터를 **암호화**하여 인터넷 연결을 보호하기 위한 표준 기술
- TLS
  - Transport Layer Security
  - SSL의 향상된 버전

네스케이프가 처음 SSL을 발명했다가 점점 폭넓게 사용되며 정식으로 표준화 기구인 IETF의 관리를 받게되면서 **TLS**라는 공식 명칭이 생겼다.

- TLS 1.0 ⇒ SSL 3.0 계승
- 그러나 TLS보다 `SSL`이라고 더 많이 쓰인다고 한다.

### ▶️ SSL 인증서?

- SSL 프로토콜을 따르는 웹사이트에 **SSL 인증서**가 부여된다.
- 클라이언트와 서버간의 통신을 제 3자가 보증화 해주는 전자화된 문서이다.
- 통신내용이 공격자에게 노출되고 악의적으로 변경되는 것을 막고 서버의 신뢰성을 올릴 수 있다.
- 인증서의 목표는 `클라이언트가 접속한 서버가 신뢰 할 수 있는 서버인지`를 판단하고 `SSL 통신에 사용될 공개키를 클라이언트에게 전달하는 것`이다.
- 위에서 언급한대로 SSL인증서에는 두가지 내용이 들어가는데 첫번째는 `서비스의 정보(CA, 도메인)` 두번째는 `서버측 공개키`이다.
- 클라이언트가 서버에 접속한 직후에 서버는 클라이언트에게 이 인증서 정보를 전달한다. 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증 한 후에 다음 절차를 수행하게 된다. SSL과 SSL 디지털 인증서를 이용했을 때의 이점은 아래와 같다.
  - 통신 내용이 공격자에게 노출되는 것을 막을 수 있다.
  - 클라이언트가 접속하려는 서버가 신뢰 할 수 있는 서버인지를 판단할 수 있다.
  - 통신 내용의 악의적인 변경을 방지할 수 있다.

▶️ CA **(Certificate Authority) ?**

SSL인증서를 기준으로 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지를 확인을 하게 된다. 이러한 일을 해주는 공인된 회사들을 CA라고 한다. 이러한 CA는 브라우저가 리스트를 가지고 있고 이 CA들의 `공개키`들을 가지고 있다

▶️ **인증서로 서버가 신뢰할 수 있는지 판단하는 법**

위에서 말했던 `공개키 서명`방식을 사용한다. 브라우저는 위에서 말했듯 신뢰할 수 있는 사이트와 그 사이트의 공개키를 가지고 있다. 웹 브라우저가 서버에 접속한다면

1. 서버는 SSL 인증서를 제공한다. 이 인증서에는 서버측의 공개키와 서비스의 정보를 담고있다.
2. 브라우저는 이 인증서를 발급한 CA가 자신의 CA리스트에 있는지 확인한다.
3. 있다면 CA의 해당 공개키를 이용해서 인증서를 복호화한다. **복호화가 된다면 이 인증서는 CA의 비공개키에 의해 암호화 되었다는 것을 알 수 있다.**

> SSL 인증서의 역할은 다소 복잡하기 때문에 인증서의 메커니즘을 이해하기 위한 몇가지 지식들을 알고 있어야 한다. 인증서의 기능은 크게 두가지다. 이 두가지를 이해하는 것이 인증서를 이해하는 핵심이다.
>
> 1. 클라이언트가 접속한 서버가 신뢰 할 수 있는 서버임을 보장한다.
> 2. SSL 통신에 사용할 공개키를 클라이언트에게 제공한다.

### ▶️ handshake

- SSL 인증서가 웹사이트/서버와 브라우저 간에 암호화된 연결을 수립하는 과정
- 당연히 웹사이트 방문자에게는 해당 과정이 보이지 않도록 순간적으로 이루어짐
- TLS/SSL handshake의 가장 큰 특징은, **대칭키 방식과 비대칭키 방식을 혼용한다**는 점

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1d40a623-da7d-44e1-9004-b5b5a8861918/c166c17f-7632-4a89-9d86-335949cbefb8/Untitled.png)

1. `client Hello`: client에서 SSL버전 정보와 지원하는 암호화 방식, 무작위 바이트 문자열(이후에 사용하게 되는 중요한 데이터입니다)이 포함 되어 전달. 이미 SSL handshake를 했었다면 세션을 재사용 할 수 있음.
2. `server Hello`: 지원하는 암호화 방식 중 서버에서 어떤것을 사용할 지, 세션 ID, 서버측에서 생성한 무작위 바이트 문자열을 전송. 클라이언트에서 인증서를 요구하게 되면 SSL 인증서를 전송.
3. 인증서를 받게 되면 위에서 언급했던 방식(인증서가 서버가 신뢰할 수 있는지 어떻게 판단할까?)로 신뢰할 수 있는 사이트 인지 판단. 이 과정을 통해 클라이언트는 공개키를 얻게 됨.
4. 이후 클라이언트는 자신이 만든 무작위 바이트 문자열과 서버쪽에서 전송된 무작위 바이트 문자열을 조합하여 `pre master secret`키를 생성. 이 키는 이후 데이터를 주고 받을 때 대칭키 방식을 사용할 때 사용하게 됨. 이 `pre master secret`키를 서버로 전송할 때 인증서에서 받았던 키를 이용하여 공개키 방식 암호화를 하게 됨. 그리고 서버쪽으로 전송.
5. 서버쪽에서는 수신한 `pre master secret`키를 비밀키를 이용하여 복호화 하여 얻게됨.
6. server client 둘 다 일련의 과정을 거쳐 `pre master secret`키를 `master key`로 만들게 되고 이 master key를 이용하여 `session key` 만듦. 이후 데이터를 주고 받을 떄 `session key`를 `대칭키 방식`으로 이용하여 통신.
7. 통신이 끝나면 세션키를 폐기.