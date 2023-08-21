Jwt用于生成验证登录的token

# 导入依赖

```xml
<!--        jwt工具-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.6.0</version>
</dependency>
```

# JwtUtils工具类

在util包中

JwtUtil

```java
package xyz.kbws.util;

import com.alibaba.fastjson.JSON;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import xyz.kbws.config.JwtConfig;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

/**
 * @author hsy
 * @date 2023/3/2
 */
@Component
public class JwtUtils {
    @Autowired
    JwtConfig jwtConfig;
    /**
     * 创建Token，载荷包含学号
     * @param id
     * @return
     */
    public String createToken(String id){
        Map<String,Object> map = new HashMap<>();
        //添加键值
        map.put("id",id);
        //构建
        JwtBuilder jwtBuilder = Jwts.builder()
                //设置有效载荷
                .setClaims(map)
                //设置过期时间
                .setExpiration(new Date(System.currentTimeMillis()+1000*jwtConfig.getExp()))
                //采用HS256方式签名，key就是用来签名的密钥
                .signWith(SignatureAlgorithm.HS256,jwtConfig.getSignature());
        //调用compact函数将token打包成String返回
        return jwtBuilder.compact();
    }

    public Map<String,Object> analysisToken(HttpServletRequest request, HttpServletResponse response){
        String token = request.getHeader("token");
        Claims claims = null;
        //token不一定通过验证
        try {
            claims = Jwts.parser()
                    .setSigningKey(jwtConfig.getSignature())
                    .parseClaimsJws(token)
                    .getBody();
        }catch (Exception e){
            try {
                response.getWriter().write(JSON.toJSONString(R.TokenError()));
            }catch (IOException ioException){
                ioException.printStackTrace();
            }
        }
        return claims;
    }
}
```

# 配置文件

application.yaml

```yaml
jwt:
  # 签名
  signature: XGKX
  # 过期时间  单位 s
  exp: 604800  # 7 * 60 * 60 * 24
```

JwtConfig

```java
package xyz.kbws.config;

import lombok.Data;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/**
 * @author hsy
 * @date 2023/3/2
 */
@Data
@Component
public class JwtConfig {
    //签名
    @Value("${jwt.signature}")
    private String signature;
    //过期时间
    @Value("${jwt.exp}")
    private Integer exp;
}
```

LoginHandler

```java
package xyz.kbws.config;

import org.springframework.web.servlet.HandlerInterceptor;
import xyz.kbws.util.JwtUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author hsy
 * @date 2023/3/2
 */
public class LoginHandler implements HandlerInterceptor {
    private JwtUtils jwtUtils;

    public LoginHandler(JwtUtils jwtUtils){
        this.jwtUtils = jwtUtils;
    }

    /**
     * 拦截器
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器生效"+request.getRequestURL());
        return jwtUtils.analysisToken(request,response)!=null;
    }
}
```

MyMvcConfig

```java
package xyz.kbws.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.*;
import xyz.kbws.util.JwtUtils;

/**
 * @author hsy
 * @date 2023/3/1
 */
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Autowired
    JwtUtils jwtUtils;

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOriginPatterns("*")
                .allowedMethods("GET","POST","PUT","DELETE","HEAD","OPTIONS")
                .allowCredentials(true)
                .maxAge(3600)
                .allowedHeaders("*");
    }

    public void addInterceptors(InterceptorRegistry registry){
        registry.addInterceptor(new LoginHandler(jwtUtils))
                .addPathPatterns("/**")
                .excludePathPatterns("/user/login","/error")
                .excludePathPatterns("/swagger-ui.html")
                .excludePathPatterns("/swagger-resources/**")
                .excludePathPatterns("/webjars/**")
                .excludePathPatterns("/v2/api-docs/**")
                .excludePathPatterns("/doc.html");
    }
}
```

