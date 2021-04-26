## cvm 操作

```bash
pip install tencentcloud-sdk-python

```

### 1. 查询cvm 

```py
from tencentcloud.common import credential
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
from tencentcloud.cvm.v20170312 import cvm_client, models
try:
    cred = credential.Credential("secretId", "secretKey")
    client = cvm_client.CvmClient(cred, "ap-shanghai")
    req = models.DescribeInstancesRequest()
    resp = client.DescribeInstances(req)
    print(resp.to_json_string())
except TencentCloudSDKException as err:
    print(err)
```

### 2. 创建cvm
```py
import json
from tencentcloud.common import credential
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
from tencentcloud.cvm.v20170312 import cvm_client, models
try: 
    cred = credential.Credential("$secretId", "$secretKey") 
    httpProfile = HttpProfile()
    httpProfile.endpoint = "cvm.ap-nanjing.tencentcloudapi.com"

    clientProfile = ClientProfile()
    clientProfile.httpProfile = httpProfile
    client = cvm_client.CvmClient(cred, "ap-nanjing", clientProfile) 

    req = models.RunInstancesRequest()
    params = {
   
        "InstanceChargeType": "POSTPAID_BY_HOUR",#PREPAID:预付费,即包年包月,POSTPAID_BY_HOUR:按小时后付费,CDHPAID：独享子机（基于专用宿主机创建，宿主机部分的资源不收费,SPOTPAID：竞价付费,默认值：POSTPAID_BY_HOUR。
        "Placement": {
            "Zone": "ap-nanjing-1",
            "ProjectId": 0
        },
        "VirtualPrivateCloud": {
            "AsVpcGateway": False,
            "VpcId": "vpc-1mvihmrh",
            "SubnetId": "subnet-punhzgeg",
            "Ipv6AddressCount": 0
        },
        "InstanceType": "SMALL1",                 #设置实例规格(几核几G) SMALL1:1核1GB,S5.SMALL2:1核2GB,S5.SMALL4:1核4GB,S5.MEDIUM4:2核4GB,S5.MEDIUM8:2核8GB,S5.LARGE8:4核8GB,S5.LARGE16:4核16GB,S5.2XLARGE16:8核16GB
         
        "ImageId": "img-oikl1tzv",
        "SystemDisk": {
            "DiskSize": 100,                             #设置磁盘大小.
            "DiskType": "CLOUD_PREMIUM"
        },
        "InternetAccessible": {
            "InternetMaxBandwidthOut": 98,                  #设置启动时带宽大小.
            "PublicIpAssigned": True,
            "InternetChargeType": "TRAFFIC_POSTPAID_BY_HOUR"
        },
        "InstanceName": "设置你想要的名字",                           #设置实例名称
        "LoginSettings": {
            "Password": "设置cvm的密码"
        },
        "SecurityGroupIds": [ "sg-k7s7zjnq" ],                 #这个安全组提前要创建出来
        "InstanceCount": 1,                                    #设置启动几个实例.
        "EnhancedService": {
            "SecurityService": {
                "Enabled": True
            },
            "MonitorService": {
                "Enabled": True
            }
        }
    }
    req.from_json_string(json.dumps(params))

    resp = client.RunInstances(req) 
    print(resp.to_json_string(indent=2)) 

except TencentCloudSDKException as err: 
    print(err) 
 
#结果输出 
#{"InstanceIdSet": ["ins-b7lh9jt6", "ins-p0thg5og", "ins-51t9ncem", "ins-c70r2hc4", "ins-acipnqjy", "ins-qccblnk4", "ins-ddptrqhy", "ins-36k7wdk8"], "RequestId": "c32988b6-fafa-4b4c-acc9-2fedaeed89cd"}


```


### 3. 删除cvm
```py

import json
from tencentcloud.common import credential
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
from tencentcloud.cvm.v20170312 import cvm_client, models
try: 
    cred = credential.Credential("SecretId", "SecretKey") 
    httpProfile = HttpProfile()
    httpProfile.endpoint = "cvm.tencentcloudapi.com"

    clientProfile = ClientProfile()
    clientProfile.httpProfile = httpProfile
    client = cvm_client.CvmClient(cred, "ap-nanjing", clientProfile) 

    req = models.TerminateInstancesRequest()
    params = {
        "InstanceIds": [ "ins-ddptrqhy" ]                   #一个或多个待操作的实例ID。可通过DescribeInstances接口返回值中的InstanceId获取。每次请求批量实例的上限为100。
    }
    req.from_json_string(json.dumps(params))

    resp = client.TerminateInstances(req) 
    print(resp.to_json_string()) 

except TencentCloudSDKException as err: 
    print(err) 


```
