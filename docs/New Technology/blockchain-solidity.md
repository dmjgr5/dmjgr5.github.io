---
layout: default
title: 리믹스(Remix) 도구를 이용한 Solidity 이해하기
parent: Blockchain
grand_parent: New Technology
permalink: /docs/newtech/blockchain-solidity/
nav_order: 11
---

# 리믹스(Remix) 도구를 이용한 Solidity 이해하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

 


![example](/assets/images/solidity0.png)

{: .note }
Remix 는 스마트 계약 개발을 위한 솔리디티 언어를 이용하여 계약의 제작, 개발, 배포 등을 할 수 있는 통합 개발 환경으로서 이전에는 `browser-solidity `라고 불려졌습니다. 현재는 별도 설치 없이 브라우저 환경에서 손쉽게 개발환경을 구축할 수 있으며, 간단 예제를 통해 솔리디티 개발 환경을 이해하고자 합니다.
 

### 1. 리믹스 접속 


아래 주소를 접속하여 리믹스에 접근합니다.

- [https://remix.ethereum.org/](https://remix.ethereum.org/)



### 2. 개발 환경 접속 및 파일 생성
아래와 같이 개발 환경 화면을 확인할 수 있으며, 새 파일을 클릭하여 `HellowWorld.sol` 파일을 생성합니다.


![example](/assets/images/solidity1.png)

 


### 3. 예제 소스 작성

예제 소스를 작성합니다. 
소스 내용은 아래를 클릭하여 확인하여 주세요.

<details>
<summary>HellowWorld.sol</summary>
<div markdown="1">

```javascript
pragma solidity ^0.6.0; 

contract HelloWorld { 
    uint count;
    
    constructor() public {
        count = 0;
    }
    
    function getCount() public view returns(uint){
        return count;
    }
    
    function incrementCount() public{
        count = count + 1;
    }
}
```
</div>
</details>


![example](/assets/images/solidity2.png)


### 4. Compile

Compiler Version 을 소스의 버전과 맞추고 `Compile HellowWorld.sol` 을 클릭합니다.

![example](/assets/images/solidity3.png)

노란색 버튼(`ABI`, `Bytecode`) 를 복사한 후 메모장에서 확인하여 아래와 같은 내용을 확인할 수 있습니다. 


![example](/assets/images/solidity4.png)



### 5. Deploy

좌측 탭 메뉴 중 Deploy 메뉴로 이동하여 `Deployed Contracts` 를 클릭하면 현재 생성된 컨트랙트 내 함수들을 확인 할수 있습니다.

![example](/assets/images/solidity5.png)


### 6. Excute

`getCount` 를 클릭하면 `count` 변수를 초기화 했던 `0` 값을 확인할 수 있습니다.

![example](/assets/images/solidity6.png)

 `incrementCount` 클릭 후, `getCount` 를 클릭하면 `uint256` 값이 1 증가한 것을 확인할 수 있습니다. 


![example](/assets/images/solidity7.png)






