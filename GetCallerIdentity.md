## 接口描述
/**
 本实例介绍如何使用阿里云Java SDK调用安全令牌 STS的 GetCallerIdentity 接口获取当前调用者的身份信息
 GetCallerIdentity_操作接口_API 参考（STS）_API参考_访问控制-阿里云 https://help.aliyun.com/document_detail/43767.html?spm=a2c4g.11186623.6.683.xsDo62 阿里云STS (Security Token Service) 是为阿里云账号（或RAM用户）提供短期访问权限管理的云服务。
 通过STS，您可以为联盟用户（您的本地账号系统所管理的用户）颁发一个自定义时效和访问权限的访问凭证。
 联盟用户可以使用STS短期访问凭证直接调用阿里云服务API，或登录阿里云管理控制台操作被授权访问的资源。

 在创建临时身份之前，您需要获取以下信息：
 1)您已经开通了阿里云RAM服务，并为主账号或者RAM用户创建了Access Key ID 和Access Key Secret。
 对应访问控制-用户管理中配置用户（注意：用户AK只提供唯一的下载的机会，务必及时保管）
 对应访问控制-角色管理中配置角色（注意：用户存在不同的角色使用权）
 2）maven项目引入相关依赖
 <!-- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-core -->
 <dependency>
 <groupId>com.aliyun</groupId>
 <artifactId>aliyun-java-sdk-core</artifactId>
 <version>4.0.3</version>
 </dependency>

 <!-- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-sts -->
 <dependency>
 <groupId>com.aliyun</groupId>
 <artifactId>aliyun-java-sdk-sts</artifactId>
 <version>3.0.0</version>
 </dependency>

 */

## 实例代码
```
package sts;

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.sts.model.v20150401.GetCallerIdentityRequest;
import com.aliyuncs.sts.model.v20150401.GetCallerIdentityResponse;
public class GetCallerIdentityDemo {
    // 您的可用区ID，
    // 服务地址_附录_API 参考（STS）_API参考_访问控制-阿里云 https://help.aliyun.com/document_detail/66053.html?spm=a2c4g.11186623.6.689.6NvQAN
    private static final String REGION_ID = "sts.aliyuncs.com";
    // 只有 子账号才能调用 AssumeRole接口
    // 阿里云主账号的AccessKeys不能用于发起AssumeRole请求
    // 请首先在RAM控制台创建子用户，并为这个用户创建AccessKeys
//    private static final String ACCESS_KEY_ID  = "access-key-id>";
//    private static final String ACCESS_KEY_SECRET = "<access-key-secret>";
    private static final String ACCESS_KEY_ID  = "LTAIYXWaYd2g0u2C";
    private static final String ACCESS_KEY_SECRET = "EY9HGITqAdhxxlq8FboPTNsO7bYSlM";
    public static void main(String[] args) {
        // 创建 DefaultProfileAcsClient并实例化
        DefaultProfile profile = DefaultProfile.getProfile(REGION_ID, ACCESS_KEY_ID, ACCESS_KEY_SECRET);
        DefaultAcsClient client = new DefaultAcsClient(profile);

        GetCallerIdentityRequest request = new GetCallerIdentityRequest();

        //发起请求
        try {
            GetCallerIdentityResponse response = client.getAcsResponse(request);
            System.out.println("RequestId: " + response.getRequestId());
            System.out.println("AccountId: " + response.getAccountId());
            System.out.println("UserId: " + response.getUserId());
            System.out.println("Arn: " + response.getArn());
        } catch (ClientException e) {
            System.out.println("Failed：");
            System.out.println("Error code: " + e.getErrCode());
            System.out.println("Error message: " + e.getErrMsg());
            System.out.println("RequestId: " + e.getRequestId());
        }

    }
}
```
