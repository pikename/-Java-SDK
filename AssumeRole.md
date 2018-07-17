/**

 本实例介绍如何使用阿里云Java SDK调用安全令牌 STS的 AssumeRole 接口获取一个扮演该角色的临时身份

 阿里云STS (Security Token Service) 是为阿里云账号（或RAM用户）提供短期访问权限管理的云服务。
 通过STS，您可以为联盟用户（您的本地账号系统所管理的用户）颁发一个自定义时效和访问权限的访问凭证。
 联盟用户可以使用STS短期访问凭证直接调用阿里云服务API，或登录阿里云管理控制台操作被授权访问的资源。

 在创建临时身份之前，您需要获取以下信息：
 1)您已经开通了阿里云RAM服务，并为主账号或者RAM用户创建了Access Key ID 和Access Key Secret。
 对应访问控制-用户管理中配置（注意：用户AK只提供唯一的下载的机会，务必及时保管）
 对应访问控制-角色管理中配置
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
 你需要给用户授权 AliyunSTSAssumeRoleAccess
 */
package sts;
