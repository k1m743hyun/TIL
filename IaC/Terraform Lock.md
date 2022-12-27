# Terraform Lock


## 발생 원인
- Terraform은 여러 사용자가 동시에 수정하는 것을 방지하기 위해 Lock을 걸어 동시성 제어를 함
- Terraform으로 명령(plan 또는 apply)을 실행하다가 실패하게 되면 Lock이 걸린 상태로 끝남
- 이때 다시 Terraform 명령을 실행하면 아래와 같은 에러 메세지가 나타남

```
│ Error: Error acquiring the state lock
│ 
│ Error message: ConditionalCheckFailedException: The conditional request failed
│ Lock Info:
│   ID:        ebf8c0ed-c32e-84e3-797b-4655cf7036b7
│   Path:      terraform/terraform.tfstate
│   Operation: OperationTypeApply
│   Who:       kimtaehyun-ui-MacBook-Pro.local
│   Version:   1.2.2
│   Created:   2022-12-27 03:02:45.317382 +0000 UTC
│   Info:      
│ 
│ 
│ Terraform acquires a state lock to protect the state from being written
│ by multiple users at the same time. Please resolve the issue above and try
│ again. For most commands, you can disable locking with the "-lock=false"
│ flag, but this is not recommended.
```


## 해결 방안


### 1. `-lock=false`
- Lock을 무시하고 명령을 계속해서 실행하는 방법
- Terrafom에서 권장하지 않는 방법

```
$ terraform apply -lock=false
```


### 2. `terraform force-unlock <Lock ID>`
- Lock을 수동으로 강제로 풀어주는 방법
- 이 방법으로 Lock을 해제 후 명령 실행

```
$ terraform force-unlock ebf8c0ed-c32e-84e3-797b-4655cf7036b7
$ terraform plan
```


# 출처
- [terraform lock 해결방안](https://sarc.io/index.php/cloud/2127-terraform-lock)