

## Project Setup ##
  * Download and install Mercurial from http://mercurial.selenic.com/downloads/
  * Checkout project: `hg clone https://openrtb.googlecode.com/hg/ openrtb`
  * Build project using maven: `mvn install`
  * Note: The current version is a SNAPSHOT version

## Projects Structure ##

  * common (common classes including model and common utils)
  * demand-side
    * dsp-client
    * dsp-core
    * dsp-intf
    * dsp-web
  * supply-side
    * ssp-client (sample implementations)
    * ssp-core (core processing classes)
    * ssp-intf (integraion point interfaces)
    * ssp-web (empty project to use for custom implementation)

## Implementing SSP Side ##
  1. Implement the SupplySideService interface:
    * Implement the `List<Advertiser> setBlocklists(List<Advertiser> advertisers)` method - _Add a Blocklist object to each advertiser (see SupplySideServiceRefImpl in ssp-client)_
    * Implement the `byte[] getSharedSecret(String dsp)` method - _Return the shared secret bytes for the calling DSP (e.g. from db)_
  1. Inject your implementation of the SupplySideService into the SupplySideServer
  1. Implement a servlet that passes the request body to the SupplySideServer
**Sample implementations**

_Spring injection_
```
<bean id="supplySideServer" class="org.openrtb.ssp.core.SupplySideServer">
	<constructor-arg ref="supplySideService"/>
</bean>

<bean id="supplySideService" class="com.example.openrtb.service.impl.SupplySideServiceImpl"/>
```

_SpringMVC Servlet_
```
@Controller
public class OpenRtbController {

	@Resource
	private SupplySideServer supplySideServer;

	@RequestMapping(value = "/")
	public void getBlockLists(@RequestBody String body, HttpServletResponse response, Writer writer) throws Exception {
		String resJson = supplySideServer.process(body);
		//...
		writer.write(resJson);
	}
}
```

## Manually Generating Request Token ##

_For testing purposes, you may want to manually generate a MD5 token for a request. Below are instructions and examples._
  1. Remove all spaces and new lines that are ouside literals from the JSON request
  1. Remove the 'token' element and add a 'sharedSecret' element. See example below
  1. Remove last linefeed character (automatically added by vi,nano,etc)
  1. The shared secret element value is the hex representation of the secret key characters. E.g. for the secret key: Zca2 the shared secret value would be: 5A636132

**Sample MD5 generation**

Edited request for md5 generation
```
{{"identification":{"organization":"DSP","timestamp":1289405763341},"advertisers":[{"landingPageTLD":"cnn.com","name":"CNN"}]},"sharedSecret":5A636132}
```

Removing last linefeed
```
cat jsonfile | tr -d "\r\n" > jsonfile2
```

Generating MD5
```
cat jsonfile2 | md5sum
```

## Handling Bad Requests ##

_Some bad requests are handled by the SupplySideServer class. In case of other exceptions null is returned. You may want to check the returned value in your Servlet implementation to handle such requests._

**Sample implementation (Spring MVC)**
```
String resJson = supplySideServer.process(body);
if(resJson==null){
	response.setStatus(HttpServletResponse.SC_BAD_REQUEST);	
}else{
 	writer.write(resJson);
}
```