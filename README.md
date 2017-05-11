git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/meijw/Demo.git
git push -u origin master


# Test
```java
@Aspect
@Component
public class WebLogAspect {

	private Logger logger = LoggerFactory.getLogger(this.getClass());

	ThreadLocal<Long> startTime = new ThreadLocal<>();

	@Pointcut("execution(public * com.speed4j.microservice..*.*(..))")
	public void webLog() {
	}

	@Before("webLog()")
	public void doBefore(JoinPoint joinPoint) {
		startTime.set(System.currentTimeMillis());
		MDC.put("logFileName", Thread.currentThread().getName() + " " + Thread.currentThread().getId());
	}

	@AfterReturning(returning = "ret", pointcut = "webLog()")
	public void doAfterReturning(Object ret) {
		// 处理完请求，返回内容
		logger.info("RESPONSE : " + ret);
		logger.info("SPEND TIME : " + (System.currentTimeMillis() - startTime.get()));
		MDC.remove("logFileName");
	}

}

```
