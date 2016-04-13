#Spring Boot 静态资源处理
Spring Boot 默认的处理方式就已经足够了，默认情况下Spring Boot 使用WebMvcAutoConfiguration中配置的各种属性。

建议使用Spring Boot 默认处理方式，需要自己配置的地方可以通过配置文件修改。

但是如果你想完全控制Spring MVC，你可以在@Configuration注解的配置类上增加@EnableWebMvc，增加该注解以后WebMvcAutoConfiguration中配置就不会生效，你需要自己来配置需要的每一项。这种情况下的配置方法建议参考WebMvcAutoConfiguration类。

本文以下内容针对Spring Boot 默认的处理方式，部分配置通过在application.yml配置文件中设置。

##配置资源映射
Spring Boot 默认配置的`/**`映射到`/static`（或`/public` ，`/resources`，`/META-INF/resources`），`/webjars/**`会映射到`classpath:/META-INF/resources/webjars/`。

**注意：**上面的/static等目录都是在classpath:下面。

#####1. 自定义目录
如果你想增加如`/mystatic/**`映射到`classpath:/mystatic/`，你可以让你的配置类继承`WebMvcConfigurerAdapter`，然后重写如下方法：

```
@Configuration
public class MyWebAppConfigurer extends WebMvcConfigurerAdapter {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/mystatic/**")
                .addResourceLocations("classpath:/mystatic/");
    }
}
```
这种方式会在默认的基础上增加`/mystatic/**`映射到`classpath:/mystatic/`，不会影响默认的方式，可以同时使用。

#####2. 使用外部目录
如果我们要指定一个绝对路径的文件夹（如 H:/myimgs/ ），则只需要使用`addResourceLocations`指定即可。

代码如下：
```
registry.addResourceHandler("/myimgs/**").addResourceLocations("file:H:/myimgs/");
```

#####3. 通过配置文件配置
上面是使用代码来定义静态资源的映射，其实Spring Boot也为我们提供了可以直接在 application.properties（或.yml）中配置的方法。

配置方法如下：
```
spring.mvc.static-path-pattern=
# 默认值为 classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/ 

spring.resources.static-locations=这里设置要指向的路径，多个使用英文逗号隔开，
```
修改`spring.mvc.static-path-pattern`配置会影响默认的`/**`，例如修改为`/static/**`后，只能映射如`/static/js/sample.js`这样的请求（修改前是`/js/sample.js`）。这个配置只能写一个值，不像大多数可以配置多个用逗号隔开的。

修改`spring.resources.static-locations`可以重新定义 pattern 所指向的路径，支持 classpath: 和 file: （上面已经做过说明）

##使用注意
例如有如下目录结构：
```
└─ resources
    │ application.yml
    │
    ├─static
    │  ├─css
    │  │    index.css
    │  │
    │  └─js
    │       index.js
    │
    └─templates
            index.ftl
```
在index.ftl中该如何引用上面的静态资源呢？ 
如下写法：
```
<link rel="stylesheet" type="text/css" href="/css/index.css">
<script type="text/javascript" src="/js/index.js"></script>
```
注意：默认配置的`/**`映射到`/static`（或`/public` ，`/resources`，`/META-INF/resources`）

当请求`/css/index.css`的时候，Spring MVC 会在`/static/`目录下面找到。

如果配置为`/static/css/index.css`，那么上面配置的几个目录下面都没有`/static`目录，因此会找不到资源文件！

所以写静态资源位置的时候，不要带上映射的目录名（如`/static/`，`/public/` ，`/resources/`，`/META-INF/resources/`）！

##使用WebJars
WebJars：http://www.webjars.org/

先说一下什么是webjars？我们在Web开发中，前端页面中用了越来越多的JS或CSS，如jQuery等等，平时我们是将这些Web资源拷贝到Java的目录下，这种通过人工方式拷贝可能会产生版本误差，拷贝版本错误，前端页面就无法正确展示。

WebJars 就是为了解决这种问题衍生的，将这些Web前端资源打包成Java的Jar包，然后借助Maven这些依赖库的管理，保证这些Web资源版本唯一性。

WebJars 就是将js, css 等资源文件放到 classpath:/META-INF/resources/webjars/ 中，然后打包成jar 发布到maven仓库中。

例如使用jquery，添加依赖：
```
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>1.11.3</version>
</dependency>
```
然后可以如下使用：
```
<script type="text/javascript" src="/webjars/jquery/1.11.3/jquery.js"></script>
```
Spring Boot 默认将 `/webjars/**` 映射到 `classpath:/META-INF/resources/webjars/`。

文件存放结构为：
```
META-INF/resources/webjars/jquery/2.1.4/jquery.js
META-INF/resources/webjars/jquery/2.1.4/jquery.min.js
META-INF/resources/webjars/jquery/2.1.4/jquery.min.map
META-INF/resources/webjars/jquery/2.1.4/webjars-requirejs.js
```

##版本号统一管理
你可能注意到href中的1.11.3版本号了，如果仅仅这么使用，那么当我们切换版本号的时候还要手动修改href，怪麻烦的，我们可以用如下方式解决。

先在pom.xml中添加依赖：
```
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator</artifactId>
</dependency>
```
增加一个WebJarController：
```
/**
 * 处理WebJars，自动读取版本号
 */
@Controller
public class WebJarController {

    private final WebJarAssetLocator assetLocator = new WebJarAssetLocator();

    @ResponseBody
    @RequestMapping("/webjarslocator/{webjar}/**")
    public ResponseEntity locateWebjarAsset(@PathVariable String webjar,
    	HttpServletRequest request) {
        try {
            String mvcPrefix = "/webjarslocator/" + webjar + "/";
            String mvcPath = (String) request.getAttribute(
            	HandlerMapping.PATH_WITHIN_HANDLER_MAPPING_ATTRIBUTE);
            String fullPath = assetLocator.getFullPath(webjar,
            	mvcPath.substring(mvcPrefix.length()));
            return new ResponseEntity(new ClassPathResource(fullPath),
            	HttpStatus.OK);
        } catch (Exception e) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }
}
```
然后使用的时候按照如下方式：
```
<script type="text/javascript" src="/webjarslocator/jquery/jquery.js"></script>
```
**注意：**这里不需要在写版本号了，但是注意写url的时候，只是在原来url基础上去掉了版本号，其他的都不能少！







