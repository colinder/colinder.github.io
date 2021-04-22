# Block_Chain setting & basic


​	

# 🎈가상환경에서 블록체인 채굴 및 기본 시스템 정리

​	

## 1. geth(Go-ethereum) 설치

> 블록체인 기술은 중앙집중화된 데이터베이스에 반기를 들고 나온 개념이기 때문에 중앙 서버 이런게 없다. 즉 누구라도 해당 블록체인 네트워크에 참여하고 싶다면 언제라도 블록체인 데이터베이스를 싱크해주는 로컬 프로그램을 다운 받아 실행하면 데이터를 받을 수 있다.
>
> 그렇다면 블록체인 네트워크에 참여하고 싶다면 어떻게 해야 할까? 앞서 말한 로컬 프로그램을 다운 받으면 참여가 가능하다. 
>
> 이 프로그램들 중 하나인 GO 로 짜여진 Go Ethereum(줄여서, geth)을 [설치](https://geth.ethereum.org/downloads/)해본다.

<image src="/images/image-20200831143159580.png">

작성당시 가장 최신 버전인 1.9.20 for windows로 다운을 받는다. 

1. 다운 받고 실행하면 경고창이 뜨는데 `추가정보`누르고 `실행`누르고 진행한다.
2. devolop 뭐시기는 선택하지 않았고, next를 누루고 진행한다.
3. 설치가 깔끔히 마무리 되지 않고 경고창이 떴는데, 환경변수 설정해주었다. 
   
   - 검색 → 시스템 환경 변수 편집 → 고급 → 환경 변수 → Path더블클릭 →  새로만들기 → C:\Program Files\Geth 추가
   
   <image src="/images/image-20200831153211813.png" width="800px">



4. cmd 창을 열어(관리자: 명령 프롬프트) geth를 입력해 실행되는지 확인한다.________끝_

   ​	

## 2. Web3 설치

> 모든 자료와 정보가 분산화, 분권화된 차세대 네트워크 구조로서, 서버가 없는 혁신적인 인터넷 분산형 웹.
>
> web3.js는 JavaScript 기반으로 [Dapp](http://wiki.hash.kr/index.php/Dapp) 이나 서비스를 구현할 때 매우 유용. (출처: 위키)

​	

이미 Vue_CLI 프로젝트를 구성해 놓은 것이 있어서 

1. Terminal 에서 `npm install web3` 입력해서 web3를 설치한다.  

<image src="/images/image-20200831154822148.png" width="800px" caption="package.json 에서 web3가 설치된 것을 볼 수 있다. ">

​	

## 3. [VirtualBox](https://www.virtualbox.org/wiki/VirtualBox) 설치

> VirtualBox: 하드웨어를 소프트웨어적으로 구현해서 그 위에서 운영체제가 작동하도록하는 기술

- [Windows hosts](https://download.virtualbox.org/virtualbox/6.1.12/VirtualBox-6.1.12-139181-Win.exe) 와 [All supported platforms](https://download.virtualbox.org/virtualbox/6.1.12/Oracle_VM_VirtualBox_Extension_Pack-6.1.12.vbox-extpack) 두개를 다운 받는다.

  1. Windows hosts: 그냥 next연타하여 설치

  2. All supported platforms을 더블클릭하여 설치를 진행하는데



---

__👍전체 설치는 [이 블로그](https://webnautes.tistory.com/448)의 설치 동영상을 참고하여 설치를 진행__

​	

## 4. Vagrant 설치

> Vagrant는 가상머신을 편리하게 사용할 수 있도록 도와주는 프로그램입니다. 이미 누군가가 Vagrant로 설정해 놓은 **가상머신 Box**를 간단한 명령어로 손쉽게 설치 할 수 있으며 **가상 머신**과 **호스트 머신**과의 환경설정도 쉽게 할 수 있습니다. 다운로드는 [여기](https://www.vagrantup.com/downloads.html)서 진행.

​	

## 5. Remix IDE 설정

>이더리움 `스마트 컨트랙트` 프레임워크 

[여기](https://remix.ethereum.org/#optimize=false&evmVersion=null&version=soljson-v0.6.6+commit.6c089d02.js)서 진행!

​	

# 🎈본격적인 블록체인 관련 가상환경 세팅 및 채굴 진행

# 20.09.01 한 일..

---

#### ⛏ 광산에서 일하는 것을 예시로!

- **VirtualBox(🏞)**: 블록체인 생성 및 채굴을 위한 가상환경을 세팅 (내가 채굴할 산을 고르는 것)

- **Vagrant(🏟)**: 가상환경에서 노드를 생성하는 등, 블록체인 관련 프레임워크 (광구를 생성하는 것)

- **geth(📜👷‍♂️)**:  블록체인 노드의 관련 프로그램 (광구에 어떤 사람이 들어오고 어떤 사람을 파견할지 등등)

  ​	

1. 블록체인 관련 작업을 할 디렉토리 생성 후 Terminal을 열어 vagrant 관련 명령어 실행(이미 설치는 완료)

   ```bash
   > vagrant version      #설치 여부 및 버전 확인 => 버전 넘버가 보이면 ok
   ```

   ```bash
   > vagrant plugin install vagrant-scp		# 호스트 & 가상머신 간 연동 프로그램 설치
   ```

   ```bash
   > vagrant init		# 블록체인 채굴을 위한 기초 세팅 시작
   # 해당 명령어를 입력하면 작업하는 디렉토리에 Vagrantfile이 생긴다.
   ```

   - vagrant init 후 생기는 Vagrantfile을 수정해 최초의 블록(**genesis.json**)을 생성해야 한다.

   ```bash
   # Vagrantfile	=>  내용을 수정한다. 
   
   # -*- mode: ruby -*-
   # vi: set ft=ruby :
   
   # All Vagrant configuration is done below. The "2" in Vagrant.configure
   # configures the configuration version (we support older styles for
   # backwards compatibility). Please don't change it unless you know what
   # you're doing.
   
   
   VAGRANT_API_VERSION = "2"
   
   vms = {
     'eth0' => "10",
     'eth1' => "11"
   }
   
   Vagrant.configure(VAGRANT_API_VERSION) do |config|
     config.vm.box = "ubuntu/bionic64"
     vms.each do |key, value|
       config.vm.define "#{key}" do |node|
         node.vm.network "private_network", ip: "192.168.50.#{value}"
         if "#{key}" == "eth0"
           node.vm.network "forwarded_port", guest: 8545, host: 8545
         end
         node.vm.hostname = "#{key}"
         node.vm.provider "virtualbox" do |nodev|
           nodev.memory = 2048
         end
       end
     end
   end
   
   # 내가 이해?한 바로는 eth0과 eth1이라는 2개의 노드(광구)를 설정했다. 정도
   ```

   ```bash
   > vagrant up		# 방금 설정한 eth0과 eth1 노드(광구)를 오픈(실행)
   ```

   ```bash
   > vagrant status		# 설정되어 있는 두 노드(광구)의 상태를 확인 runing이면 오픈한 상태
   ```

   ​	

2. 여기 가지 작업을 했다면 vitualbox에 두개의 노드(광구)가 등록되어있는 것을 확인 할 수 있다. 

   <image src="/images/image-20200901204708869.png" width="800px">

   ​	

등록된 노두(광구) 중에서 eth0을 먼저 설정해보자

지금까지 진행한 폴더에서 Terminal을 열고 **vagrant** 명령어로 노드(광구)를 설정해준다.

___기본적은 vagrant 명령어___

| 명령어                                | 설명                   |
| :------------------------------------ | :--------------------- |
| vagrant up 상태확인할가상머신이름     | 가상머신 기동          |
| vagrant status 상태확인할가상머신이름 | 가상머신 상태 확인     |
| vagrant ssh 접속할가상머신이름        | 가상머신에 접속        |
| vagrant halt 멈출가상머신이름         | 가상머신 정지          |
| vagrant suspend                       | 가상머신 휴면          |
| vagrant resume                        | 가상머신 휴면에서 복원 |
| vagrant reload                        | 가상머신 재시동        |
| vagrant destroy                       | 가상머신 제거          |

*모든 명령어가  _vagrant up 상태확인할가상머신이름_ 과 같이 사용 가능하다.

​	

3. 이제 노드(광구)를 열었으니 접속해보자 =>  _vagrant ssh 접속할가상머신이름_ 

   <image src="/images/image-20200902223519522.png" width="800px">

   ​	

4. geth를 설치해준다.

   _*Geth 설치 (Go-ethereum client)_

   ```bash
   # vagrant@eth0:~$ 가상머신에서 수행
   
   > sudo apt-get update
   > sudo apt-get install software-properties-common
   > sudo add-apt-repository -y ppa:ethereum/ethereum
   > sudo apt-get install ethereum
   #여기까지 순서대로 설치하면 geth설치는 완료.
   
   # 프라이빗 이더리움를 관리할 폴더 생성
   > mkdir -p dev/eth_localdata		#-p는 mkdir 옵션사항 => 중간경로도 다 만들어라
   > mkdir database		
   > cd database
   
   # 프라이빗 이더리움을 위한 genesis.json블록파일(블록들을 연결하기 위한 최초의 블럭) 생성
   > vi genesis.json		# genesis.json를 만들겠다.
   ```

   ```bash
   # genesis.json의 내용 등록 (예시)
   i → 입력 활성화 → 아래 내용입력 후 → esc → ZZ(저장하고 종료)
   
   {
     "config": {
       "chainId": 5245,
       "homesteadBlock": 0,
       "eip150Block": 0,
       "eip155Block": 0,
       "eip158Block": 0
       },
     "nonce": "0xdeadbeefdeadbeef",
     "difficulty": "0x40",
     "gasLimit": "9999999",
     "alloc": {},
     "extraData": "",
     "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
     "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
     "timestamp": "0x00"
   }
   ```

   ```bash
   # genesis.json 저장 후 
   > cd ..
   다시 root폴더로 이동한다.
   ```

   ​	

5. 나의 최초의 블록(genesis.json)을 만들었다면 등록(초기화)해줘야 한다. => ✨geth를 사용하여 진행

   ```bash
   > geth init		#이 명령어로 등록(초기화)을 할 수 있는데...
   ```

   <image src="/images/image-20200903104621097.png" width="100%" caption="--datadir관련 공식문서 설명">

   

   ```bash
   # 위의 --datadir을 참고하여
   
   > geth --datadir ~/dev/eth_localdata init ~/database/genesis.json
   # ~/database 위치에 있는 genesis.json을 등록(초기화)하고 등록 후 생기는 관련 자료는 ~/dev/eth_localdata에 모아놓겠다는 뜻
   ```

   <image src="/images/image-20200917134852473.png" width="800px">

   ​		

6. 이제 노드(광구)를 관리할 계정(🙋‍♂️사람)도 등록해준다.

   ```bash
   > geth account new
   # 이 명령어를 입력하면 아이디는 자동으로 생성되고 비밀번호만 설정할 수 있게 진행된다.
   ```

   ```bash
   # 계정이 잘 만들어 진 것인지 확인하기 위해서는 
   > geth account list
   ```

   <image src="/images/image-20200917100306437.png" width="800px">

   ​		

7. 이제 노드(광구)에 어떤 사람들이 들어올 수 있는지와 입장하는 명령어들을 살펴보자.

   - _어떤식으로 입력(설정)하는 지는 [여기](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)를 참고하여 진행하였다._

   ```bash
   # eth0의 노드(광구) 입장 명령어
   > geth console
   
   # 다만 보통의 경우 아래와 같이 노드(eth0)에 다양한 주소 및 설정을 붙이고 진행한다. 
   > geth --networkid 5245 --port 30303 --maxpeers 2 --datadir ~/dev/eth_localdata --rpcaddr 0.0.0.0 --nodiscover --nousb console >> ~/eth0.log
   
   # 일단 이걸로 채굴 성공
   geth --datadir ~/dev/eth_localdata --nodiscover --nousb --networkid 5245 console
   
   # 맨 마지막에 2>> ~/eth0.log를 붙이면, 진행기록을 root주소에 eth0.log를 만들어 저장한다.란 뜻.
   ```

   ```bash
   # 진행 도중 ERROR 모음
      
   Failed to enumerate USB devices  => --nousb 추가해주면 사라짐.
   #if you want USB wallet support disabled. By default it's enabled.
      
   Looking for peers  => --nodiscover 추가해주면 사라짐
   # --nodiscover 옵션은 같은 제네시스 블록과 네트워크ID에 있는 노드들이 연결하는 것을 방지
   ```

   ​		

---

# 20.09.02 한 일..

> 어제까지 진행한 내용 보강 & geth 진입(광구) 후 명령어 정리
>
> 광구에 진입했으니까. 일할 사람👷‍♂️을 등록해주고, 채굴한 광물(coin)을 저장할 곳🛒(coinbase) 등 추가 등록이 필요하다. 

1. **먼저 일할 사람👷‍♂️(EOS 계정)을 등록해주자.**

   _✋ 여기서 잠깐.!_

   > EOS란?
   >
   > **이오스**(EOS)는 [위임지분증명](http://wiki.hash.kr/index.php/위임지분증명)(DPoS) 방식을 사용하는 제3세대 암호화폐. 이오스는 [이더리움](http://wiki.hash.kr/index.php/이더리움)의 느린 처리 속도와 높은 수수료 문제를 해결하기 위한 대안으로 등장하였다. 이오스는 분산 애플리케이션인 [디앱](http://wiki.hash.kr/index.php/디앱)(DApp)을 구동할 수 있는 플랫폼을 제공함으로써 범용적인 블록체인 운영체제(OS)를 만드는 것을 목표로 하는 프로젝트.

   ```bash
   personal.newAccount("eth")
   #처음 등록된 일할 사람👷‍♂️은 자동으로 저장할 곳🛒(coinbase) 으로 지정된다.
   #나는 계정을 2개 만들어서 진행한다.
   ```

   | personal명령어             | 의미                                            |
   | :------------------------- | ----------------------------------------------- |
   | personal.newAccount("eth") | eth를 비밀번호로 하는 계정 생성 (keystore 추가) |
   | personal.listAccounts      | 등록된 계정 리스트 확인                         |

   ​	

2. **그럼 이제 채굴해보자**

   ```bash
   miner.start(1)		=> #코인 1개 채굴할때까지 진행
   ```

   | miner(채굴관련) 명령어 | 의미                                       |
   | ---------------------- | ------------------------------------------ |
   | miner.start()          | ()안에 숫자 만큼 채굴. 비워두면 계속 채굴. |
   | miner.stop()           | 채굴 중단                                  |
   | miner.setEtherbase()   | 채굴 보상을 줄 계정을 등록                 |

   <image src="/images/image-20200903171250312.png" width="800px">

   _정상적으로 채굴이 되면 Successfully sealed new block 까지 확인할 수 있다._

   ​	

   이외에 eth안에서 사용하는 여러 명령어를 정리한다. 

   | eth명령어                                                  | 의미                               |
   | ---------------------------------------------------------- | :--------------------------------- |
   | eth.accounts (personal.listAccounts와 동일)                | 현재 생성된 총 계정 확인           |
   | eth.blockNumber                                            | 현재 채굴된 블록의 번호            |
   | eth.hashrate                                               | 해시레이트(진행속도?를 알 수 있음) |
   | eth.coinbase                                               | 채굴이 진행되면 보상을 받을 계정.  |
   | eth.mining                                                 | 현재 채굴중인지 여부               |
   | eth.getBalance(account[0])                                 | 계좌 잔고 조회                     |
   | eth.getTranaction()                                        | ()안에 숫자번호의 트랜잭션의 정보  |
   | eth.sendTransaction({from:account, to:account, value:wei}) | 트랜잭션 보내기                    |
   | eth.pendingTransactions                                    | 처리 대기 중인 트랜잭션            |

   _*이것 말고도 명령어가 더 있을 수 있으니 필요한 것은 찾아보자._

   

   ​		

---

# 20.09.03 한 일..

> 어제까지 진행한 내용 보강 & 실습 진행

​	

#### ✨public blockchain과 private blockchain의 차이

>- public blockchain은 전세계 사람 어느 누구든 blockchain에 접근하여 data를 읽을 수도 있고 transaction을 날릴 수도 있다. 또한 채굴(consensus process)에 참여할 수도 있다.
>- private blockchain은 개인 또는 하나의 조직이 blockchain에 대한 접근을 관리하는 것을 말한다. 이로 인해 private network에 참여하기 위해서는 해당 관리자의 허락이 필요하다.

​	

채굴이 잘 되었는지 확인해보자.

```bash
eth.getBalance(eth.accounts[0])
eth.getBalance(eth.accounts[1])

# 이와 같은 방법으로 EOS계정별로 확인
```

<image src="/images/image-20200903173959675.png" width="1000px">

​	

이를 web3을 이용해 잔고단위를 바꿔 확인해보자

```bash
web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
web3.fromWei(eth.getBalance(eth.accounts[1]), "ether")
```

<image src="/images/image-20200903173949119.png" width="1000px">

​	

이제 EOS계정 간에 코인들을 송금해줘보자 => 이를 ___트랜잭션(Transaction)___ 한다고 한다.

현재 EOS계정이 2개가 있다. (personal.listAccounts로 확인 가능)

<image src="/images/image-20200903174038187.png" width="1000px">

​	

기억해야 할 점은 계정들이 리스트로 담겨 있으며, 이는 

<image src="/images/image-20200903174049940.png" width="1000px">

이런 식으로 확인(표현)이 가능하다. 

​	

### Transaction

___eth0 안에 account[0] 이랑 account[1] 이 있고 coinbase는 accounts[0] 인 상황에서 account[0]에서 채굴한걸 account[1]로 보낼 것이다.___  => 이를 ***트랜잭션(Transaction)*** 한다고 한다.

트랜잭션을 위해서는 먼저 계정들의 잠금 해제가 필요하다. EOS계정을 만들면 기본적으로 잠금 상태인데, 

```bash
# 잠금해제 command 로 해제가 가능하다. (이더를 보내는 EOS계정만 잠금을 풀어주면 된다.)

personal.unlockAccount(eth.accounts[0])
#기본적인 세팅, 입력 후 비밀번호 입력 계정 잠금 해제(잠금 해제 유효시간 기본 - 300초)

personal.unlockAccount(eth.accounts[0], "inputpassword")
#비밀번호가 입력되어 계정 잠금 해제(비밀번호 입력 필요X)

personal.unlockAccount(eth.accounts[0], "inputpassword", 0)
#계정 잠금 해제(0 - Geth 프로세스 종료되기 전까지 해제)
```

​	

이제 트랜잭션이 있는지 확인해보자. (eth.pendingTransactions => 트랜잭션 확인) 

<image src="/images/image-20200903174107273.png" width="1000px">

_입력한 transaction이 없으니 비어있다._

​	

이제 __송금(Transaction)__ 해보자.

```bash
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(10, "ether")})

# eth.accounts[0]에서 eth.accounts[1]로 web3단위로 10이더를 보내겠다.는 뜻.
```

<image src="/images/image-20200903174351993.png" width="1000px">

_색이 이쁘니 성공적으로 명령을 처리 한 것 같다._

<image src="/images/image-20200903174509460.png" width="1000px">

*하지만 accounts[1]계정에 돈이 들어오지 않았다.*

​	

트랜잭션은 등록된게 확인된다. 왜 진행은 안됬을까....

<image src="/images/image-20200903174652355.png" width="1000px">

*하지만 accounts[1]계정에 돈이 들어오지 않았다.*

_`*blockNumber: null`로 accounts[0]에서 보낸 트랜잭션이 accounts[1]의 블록에 포함되지 않음을 알 수 있다._

​	

**송금 확인(sendTransaction으로 트랜잭션을 발행해도 처리안됨 => 블록체인에서는 블록 안에 그 트랜잭션이 포함될 때 트랜잭션 내용이 실행)**

**✨ 간단히 트랜잭션을 등록하고 miner.start() command로 채굴을 시작하는 경우(블록 활성화?)에 트랜잭션이 반영된다!**

```bash
> miner.start(1)				# 이걸로 잠시 채굴을 켰다가.
> miner.stop()					# 으로 채굴을 꺼주고
> eth.pendingTransactions		# 으로 트랜잭션 내용을 다시 확인해주면 비어있는 것을 볼 수 있다.
```

<image src="/images/image-20200903174935650.png" width="1000px">

<image src="/images/image-20200903175110061.png" width="1000px">

​		

그리고 

```bash
# EOS계정(accounts[1])의 잔액을 확인해보면

> eth.getBalance(eth.accounts[1])
> web3.fromWei(eth.getBalance(eth.accounts[1]), "ether")
#둘다 확인 가능하며 송금이 잘 진행된 것을 볼 수 있다. 
```

<image src="/images/image-20200903175325622.png" width="1000px">

<image src="/images/image-20200903175350537.png" width="1000px">



---

여기까지 진행하면서, 두 블로그를 매우 많이 참고했고, 도움이 되었어서 소개한다.

[Mr.Latte](https://www.mrlatte.net/research/2018/02/27/ethereum-geth.html) - [Go Ethereum 사용 법]

[Allg](https://allg.tistory.com/44) - [Geth 설치 및 테스트 네트워크에서 송금하기]

---
