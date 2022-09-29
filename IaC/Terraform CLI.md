# Terraform CLI Commands


## init


### Usage
```
$ terraform init [options]
```

### Options
- `-input=true`
- `-lock=false`
- `-lock-timeout=<duration>`
- `-no-color`
- `-upgrade`


## plan


### Usage
```
$ terraform plan [options]
```


### Options
- `-refresh=false`
- `-replace=ADDRESS`
- `-target=ADDRESS`
    - 특정 리소스를 지정하는 옵션
- `-var 'NAME=VALUE'`
- `-var-file=FILENAME`


## apply


### Usage
```
$ terraform apply [options]
```


### Options
- `-auto-approve`
- `-compact-warnings`
- `-input=false`
- `-json`
- `-lock=false`
- `-lock-timeout=DURATION`
- `-no-color`
- `-parallelism=n`
- `-target=ADDRESS`
    - 특정 리소스만 지정하는 옵션


## destroy


### Usage
```
$ terraform destroy [options]
```


### Options
- `-target=ADDRESS`
    - 특정 리소스를 지정하는 옵션