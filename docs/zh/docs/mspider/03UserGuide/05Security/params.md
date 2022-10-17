# 安全治理参数配置

本页介绍有关对等身份认证、请求身份认证、授权策略相关的参数配置。

## 对等身份认证

采用向导模式时，对等身份认证分为基本配置和认证配置两步，各参数说明如下：

| **UI 项**               | **YAML 字段**                        | **描述**                                                     |
| ---------------------- | -------------------------------------- | ------------------------------------------------------------ |
| 名称                   | metadata.name | 必填。对等身份认证名称，同一个命名空间下不可重名。<br />格式要求：小写字母、数字和中划线（-）组成，必须以小写字母开头，以小写字母或数字结尾，最长 63 个字符。 |
| 命名空间               | metadata.namespace                     | 必选。对等身份认证所属的命名空间。当选择网格的根命名空间时，将创建全局策略。全局策略仅能创建一个，需在界面中做检查，避免用户重复创建。 |
| 添加工作负载选择标签   | spec.selector                          | 可选。应用对等身份认证策略的工作负载选择标签，可添加多个标签，无需排序。 |
| mTLS 模式               | spec.mTLS.mode                         | 必填。用于命名空间的 mTLS 模式设定，包含四项：<br />- UNSET：继承父类命名空间设定，如果没有，就按 PERMISSIVE 处理；<br />- DISABLE：仅建立明文连接；<br />- PERMISSIVE：可以建立明文或 mTLS 连接；<br />- STRICT：必须建立 mTLS 连接 |
| 为指定端口添加 mTLS 模式 | spec.portLevelMtls                     | 可选。针对指定端口设置 mTLS 规则，可添加多条规则，无需排序。用于指定端口号的 mTLS 规则，包括四项：<br />- UNSET：继承父类命名空间设定，如果没有，就按 PERMISSIVE 处理；<br />- DISABLE：仅建立明文连接；<br />- PERMISSIVE：可以建立明文或 mTLS 连接；<br />- STRICT：必须建立 mTLS 连接 |

## 请求身份认证

采用向导模式时，请求身份认证分为基本配置和认证配置两步，各参数说明如下：

| **UI 项**            | **YAML 字段**                        | **描述**                                                     |
| -------------------- | ------------------------------------ | ------------------------------------------------------------ |
| 名称                 | metadata.name                        | 必填。请求身份认证名称，同一个命名空间下不可重名。<br />格式要求：小写字母、数字和中划线（-）组成，必须以小写字母开头，以小写字母或数字结尾，最长 63 个字符。 |
| 命名空间             | metadata.namespace                   | 必选。请求身份认证所属的命名空间。当选择网格的根命名空间时，将创建全局策略。全局策略仅能创建一个，需在界面中做检查，避免用户重复创建。<br />同一个命名空间内，请求身份认证的名称不能重复。 |
| 添加工作负载选择标签 | spec.selector                        | 可选。应用请求身份认证策略的工作负载选择标签，可添加多条选择标签，无需排序。 |
| 标签名称             | spec.selector.matchLabels            | 必填。由小写字母、数字、连字符（-）、下划线（_）及小数点（.）组成 |
| 标签值               | spec.selector.matchLabels.{标签名称} | 必填。由小写字母、数字、连字符（-）、下划线（_）及小数点（.）组成 |
| 添加 JWT 规则        | spec.jwtRules                        | 可选。用于用户请求认证的 JWT 规则，可添加多条规则。          |
| Issuer               | spec.jwtRules.issuers                | 必填。JSON Web Token (JWT) 签发人信息。                      |
| Audiences            | spec.jwtRules.issuers.Audiences      | 可选。配置可访问的 audiences 列表，如果为空，将访问 service name。 |
| jwkURI               | spec.jwtRules.issuers.jwkUri         | 可选。JSON Web Key (JWK) 的 JSON 文件路径，格式为：URI。     |
| jwks                 | spec.jwtRules.issuers.jwks           | 可选。JSON Web Key Set (JWKS) 文件内容，可以与 jwkURI 二选一提供。 |

## 授权策略

采用向导模式时，授权策略分为基本配置和认证配置两步，各参数说明如下：

| **元素**             | **YAML 字段**                       | **描述**                                                     |
| -------------------- | -------------------------------------- | ------------------------------------------------------------ |
| 名称                 | metadata.name | 必填。授权策略名称  格式要求：小写字母、数字和中划线（-）组成，必须以小写字母开头，以小写字母或数字结尾，最长 63 个字符。 |
| 命名空间             | metadata.namespace                     | 必选。授权策略所属命名空间，当选择网格根命名空间时，将创建全局策略，全局策略仅能创建一个，需在界面做检查，避免用户重复创建。同一个命名空间内，请求身份认证不可重名。 |
| 添加工作负载选择标签 | spec.selector                          | 可选。应用授权策略的工作负载选择标签，可添加多条选择标签，无需排序。 |
| 标签名称             | spec.selector.matchLabels              | 可选。由小写字母、数字、连字符（-）、下划线（_）及小数点（.）组成。 |
| 标签值               | spec.selector.matchLabels.{标签名称}   | 可选。由小写字母、数字、连字符（-）、下划线（_）及小数点（.）组成。 |
| 策略动作             | spec.action                            | 可选。包含：<br />- 允许（allow）<br />- 允许所有（allow-all）<br />- 拒绝（deny）<br />- 拒绝所有（deny-all）<br />- 审计（audit）<br />- 自定义（custom）<br />注意：选择`允许所有`或`拒绝所有`时，`请求来源`、`请求操作`、`策略条件`均置灰。<br />选择自定义时，增加`provider`输入项。 |
| Provider Name        | spec.provider.name                     | 必填。仅在`策略动作`选择为`自定义`时，才显示该输入框。              |
| 添加规则             | spec.rules                             | 可选。授权执行规则包含请求来源、操作、策略条件三部分，可添加添加多条规则，按顺序执行。 |
| 添加请求来源         | spec.rules.-from                       | 可选。请求来源可基于命名空间、IP 段等规则进行定义，可添加多条规则。  |
| 添加请求操作         | spec.rules.-to                         | 可选。请求操作为对筛选出的请求执行的操作，例如发送至指定端口或主机。可添加多个操作。 |
| 添加策略条件         | spec.rules.-when                       | 必填。策略条件是一个可选设置，可以为授权规则增加类似黑名单（values）、白名单的限制条件（notValues），例如增加一个 IP 白名单限制。可添加多个策略条件。 |