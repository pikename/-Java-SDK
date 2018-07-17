```
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
