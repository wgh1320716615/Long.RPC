# Introduction
Introduction
I learned Netty decided to write their own 1 based on Netty, Zookeeper, Spring's lightweight RPC framework, gained a lot, but I'm shallow, inevitably there are omissions, if there are criticisms and suggestions, please send to the mail wgh1320716615@gmail.com.

# Features
* **Support for long connections**
* **Support for asynchronous calls**
* **Heartbeat detection**
* **Support for JSON serialization**
* **Nearly zero configuration, annotation-based call implementation**
* **Service registry based on Zookeeper**
* **Support for dynamic management of client connections**
* **Support for client-side service listening and discovery**
* **Support for server-side service registration**
* **Based on Netty4.X version implementation**

# Quick Start
## server-side development
* **Add your own Service under Service on the server side, and add the @Service annotation.**
```Java
  @Service
  public class TestService {
  	public void test(User user){
  		System.out.println("调用了TestService.test");
  	}
  }
```
* **Generate a service interface and generate a class that implements it**
The interfaces are as follows
```Java
  public interface TestRemote {
  	public Response testUser(User user);  
  }
```
The implementation class is as follows, add the @Remote annotation to your implementation class, this class is where you actually call the service, you can generate any kind of Response you want to return to the client
```Java
  @Remote
  public class TestRemoteImpl implements TestRemote{
  	@Resource
  	private TestService service;
  	public Response testUser(User user){
  		service.test(user);
  		Response response = ResponseUtil.createSuccessResponse(user);
  		return response;
  	}
  }
```
## Client development
* **Generate an interface on the client side that is the interface you want to call**
```Java
  public interface TestRemote {
  	public Response testUser(User user);
  }
```
## utilization
* **Generate a property in the form of an interface where you want to call it, add the @RemoteInvoke annotation to the property**
```Java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(classes=RemoteInvokeTest.class)
  @ComponentScan("\\")
  public class RemoteInvokeTest {
  	@RemoteInvoke
  	public static TestRemote userremote;
  	public static User user;
  	@Test
  	public void testSaveUser(){
  		User user = new User();
  		user.setId(1000);
  		user.setName("张三");
  		userremote.testUser(user);
  	}
  }	
  ```
## end
* **Result of 10,000 calls**
![](https://camo.githubusercontent.com/57188c6ad1c73358c3e77a0d1206b8de58ad694d6760bcf97325c892131fb0eb/68747470733a2f2f73312e617831782e636f6d2f323031382f30372f30362f505a4d4d42462e706e67)
* **Results of 100,000 calls**
![](https://camo.githubusercontent.com/3d4e97748ea95bb1127458d3430479f004bb6dd67a635f669fe6ba30120ed1e3/68747470733a2f2f73312e617831782e636f6d2f323031382f30372f30362f505a4d334e392e706e67)
* **One million calls result**
![](https://camo.githubusercontent.com/20529dea30d596d48dbfbea0f1d966f96c60bc4be654f019e2df76efb6f8a68b/68747470733a2f2f73312e617831782e636f6d2f323031382f30372f30362f505a4d5931782e706e67)

# Overview
![](https://camo.githubusercontent.com/0e1f0c52612225fdfe401402af4dace5aa35bac758f0da5d7c7d5f727be58c76/68747470733a2f2f73312e617831782e636f6d2f323031382f30372f30362f505a4b3353502e706e67)



