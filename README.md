# security-demo #
java security 权限框架实战

# security 和 shiro #
Spring Security是一个能够为基于Spring的企业应用系统提供声明式的安全访问控制解决方案的安全框架。它提供了一组可以在Spring应用上下文中配置的Bean，充分利用了Spring IoC，DI（控制反转Inverse of Control ,DI:Dependency Injection 依赖注入）和AOP（面向切面编程）功能，为应用系统提供声明式的安全访问控制功能，减少了为企业系统安全控制编写大量重复代码的工作。
Spring Security对Web安全性的支持大量地依赖于Filter过滤器。这些过滤器拦截进入请求，并且在应用程序处理该请求之前进行某些安全处理。 Spring Security提供有若干个过滤器，它们能够拦截请求，并将这些请求转给认证和访问决策管理器处理, 从而增强安全性。根据自己的需要，可以使用所列的几个过滤器来保护自己的应用程序是非侵入式安全架构
基于Servelt Filter和Spring AOP,使商业逻辑与安全逻辑分开,结构更清晰，使用Spring来代理对象, 能方便得保护方法调用

http://ju.outofmemory.cn/entry/109825

http://outofmemory.cn/#csdn

# Spring Security实现原理：

Spring Security对Web安全性的支持大量地依赖于Servlet过滤器。通过这些过滤器拦截进入请求，判断是否已经登录认证且具访问对应请求的权限。

要完成访问控制，Spring Security至少需要下面四个拦截器（调度器、认证管理器、权限资源关联器、访问决策器）进行配合完成：

    <!-- mySecurityInterceptor这里我们把它命名为调度器吧 -->
    <!-- 必须包含 authenticationManager,securityMetadataSource,accessDecisionManager 三个属性 -->   
    <!-- 我们的所有控制将在这三个类中实现 --> 
    <!-- 它继承AbstractSecurityInterceptor类并实现了Filter接口 --> 
    <bean id="mySecurityInterceptor" class="com.luo.Filter.MySecurityInterceptor">  
        <b:property name="authenticationManager" ref="authenticationManager" />  
        <b:property name="securityMetadataSource" ref="securityMetadataSource" />  
        <b:property name="accessDecisionManager" ref="accessDecisionManager" />  

    </bean>  

    <!-- 认证管理器，实现用户认证的入口 -->  
    <authentication-manager alias="authenticationManager">  
        <authentication-provider user-service-ref="myUserDetailService" />   
    </authentication-manager>  

    <!-- 在这个类中，你就可以从数据库中读入用户的密码，角色信息等 -->  
    <!-- 主要实现UserDetailsService接口即可，然后返回用户数据 -->  
    <bean id="myUserDetailService" class="com.luo.Filter.MyUserDetailService" />  

    <!-- 权限资源关联器，将所有的资源和权限对应关系建立起来，即定义某一资源可以被哪些角色访问 -->  
    <!-- 它实现了FilterInvocationSecurityMetadataSource接口 -->  
    <bean id="securityMetadataSource" class="com.luo.Filter.MyFilterInvocationSecurityMetadataSource" /> 

    <!--访问决策器，决定某个用户具有的角色，是否有足够的权限去访问某个资源 --> 
    <!-- 它实现了AccessDecisionManager接口 --> 
    <bean id="accessDecisionManager" class="com.luo.Filter.MyAccessDecisionManager">


看完上面的配置，可能未必能够完全明白，下面我们再进一步说明。

（1）首先我们自定义一个过滤器（调度器，这里我们命名为mySecurityInterceptor），这个过滤器继承AbstractSecurityInterceptor类（这里先说明，本文但凡不是自定义的类或接口都是Spring Security提供的，无须深究）。 它至少包含 authenticationManager,accessDecisionManager,securityMetadataSource三个属性，我们的所有控制将在这三个类中实现。

（2）登录验证：自定义类MyUserDetailService实现UserDetailsService接口和其loadUserByUsername方法，这个方法根据用户输入的用户名，从数据库里面获取该用户的所有权限细信息（统称用户信息）。Spring Security的AuthenticationProcessingFilter拦截器调用authenticationManager，类MyUserDetailService拿到用户信息后，authenticationManager对比用户的密码（即验证用户），如果通过了，那么相当于通过了AuthenticationProcessingFilter拦截器，也就是登录验证通过。

（3）资源访问控制：MySecurityInterceptor继承AbstractSecurityInterceptor、实现Filter是必须的。登陆后，每次访问资源都会被MySecurityInterceptor这个拦截器拦截，它首先会调用MyFilterInvocationSecurityMetadataSource类的getAttributes方法获取被拦截url所需的权限，在调用MyAccessDecisionManager类decide方法判断用户是否够权限。

http://blog.csdn.net/u012367513/article/details/38866465

http://blog.csdn.net/u013142781/article/details/50631663

