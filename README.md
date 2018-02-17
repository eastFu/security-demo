# security-demo #
java security 权限框架实战

# security 和 shiro #
Spring Security是一个能够为基于Spring的企业应用系统提供声明式的安全访问控制解决方案的安全框架。它提供了一组可以在Spring应用上下文中配置的Bean，充分利用了Spring IoC，DI（控制反转Inverse of Control ,DI:Dependency Injection 依赖注入）和AOP（面向切面编程）功能，为应用系统提供声明式的安全访问控制功能，减少了为企业系统安全控制编写大量重复代码的工作。
Spring Security对Web安全性的支持大量地依赖于Filter过滤器。这些过滤器拦截进入请求，并且在应用程序处理该请求之前进行某些安全处理。 Spring Security提供有若干个过滤器，它们能够拦截请求，并将这些请求转给认证和访问决策管理器处理, 从而增强安全性。根据自己的需要，可以使用所列的几个过滤器来保护自己的应用程序是非侵入式安全架构
基于Servelt Filter和Spring AOP,使商业逻辑与安全逻辑分开,结构更清晰，使用Spring来代理对象, 能方便得保护方法调用

http://ju.outofmemory.cn/entry/109825

http://outofmemory.cn/#csdn

