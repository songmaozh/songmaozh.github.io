---
title: Webflow配置文件分析
date: 2017-01-19 15:38:31
tags:
---
Webflow配置文件分析
在WEB-INF文件夹下的login-webflow.xml是登陆流程的主要配置文件。在该文件中，定义了用户登录的整个处理流程。
首先，配置文件中的 on-start标签定义了用户第一次进入流程中的预处理动作。该标签对应spring中的id为initialFlowSetupAction的bean。查看该bean（InitialFlowSetupAction）的代码。该类需要继承自AbstractAction，AbstractAction方法是org.springframework.webflow.action包中的类。是webflow中的基础类。该类中的doExecute方法是对应处理业务的方法。就犹如servlet中的service方法一样。该方法的参数是RequestContext对象，该参数是一个流程的容器。该方法从request中获取TGT，并且构建一个临时的service对象（不同域注册的service，详情见接入系统管理）。并且，将TGT和service放在FlowScope作用域中。
流程的初始化完毕之后，就开始一系列的判断了。也就是进入decision-state节点。这些节点是依次执行的。
<!-- 检查flow中是否存在TGT如果存在，存在进入hasServiceCheck,为空进入gatewayRequestCheck -->
    <decision-state id="ticketGrantingTicketExistsCheck">
       <if test="flowScope.ticketGrantingTicketId neq null" then="hasServiceCheck" else="gatewayRequestCheck" />
    </decision-state>
   
    <!-- 主要是CS结构使用gatewat,暂时不研究 -->
    <decision-state id="gatewayRequestCheck">
       <if test="externalContext.requestParameterMap['gateway'] neq '' &amp;&amp; externalContext.requestParameterMap['gateway'] neq null &amp;&amp; flowScope.service neq null" then="gatewayServicesManagementCheck" else="viewLoginForm" />
    </decision-state>
   
    <!-- 存在TGT，说明用户已经登陆,测试flow中service是否为空，不为空，进入renewRequestCheck，为空，进入viewGenericLoginSuccess -->
    <decision-state id="hasServiceCheck">
       <if test="flowScope.service != null" then="renewRequestCheck" else="viewGenericLoginSuccess" />
    </decision-state>
    <!--
       用户已经登陆，且请求参数中存在service 判断请求中是否存在'renew'参数，如果renew参数为空或者没有内容，那么，进入viewLoginForm，否则进入generateServiceTicket
       renew参数和gateway参数不兼容。renew参数将绕过单点登录。也就是说即使用户登录了，还将要求用户登录。(等你妹啊，人家都登录了，凭什么还要让人家再登录一次)
     -->
    <decision-state id="renewRequestCheck">
       <if test="externalContext.requestParameterMap['renew'] neq '' &amp;&amp; externalContext.requestParameterMap['renew'] neq null" then="viewLoginForm" else="generateServiceTicket" />
    </decision-state>
   
    <decision-state id="warn">
       <if test="flowScope.warnCookieValue" then="showWarningView" else="redirect" />
    </decision-state>
 
对应的dicision-state走完之后，如果不存在TGT，其实就会进入voiwLoginForm节点。该节点是一个view-state类型的，这也就是说明该节点是一个页面，view=“casLoginView”属性定义了该view对应的页面是“casLoginView”。这个视图会被spring的视图解析器解析成/WEB-INF/view/jsp/default/ui/casLoginView.jsp页面。用户这时候就能看到一个登陆界面了。
需要注意的是，用户看到的登录界面中，会有hidden类型的一个lt参数：
<input type="hidden" name="lt" value="${flowExecutionKey}" />
该参数可以理解成每个需要登录的用户都有一个流水号。只有有了webflow发放的有效的流水号，用户才可以说明是已经进入了webflow流程。否则，没有流水号的情况下，webflow会认为用户还没有进入webflow流程，从而会重新进入一次webflow流程，从而会重新出现登录界面。
 
用户点击登录之后，提交到realSubmit节点。该节点执行的是authenticationViaFormAction.submit方法，在该方法中，将会验证用户的认证信息是否正确。验证成功之后，跳转到sendTicketGrantingTicket,在这里，将生成TGT，然后，进入serviceCheck，在serviceCheck中，将会验证flowScope作用域中是否存在service，如果存在，则进入generateServiceTicket，否则进入登录成功页面。在generateServiceTicket中，也将生成ST，同时检查是否有警告信息，然后进行重定向到用户最开始请求的地址。
至此，springMVC与webflow整合以及登录整个流程已经讲解完毕。


login-webflow.xml
<view-state id="viewLoginForm" view="casLoginView" model="credential">
        <binder>
        	<!-- 增加惠商云企业账号登录 -->
        	<binding property="tenantCode" required="true"/>
            <binding property="username" required="true"/>
            <binding property="md5Password" required="true"/>
            <binding property="shaPassword" required="true"/>
            <binding property="sysid" required="true"/>
            <binding property="verify_code" required="true"/>
            <binding property="isAutoLogin" required="true"/>
            <binding property="tokeninfo" required="true"/>
            <binding property="openid" required="true"/>
            <binding property="tenantid" required="true"/>
            <binding property="service" required="true"/>
            
            <binding property="newpass" required="true"/> <!-- 修改的新的用户名-->
            <binding property="randomvalue" required="true"/>  
        </binder>
        <on-entry>
            <set name="viewScope.commandName" value="'credential'"/>

            <!--
            <evaluate expression="samlMetadataUIParserAction" />
            -->
        </on-entry>
        <transition on="submit" bind="true" validate="true" to="realSubmit"/>
        
        <transition on="modifyPW" bind="true" validate="true" to="modifyPWSubmit"/>
    </view-state>

casLoginView.jsp

     <% String sysid =  request.getParameter("sysid")  ;
    String verify_code = request.getParameter("verify_code")  ; 
    String service= request.getParameter("service")  ;  //客户端服务的 地
    String openid = request.getParameter("openid")  ;

%>