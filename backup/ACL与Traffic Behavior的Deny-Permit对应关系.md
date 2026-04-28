<html><body>
<!--StartFragment--><div class="ds-markdown ds-markdown--block" style="--ds-md-zoom: 1.143;"><h1>ACL与Traffic Behavior的Deny/Permit对应关系</h1><p>ACL和Traffic Behavior中都有deny/permit的概念，但它们的含义和作用不同。以下是它们的对应关系表格：</p><h2>Deny/Permit对应关系表</h2>

ACL规则动作 | Traffic Behavior动作 | 最终结果 | 说明
-- | -- | -- | --
permit | permit | 允许通过 | ACL允许且Behavior也允许，流量正常通过
permit | deny | 拒绝通过 | ACL允许但Behavior拒绝，最终拒绝流量
deny | (任何Behavior动作) | 拒绝通过 | ACL直接拒绝，不再检查Behavior
(无匹配规则) | (不适用) | 隐含拒绝 | 默认情况下ACL对未匹配的流量是deny

<h2>详细说明</h2><ol start="1"><li><p><strong>ACL permit + Behavior permit</strong></p><ul><li><p>流量被允许且可能应用其他策略(如QoS)</p></li><li><p>示例：允许特定IP并限制其带宽</p></li></ul></li><li><p><strong>ACL permit + Behavior deny</strong></p><ul><li><p>虽然ACL允许，但Behavior明确拒绝</p></li><li><p>可用于例外情况的特殊处理</p></li></ul></li><li><p><strong>ACL deny</strong></p><ul><li><p>直接拒绝，不检查后续Behavior</p></li><li><p>效率最高的一种拒绝方式</p></li></ul></li><li><p><strong>无匹配规则</strong></p><ul><li><p>大多数网络设备ACL默认隐含deny any规则</p></li><li><p>等同于显式的<code>deny</code>动作</p></li></ul></li></ol>
# 配置示例

``` 
示例1：ACL permit + Behavior permit
acl number 2001
rule permit source 192.168.1.10 0

 traffic behavior bh-permit
 permit
car cir 2000  # 允许但限速2Mbps

 示例2：ACL permit + Behavior deny
acl number 2002
 rule permit source 192.168.1.20 0

 traffic behavior bh-deny
 deny  # 明确拒绝

 应用策略
 traffic policy pl-test
 classifier 2001 behavior bh-permit
classifier 2002 behavior bh-deny
```

</pre></div><h2>注意事项</h2><ol start="1"><li><p>ACL的deny动作优先级最高，会跳过Behavior处理</p></li><li><p>当需要复杂控制时，可以结合使用ACL permit和Behavior deny</p></li><li><p>华为/H3C设备中这种机制较为常见，不同厂商实现可能略有差异</p></li></ol></div><!--EndFragment-->
</body>
</html>