##cvm 操作

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
        "InstanceChargeType": "POSTPAID_BY_HOUR",#（POSTPAID_BY_HOUR:按时收费,SPOTPAID:竞价实例)
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
        "InstanceType": "S5.2XLARGE16",
        "ImageId": "img-oikl1tzv",
        "SystemDisk": {
            "DiskSize": 100,
            "DiskType": "CLOUD_PREMIUM"
        },
        "InternetAccessible": {
            "InternetMaxBandwidthOut": 98,
            "PublicIpAssigned": True,
            "InternetChargeType": "TRAFFIC_POSTPAID_BY_HOUR"
        },
        "InstanceName": "redis-",
        "LoginSettings": {
            "Password": "设置cvm的密码"
        },
        "SecurityGroupIds": [ "sg-k7s7zjnq" ],
        "InstanceCount": 8,
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
    print(resp.to_json_string()) 

except TencentCloudSDKException as err: 
    print(err) 
 
#结果输出 
#{"InstanceIdSet": ["ins-b7lh9jt6", "ins-p0thg5og", "ins-51t9ncem", "ins-c70r2hc4", "ins-acipnqjy", "ins-qccblnk4", "ins-ddptrqhy", "ins-36k7wdk8"], "RequestId": "c32988b6-fafa-4b4c-acc9-2fedaeed89cd"}


```


### 3. 删除cvm
```py


```
