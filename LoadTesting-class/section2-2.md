### ✅ EC2가 수집하는 기본 지표들

EC2 인스턴스를 생성하면 기본적으로 수집하는 지표들이 있다.

- CPU 사용률 (CPUUtilization)
- 네트워크 사용률(NetworkIn, NetworkOut)
- 디스크 성능(DiskReadOps, DiskWriteOps)
- 디스크 읽기/쓰기(DiskReadBytes, DiskWriteBytes)
- etc

![2-2.png](/images/2-2.png)

단순히 인스턴스가 멈추지 않고 가동되고 있는지를 확인하기에는 위 지표를 보는 것만으로 충분할 수도 있다.

하지만 확실한 모니터링을 위해서는 메모리(Memory)도 반드시 모니터링해야 한다. **CloudWatch Agent**를 인스턴스에 설치하고 구성 파일을 설정한 후 실행하면 메모리를 포함한 더 많은 지표를 수집할 수 있다.

### ✅ EC2에 Cloudwatch Agent 설치하기

1. **IAM Role 생성**
    
    ![2-3.png](/images/2-3.png)
    
    ![2-4.png](/images/2-4.png)
    
2. **정책 선택하기**
    
    ![2-5.png](/images/2-5.png)
    
3. **역할 이름 설정하기**
    
    ![2-6.png](/images/2-6.png)
    
4. **EC2 인스턴스에서 IAM Role 연결**
    
    ![2-7.png](/images/2-7.png)
    
5. **EC2 인스턴스로 접속해 Cloudwatch Agent 설치하기**

```bash
#Ubuntu x86-64 Cloudwatch Agent 다운로드
$ wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

# 다운받은 패키지 설치
$ sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```

1. **CloudWatch Agent 설정 파일 생성하기**

```bash
$ sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

1. CloudWatch Agent가 잘 실행되고 있는지 확인
    
    ```bash
    $ sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
    ```
    
2. CloudWatch로 가서 메트릭이 잘 수집되고 있는지 확인하기
    
    ![2-8.png](/images/2-8.png)
    

## ✅ ELB의 CPU, 메모리를 체크하지 않는 이유

AWS에서 ELB를 만들 때 1초당 수백만 개의 요청을 처리할 수 있게끔 고사양 컴퓨터를 여러 대 세팅해뒀다. 다른 말로 표현하자면, ELB 때문에 성능 저하가 일어날 가능성은 아주 적으니 신경쓰지 않아도 된다는 뜻이다. 이러한 이유 때문에 생성한 ELB(ALB)의 모니터링 지표에는 CPU, 메모리에 대한 정보가 ❌

![2-9.png](/images/2-9.png)

### ✅ 각 서버의 CPU, 메모리를 한  눈에 볼 수 있도록 세팅하기

강의 교안 참조 - 쓰기 귀찮
