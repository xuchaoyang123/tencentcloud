## cvm 操作

```bash
pip install tencentcloud-sdk-python

```

### 1. 查询cvm 

```py
import json
from tencentcloud.common import credential
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
from tencentcloud.cvm.v20170312 import cvm_client, models
try:
    cred = credential.Credential("$secretId", "$secretKey")
    client = cvm_client.CvmClient(cred, "ap-nanjing")
    req = models.DescribeInstancesRequest()
    resp = client.DescribeInstances(req)
    t = resp.to_json_string()
    a = json.loads(t)  
    #print("InstanceName|InstanceId|CPU|Memory|PrivateIpAddresses|PublicIpAddresses")  
    for x in a["InstanceSet"]:
            #print(str(x))
            print("|"+str(x["InstanceName"]),"|"+str(x["InstanceId"]),"|"+str(x["CPU"])+"C",""+str(x["Memory"])+"G","|"+str(x["PrivateIpAddresses"][0]),"|"+str(x["PublicIpAddresses"][0]),"|"+str(x["SystemDisk"]["DiskSize"])+"GB","|"+str(x["InstanceState"]))
except TencentCloudSDKException as err:
    print(err)

#输出结果
#|设置你想要的名字 |ins-ivbhyiw8 |1C 2G |10.206.0.14 |118.195.xx.xx |100GB |RUNNING

  
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
   
        "InstanceChargeType": "SPOTPAID",#PREPAID:预付费,即包年包月,POSTPAID_BY_HOUR:按小时后付费,CDHPAID：独享子机（基于专用宿主机创建，宿主机部分的资源不收费,SPOTPAID：竞价付费,默认值：POSTPAID_BY_HOUR。
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
        "InstanceType": "S5.SMALL2",                               #设置实例规格(几核几G) SMALL1:1核1GB,S5.SMALL2:1核2GB,S5.SMALL4:1核4GB,S5.MEDIUM4:2核4GB,S5.MEDIUM8:2核8GB,S5.LARGE8:4核8GB,S5.LARGE16:4核16GB,S5.2XLARGE16:8核16GB
         
        "ImageId": "img-oikl1tzv",
        "SystemDisk": {
            "DiskSize": 100,                                    #设置磁盘大小.
            "DiskType": "CLOUD_PREMIUM"
        },
        "InternetAccessible": {
            "InternetMaxBandwidthOut": 98,                      #设置启动时带宽大小.
            "PublicIpAssigned": True,
            "InternetChargeType": "TRAFFIC_POSTPAID_BY_HOUR"
        },
        "InstanceName": "设置你想要的名字",                        #设置实例名称
        "LoginSettings": {
            "Password": "设置cvm的密码"                           #设置登录密码
        },
        "SecurityGroupIds": [ "sg-k7s7zjnq" ],                  #这个安全组提前要创建出来
        "InstanceCount": 1,                                     #设置启动几个实例.
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
#{"InstanceIdSet": ["ins-b7lh9jt6"], "RequestId": "c32988b6-fafa-4b4c-acc9-2fedaeed89cd"}


```


### 3. 批量删除cvm
```py


# 删除
import json
from tencentcloud.common import credential
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
from tencentcloud.cvm.v20170312 import cvm_client, models
try:
    cred = credential.Credential( "$secretId", "$secretKey") 
       
    httpProfile = HttpProfile()
    httpProfile.endpoint = "cvm.tencentcloudapi.com"

    clientProfile = ClientProfile()
    clientProfile.httpProfile = httpProfile
    client = cvm_client.CvmClient(cred, "ap-nanjing", clientProfile)

# 将现有的cvm id加入到列表[]中
    query1 = []
    query = models.DescribeInstancesRequest()
    resp = client.DescribeInstances(query)
    t = resp.to_json_string()
    a = json.loads(t)
    for x in a["InstanceSet"]:
        query1.append(str(x["InstanceId"]))

# 进行批量删除
    req = models.TerminateInstancesRequest()
    params = {
        # 一个或多个待操作的实例ID。可通过DescribeInstances接口返回值中的InstanceId获取。每次请求批量实例的上限为100。
        "InstanceIds": query1

    }
    req.from_json_string(json.dumps(params))

    resp = client.TerminateInstances(req)
    #print(resp.to_json_string())
    print("cvm: ", query1, "已删除成功,共计: ", len(query1))

except TencentCloudSDKException as err:
    print(err)


#结果输出:
#cvm:  ['ins-r2sxub5s', 'ins-pgjeh5c6'] 已删除成功,共计:  2
```



### 4.查询余额
```py

import json
from tencentcloud.common import credential
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
from tencentcloud.billing.v20180709 import billing_client, models


def GetAccout(ak, aks):
    try:
        cred = credential.Credential(ak, aks)
        httpProfile = HttpProfile()
        httpProfile.endpoint = "billing.tencentcloudapi.com"

        clientProfile = ClientProfile()
        clientProfile.httpProfile = httpProfile
        client = billing_client.BillingClient(cred, "", clientProfile)

        req = models.DescribeAccountBalanceRequest()
        params = {

        }
        req.from_json_string(json.dumps(params))

        resp = client.DescribeAccountBalance(req)
        print("腾讯云账户余额: ", resp.__dict__["Balance"]/float(100), "元")

    except TencentCloudSDKException as err:
        print(err)


GetAccout("$ak",
          "$sk")
         
         
#输出结果: 
#腾讯云账户余额:  113.53 元
```
