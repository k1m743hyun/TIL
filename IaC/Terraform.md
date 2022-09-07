# Terraform


## Terraform이란?
- Code를 통해 Infrastructure를 구성하고 관리할 수 있도록 해주는 Open Source Tool
- 기본적으로 HCL(HashiCorp Configuration Language)을 사용하여 Code를 작성할 수 있으며 Json으로도 가능함
- 작성된 코드를 통해 Infrastructure Resource들을 생성, 변경, 삭제를 할 수 있음


## Terraform 장점
- Open Source이고 IaC를 위한 도구로 많은 사람들이 이용하고 있기 때문에 문제가 생겼을 시 검색하여 쉽게 해결법을 찾을 수 있고, 다양한 이슈들에 대해서 활발히 수정 및 업데이트가 진행되고 있음
- Terraform Code를 Git으로 관리한다면 기존에는 하기 힘든 Infrastructure 구성 History를 관리할 수 있음
- Code로 관리되기 때문에 특정 Resource를 추가하기 위해 적용 전에 다른 사람들의 리뷰를 받을 수 있음
- 기존 Code를 재사용함으로 추가적으로 동일한 Resource를 만들 때 쉽고 빠르게 적용할 수 있음


## Terraform Directory Structure
- 관점에 따라 다르게 Directory Structure Strategy를 세울 수 있음


### [1] Environment 별로 나뉜 Directory Structure
- 상위 Directory를 Environment 별로 나누는 방법


![Separated Directory for Environment](./images/separated-directories-for-environment.png)


#### 장점
- 환경 별로 독립적이고 구분이 가능함
- 환경 별 계층을 세분화하여 커스터마이징할 수 있음
- 원하지 않는 환경에 리소스를 생성하는 실수를 줄일 수 있음


#### 단점
- 새로운 환경을 구성할 때 파일이 중복되는 것은 불가피함
- 하위 디렉토리 레벨이 깊어질 수 있음


### [2] Component 별로 나뉜 Directory Structure
- 상위 Directory를 Component 별로 나누는 방법


![Separated Directory for Component](./images/separated-directories-for-component.png)
![Separated Directory for Component](./images/separated-directories-for-component2.png)


#### 장점
- 반복되는 환경에서 확장성이 있음
- 단순함


#### 단점
- 원하지 않는 환경에 리소스를 생성하는 실수가 발생할 수 있음
- 환경 별 계층을 커스터마이징하는 것이 불분명함


# 출처
- [Terraform 이란?](https://velog.io/@gentledev10/what-is-the-terraform)
- [How to Create Terraform Multiple Environments](https://getbetterdevops.io/terraform-create-infrastructure-in-multiple-environments/)