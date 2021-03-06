# 五分钟看明白WAF {#concept_lpz_q5l_l2b .concept}

WAF是阿里云云盾提供的Web应用防火墙，帮助您监控网站上的HTTP/HTTPS访问请求，并通过自定义过滤规则和启用Web攻击防护等功能，帮助您部署网站访问控制。

在使用WAF时，您需要开通WAF并将网站接入WAF，使网站的访问流量全部流转到WAF。下图显示了开通和接入WAF的简要流程。

完成接入后，WAF将按照您配置的过滤规则和启用的Web防护功能，检测并过滤收到的访问请求，只有满足规则条件的合法请求才会经WAF返回到您的服务器（回源）。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15550/15501946937111_zh-CN.png)

## 开通WAF {#activate .section}

您可以使用按量付费或包年包月的计费方式开通WAF。

-   按量付费按当日被防护网站的访问QPS峰值和当日选用的WAF防护功能，生成后付费账单；每日结算前一日费用。
-   包年包月按月/年计费，由您选购适用的WAF套餐，生成账单后直接付费；您可在选购的时长内享用套餐内的防护服务。

**说明：** 使用包年包月方式选购WAF套餐时，我们需要了解您的正常业务流量，以便区分DDoS攻击等异常流量。每种WAF套餐支持不同的业务带宽，如果您的实际业务正常流量大于套餐内的带宽限制，您需要[购买额外带宽](../../../../../cn.zh-CN/产品定价/开通WAF/额外带宽扩展说明.md#)。

更多信息，请参考[计费方式](../../../../../cn.zh-CN/产品定价/计费方式.md#)、[WAF版本说明](../../../../../cn.zh-CN/产品定价/开通WAF/WAF各版本功能说明.md#)、[购买Web应用防火墙](../../../../../cn.zh-CN/产品定价/开通WAF/购买Web应用防火墙.md#)。

开通WAF后，您将获得一个WAF实例（对应一个实例IP）；您可以使用这个WAF实例接入最多10个网站，为其开启防护，这10个网站只能使用1个一级域名。

-   如果您希望防护具有不同一级域名的网站，您需要[购买域名扩展包](../../../../../cn.zh-CN/产品定价/开通WAF/扩展域名包.md#)。
-   如果您有很重要的域名需要单独防护，而非使用同一个WAF IP防护所有域名，您可以[购买独享IP包](../../../../../cn.zh-CN/产品定价/开通WAF/独享IP包.md#)。

## 接入WAF {#implement .section}

开通WAF后，您需要在WAF控制台创建网站配置来关联要防护的域名，并通过[域名解析（DNS）](https://en.wikipedia.org/wiki/Domain_Name_System)，将网站访问流量流转到已开通的WAF实例（IP）上。

-   添加网站配置

    网站配置描述了被防护网站的流量转发关系。您可以使用自动或手动的方式添加网站配置。在网站配置中，您需要指定要防护的网站域名和源站服务器地址等信息。完成网站配置后，WAF分配给这个域名一个专用的[CNAME地址](https://en.wikipedia.org/wiki/CNAME_record)。

    具体操作，请参考[网站配置](cn.zh-CN/用户指南/接入WAF/网站配置.md#)。

-   修改DNS解析

    只有当您在对应域名的解析记录中添加并应用WAF CNAME记录后，才可以正式将网站访问流量导向WAF实例进行监控。网站接入WAF后，WAF实例帮助您过滤恶意请求，并将合法的访问流量返回源站服务器。

    具体操作，请参考[业务接入WAF配置](cn.zh-CN/用户指南/接入WAF/业务接入WAF配置.md#)。

-   自动接入WAF

    如果您的域名使用[阿里云云解析DNS](https://wanwang.aliyun.com/domain/dns/) 进行域名解析，我们支持一键式接入WAF；否则，您需要手动接入。


## 配置防护策略 {#policies .section}

网站接入WAF后，WAF分析客户端使用HTTP/HTTPS协议发送的GET/POST请求，并应用访问规则过滤恶意访问流量。

**说明：** 下述功能项对应不同的（按量付费）收费标准或（包年包月）套餐，您可以在[Web应用防火墙价格页](https://www.aliyun.com/price/product?#/waf/detail)了解具体的计费信息。

-   您可以使用**精准访问控制**，自定义访问规则，过滤客户端IP、请求URL、以及常见的请求头字段。有关操作请参考[精准访问控制](cn.zh-CN/用户指南/防护配置/精准访问控制.md#)、[黑白名单配置](cn.zh-CN/用户指南/防护配置/IP黑白名单配置.md#)。
-   您也可以直接使用Web防护功能，抵御常见的Web攻击。我们结和Web攻击特征，分析请求头和请求主体，编写了精准的过滤算法，并将这些复杂的过滤算法封装各类防护功能，方便您直接使用。WAF提供的防护功能包括：

    **说明：** WAF使用多层过滤的机制，即您在启用WAF并配置防护功能后，一个客户端请求在经过WAF时，实际上按顺序经过了多层过滤。默认的防护检测顺序为：**精准访问控制** \> **CC防护** \> **Web应用攻击防护**。

    -   **Web攻击防护**：帮助您防护SQL注入、XSS跨站攻击等常见的Web攻击。有关操作请参考[Web应用攻击防护规则策略](cn.zh-CN/用户指南/防护配置/Web应用攻击防护.md#)。
    -   **CC攻击防护**：帮助您防护针对页面请求的CC攻击。有关操作请参考[选择CC防护模式](cn.zh-CN/用户指南/防护配置/CC安全防护.md#)、[自定义CC防护](cn.zh-CN/用户指南/防护配置/自定义CC防护.md#)。
    -   **智能防护引擎**：对请求做语义分析，检测经伪装或隐藏的恶意请求，帮助您防护通过攻击混淆、变种等方式发起的恶意攻击。有关操作请参考[新智能防护引擎](cn.zh-CN/用户指南/防护配置/新智能防护引擎.md#)。
    -   **恶意IP惩罚**：帮助您自动封禁在短时间内进行多次Web攻击的客户端IP。有关操作请参考[恶意IP封禁](cn.zh-CN/用户指南/防护配置/恶意IP惩罚.md#)。
    -   **地理IP封禁**：帮助您一键封禁来自指定国内省份或海外地区的IP的访问请求。有关操作请参考[封禁地区](cn.zh-CN/用户指南/防护配置/封禁地区.md#)。
    -   **数据风控**：帮助您对抗机器威胁，如垃圾注册、账号被盗、活动作弊、垃圾消息等欺诈行为。有关操作请参考[数据风控](cn.zh-CN/用户指南/防护配置/数据风控.md#)。
    -   **网页防篡改**：帮助您锁定需要保护的网站页面，被锁定的页面在收到请求时，返回您为其设置的缓存内容。有关操作请参考[网页防篡改](cn.zh-CN/用户指南/防护配置/网站防篡改.md#)。
    -   **敏感信息防泄漏**：帮助您过滤服务器返回内容（异常页面或关键字）中的敏感信息，如身份证号、银行卡号、电话号码、和敏感词汇等。有关操作请参考[防敏感信息泄露](cn.zh-CN/用户指南/防护配置/防敏感信息泄露.md#)。

## 查看安全报表 {#reports .section}

WAF还提供了方便的数据可视化和统计功能：

-   **安全监控**：您可以在[WAF控制台总览页面](cn.zh-CN/用户指南/防护统计/总览.md#)查看图表化的业务访问数据以及安全防护统计信息。
-   **安全报表**：您可以在这里查询您的域名在30天内受到的攻击详情和风险预警信息。有关操作请参考[攻击防护报表](cn.zh-CN/用户指南/防护统计/攻击防护报表.md#)和[风险预警报表](cn.zh-CN/用户指南/防护统计/风险预警报表.md#)。
-   **全量日志**：您可以在这里搜索您的网站日志，并使用在线分析快速定位请求。有关操作请参考[全量日志查询](cn.zh-CN/用户指南/防护统计/全量日志查询.md#)。
-   **数据大屏**：您可以在这里查看WAF的实时攻防态势监控和告警。有关操作请参考[可视化大屏](cn.zh-CN/用户指南/防护统计/数据大屏.md#)。

## WAF最佳实践 {#section_o5b_s5l_l2b .section}

启用WAF后，您的源站服务器收到的所有请求都来自WAF实例，服务器IP对客户端来说是隐藏的。

-   如果您希望查看访问请求的真实客户端IP，请参考[获取访问者真实IP](../../../../../cn.zh-CN/最佳实践/获取访问者真实IP.md#)。
-   如果您的源站服务器IP已公开或不慎泄露，攻击者可能越过域名解析，也即越过WAF，直接对您的源站发动攻击。要想有效防护这种情形，您需要[配置源站保护](../../../../../cn.zh-CN/最佳实践/源站保护.md#)。
-   如果您同时使用了阿里云[DDoS高防IP服务](https://www.aliyun.com/product/ddos)或者[CDN服务](https://www.aliyun.com/product/cdn)，您可能需要以下帮助：

    **说明：** 这种情况下，您在接入WAF时，应当勾选**WAF前是否有七层代理**下的**是**。

    -   [同时部署WAF和DDoS高防IP](cn.zh-CN/用户指南/接入WAF/同时部署WAF和DDoS高防.md#)
    -   [同时部署WAF和CDN](cn.zh-CN/用户指南/接入WAF/同时部署WAF和CDN.md#)
-   如果您希望对您的原生App进行安全防护，解决CC攻击、机器滥刷等问题，您可以参考WAF的[SDK方案](cn.zh-CN/用户指南/SDK方案/SDK方案简介.md#)。

