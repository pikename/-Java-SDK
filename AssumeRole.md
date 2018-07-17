package sts;

import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.auth.sts.AssumeRoleRequest;
import com.aliyuncs.auth.sts.AssumeRoleResponse;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.profile.DefaultProfile;
/**

 本实例介绍如何使用阿里云Java SDK调用安全令牌 STS的 AssumeRole 接口获取一个扮演该角色的临时身份
 AssumeRole_操作接口_API 参考（STS）_API参考_访问控制-阿里云 https://help.aliyun.com/document_detail/28763.html?spm=5176.11065259.1996646101.searchclickresult.1aa02bdfGx5MyU
 阿里云STS (Security Token Service) 是为阿里云账号（或RAM用户）提供短期访问权限管理的云服务。
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

 FQA:
 com.aliyuncs.exceptions.ClientException: NoPermission : You are not authorized to do this action. You should be authorized by RAM.
 您需要通过RAM用户给角色授权 AliyunSTSAssumeRoleAccess
 */
 ```
public class AssumeRoleDemo {

    // 您的可用区ID，
    // 服务地址_附录_API 参考（STS）_API参考_访问控制-阿里云 https://help.aliyun.com/document_detail/66053.html?spm=a2c4g.11186623.6.689.6NvQAN
    private static final String REGION_ID = "sts.aliyuncs.com";
    // 只有 子账号才能调用 AssumeRole接口
    // 阿里云主账号的AccessKeys不能用于发起AssumeRole请求
    // 请首先在RAM控制台创建子用户，并为这个用户创建AccessKeys
    private static final String ACCESS_KEY_ID  = "access-key-id>";
    private static final String ACCESS_KEY_SECRET = "<access-key-secret>";

    public static void main(String[] args) {
        // 创建 DefaultProfileAcsClient并实例化
        DefaultProfile profile = DefaultProfile.getProfile(REGION_ID, ACCESS_KEY_ID, ACCESS_KEY_SECRET);
        DefaultAcsClient client = new DefaultAcsClient(profile);

        // 指定角色的全局资源描述符(Aliyun Resource Name，简称Arn)。每个角色都有一个唯一的全局资源描述符，规定格式为 acs:ram::$accountID:role/$roleName.
        // 您可以在RAM控制台的角色管理页面RAM控制台的角色管理列表中，进入角色详情页可以查看一个角色的RoleArn。
        String roleArn = "acs:ram::1106313973964915:role/user";
        // 此参数用来区分不同的Token，可用于用户级别的访问审计
        // 格式： ^[a-zA-Z0-9\.@\-_]+$
        String roleSessionName = "007";
        // 授权策略，您可以通过此参数限制生成的Token的权限，不指定则返回的Token将拥有指定角色的所有权限。
        // Policy语法结构_授权策略语言_用户指南_访问控制-阿里云 https://help.aliyun.com/document_detail/28664.html?spm=a2c4g.11186623.2.1.NCNMbz
        String policy = null;
        // 创建 AssumeRole 请求并设置参数
        AssumeRoleRequest request = new AssumeRoleRequest();
        request.setRoleArn(roleArn);
        request.setRoleSessionName(roleSessionName);
        request.setPolicy(policy);
        // 指定的过期时间，单位为秒。过期时间范围：900 ~ 3600，默认值为3600。
        request.setDurationSeconds(3600L);

        //发起请求
        try {
            AssumeRoleResponse response = client.getAcsResponse(request);
            System.out.println("Expiration: " + response.getCredentials().getExpiration());
            System.out.println("Access Key Id: " + response.getCredentials().getAccessKeyId());
            System.out.println("Access Key Secret: " + response.getCredentials().getAccessKeySecret());
            System.out.println("Security Token: " + response.getCredentials().getSecurityToken());
            System.out.println("RequestId: " + response.getRequestId());
        } catch (ClientException e) {
            System.out.println("Failed：");
            System.out.println("Error code: " + e.getErrCode());
            System.out.println("Error message: " + e.getErrMsg());
            System.out.println("RequestId: " + e.getRequestId());
        }
    }
}
```
