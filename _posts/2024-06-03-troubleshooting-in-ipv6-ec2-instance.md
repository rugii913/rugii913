---
title: AWS EC2 인스턴스에서 IPv6 이용 통신이 가능한가?
author: rugii913
date: 2024-06-03 13:50:00 +0900
categories: [트러블슈팅]
tags: [troubleshooting, AWS]
render_with_liquid: false
---

## 왜 IPv6를 이용해 통신하려 했는가?
- 과금을 조금이라도 줄여보고자...
  - AWS에서 public IPv4 사용 시 과금 발생
  - free tier EC2 인스턴스의 free tier 시간 동안은 public IPv4 사용에도 과금되지 않으나,
  - 현재 내가 구성하고 있는 시스템에서 ALB가 2개의 public IPv4를 사용하고 있으며(ALB는 최소 2개의 subnet에 연결되어야 함) RDS에도 public IPv4가 있었던 상태
- cf. private IPv4 사용은 과금되지 않음

## IPv6 적용 방법
1) VPC에서 Edit CIDRs → Add new IPv6 CIDR → 적절히 추가
2) 개별 subnet에서 Edit IPv6 CIDRs → Add IPv6 CIDR → 적절히 추가
3) (필수인지 확실하지 않음) Rout tables → Edit routes → Destination ::/0, Target은 VPC의 internet gateway로 하는 route 추가
4) 개별 서비스(EC2 인스턴스, ELB 등)에서 IPv6 설정(ex. EC2 Instance의 Manage IP addresses 메뉴) - 이 때 public IP 자동 할당 옵션을 꺼주면, public IPv4는 할당되지 않음

## 겪은 문제들
1) IPv6를 사용하는 인스턴스에서 인터넷에 연결 불가능 → 해결
- EC2 인스턴스를 위한 endpoint를 구성하고, 이 endpoint을 이용해 인스턴스에 ssh 접속까지 했음
- 그런데 apt-get update가 불가능 → 인터넷 연결이 안 되는 것으로 판단하긴 했으나, 해결 방법을 찾아 한참 헤맴
2) ALB 및 인스턴스에서 IPv6를 사용하자 ERR_NAME_NOT_RESOLVED 오류 발생 → 미해결
- 아래와 같은 케이스들에 대해 시도해보았으나, (4) ALB Dualstack 사용, 인스턴스 IPv4 사용 경우 외에는 모두 오류 발생
  - (1) ALB Dualstack without public IPv4 사용, 인스턴스 IPv6 사용
  - (2) ALB Dualstack without public IPv4 사용, 인스턴스 IPv4 사용
  - (3) ALB Dualstack 사용, 인스턴스 IPv6 사용
  - (4) ALB Dualstack 사용, 인스턴스 IPv4 사용

## IPv6를 사용하는 인스턴스에서 인터넷에 연결 불가능한 문제
- public IP를 열어주면 되는 간단한 문제였음
  - IPv6는 알아서 public IP로 잡힘
  - endpoint를 열어줘서 ssh 통신이 가능하더라도, 인스턴스가 인터넷으로 다른 서버에 요청을 보내려면 public IP가 필요
- 이 때 Destination ::/0, Target은 VPC의 internet gateway로 하는 route를 추가했는데, 이게 꼭 필요했는지는 모르겠음
  - 실험은 간단할 것 같지만 귀찮아서 시도하지 않음
  - 이 route를 추가한다고 해서 특별한 보안 문제가 발생할 것 같진 않으므로 별 생각 없이 추가해둬도 문제가 없을 듯함
- 참고 사항
  - endpoint의 security group 규칙에는 ssh 관련 사항이 포함될 필요가 없음
    - instance를 향해 outbound 규칙만 잘 설정되어있으면 됨
    - 심지어 ssh 연결하고자 하는 호스트의 IP에 대한 inbound 규칙도 필요 없음
  - 인스턴스의 security group 규칙에는 endpoint로부터의 inbound 규칙만 잘 설정되어 있으면 됨
    - ssh 연결하고자 하는 호스트의 IP에 대해서 열려있을 필요는 없음
- 참고 링크
  - 특별히 결정적인 도움을 준 링크는 없었음
  - https://stackoverflow.com/questions/77999126/getting-internet-access-to-ec2-instance-in-a-public-subnet-without-having-a-publ
  - https://stackoverflow.com/questions/40287706/access-internet-from-aws-vpc-instance-without-public-ip-address
  - https://medium.com/@rajeshkanumurudevops/ec2-instance-endpoint-connect-with-internet-access-79e5dadd5a8c
  - https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_Internet_Gateway.html

## ALB 및 인스턴스에서 IPv6를 사용하자 ERR_NAME_NOT_RESOLVED 오류 발생하는 문제
- ALB와 인스턴스 둘 중 어느 쪽 하나라도 IPv6만으로 통신하게 되어있으면 오류 발생함
  - ALB에서 Dualstack without public IPv4를 사용할 경우 ERR_NAME_NOT_RESOLVED 발생
  - 인스턴스에서 IPv6 사용할 경우, 데이터는 불러와지는데 소셜 로그인 창으로 연결이 안 되는 듯함
    - 정확하진 않음
- DNS, AAAA Record 등의 문제일 수도 있고, 현재 내가 클라이언트로서 이용하는 통신이 IPv6를 이용하지 못하기 때문일 수 있음
  - 내 통신 회선의 문제이더라도, 다른 사람도 충분히 겪을 수 있는 문제이므로 더 이상의 진행은 불가능하다고 판단함
- IPv4 클라이언트에서 IPv6 전용 서버로 통신이 가능한지 살펴봤는데, 아마도 평범한 방식으로는 불가능하고 특별한 기술이 필요한 듯함
- 일단 RDS에서는 public IPv4를 회수했고, 통신에는 문제가 없는 상태임
- 참고 링크
  - 문제를 해결해주진 못했지만, 나름 도움이 됐던 링크들
  - [IPv4에서 IPv6로의 전환 매커니즘](https://mydailylogs.tistory.com/127)
  - [AWS public IP 요금 부과에 따른 대처 및 EC2를 이용한 RDS 외부 연결하기](https://inchan.dev/posts/202403051335/)
  - [AWS IPv4 과금과 VPC](https://velog.io/@goinggoing/AWS-IPv4-%EA%B3%BC%EA%B8%88%EA%B3%BC-VPC)
  - https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_Tutorials.CreateVPCDualStack.html
