spring-cloud-aws를 이용한 프로젝트를 로컬에서 실행시킬 때 Application 자체는 잘 뜨는데 이런  
Warning이 날 때가 있다.
```
com.amazonaws.SdkClientException: Failed to connect to service endpoint: 
```
이런 Warning이 나는 코드를 보면
```
public final class AwsCloudEnvironmentCheckUtils {

	private static final String EC2_METADATA_ROOT = "/latest/meta-data";

	private static Boolean isCloudEnvironment;

	private AwsCloudEnvironmentCheckUtils() {
	}

	public static boolean isRunningOnCloudEnvironment() {
		if (isCloudEnvironment == null) {
			try {
				isCloudEnvironment = EC2MetadataUtils
						.getData(EC2_METADATA_ROOT + "/instance-id", 1) != null;
			}
			catch (AmazonClientException e) {
				isCloudEnvironment = false;
			}
		}
		return isCloudEnvironment;
	}

}
```
위와 같은 코드를 사용하고 있는데  
여기서 EC2MetadataUtils.getData()가 호출될 때 EC2 환경이 아니라 로컬 환경이면  
SdkClientException이 나게 되고, 내부적으로 이를 잡아 WARNING을 띄워준다.  
원인을 알고 EC2 환경에서 실행하면 해결되는 warning이긴 하지만 로컬에서 저 warning을  
더 이상 보고 싶지 않을 수 있다. 이 때 해결 방법은 해당 클래스의 로그 레벨을 조정해주는 것이다.  
WARNING에서 ERROR로 조정해주면 깔끔하게 처리된다.  

```
logging:
  level:
    com:
      amazonaws:
        util:
          EC2MetadataUtils: error
```          





















