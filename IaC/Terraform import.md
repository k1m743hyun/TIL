# Terraform Import


## Terraform State
- Terraform이 Cloud 리소스를 관리할 때 사용하는 개념이 두 가지가 있음
    - 설정(configuration): `.tf`파일
    - 상태(state): `.tfstate`파일
- Terraform Code(`xxx.tf`)에 정의한 리소스를 `terraform plan`으로 비교하고 `terraform apply`로 적용하여 실제 Cloud 환경의 리소스를 생성, 변경한 내용을 Terraform State(`terraform.tfstate`) 파일에 기록함
- Terraform State가 처음 기록된 이후 부터는 Terraform Code와 Terraform State를 비교하여 어떤 리소스를 추가, 변경, 삭제할 지를 결정하여 Cloud의 리소스를 Terraform으로 관리할 수 있음


## Terraform Import
- 만약 Terraform Code로 생성, 변경, 삭제하지 않고 Cloud의 Web Console로 생성, 변경, 삭제한 경우, 이 부분에 대한 내용을 Terraform State에 반영을 해야 Terraform에서 알 수 있음
- 이때 사용하는 명령어가 `terraform import`
- `terraform import`는 현재 상태(state)에 대해서만 가져올 수 있고 설정(configuration)은 만들어주지 않음

```
$ terraform import 'module.ec2.aws_instance.this["web"]' i-12345678
```

### AWS Security Group Rule을 Import 할 경우
- `security_group_id`, `type`, `protocol`, `from_port`, `to_port`, `cidr_block`을 underscore(`_`)로 연결하여야 함

```
$ terraform import "module.rds.aws_security_group_rule.ingress[\"https\"]" sg-000000000_ingress_tcp_443_443_000.000.000.000/32
```


# 출처
- [기존에 사용 중인 인프라를 Terraform으로 가져오기](https://blog.outsider.ne.kr/1292)
- [Resource: aws_security_group_rule](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule.html)