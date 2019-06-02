# 部署

### 部署策略

- Big bang deployment：一次性全量
- Rolling Deployment：增量部署
- Blue-Green, Red-Black or A/B Deployment
    + 将服务分为两组：A 和 B
    + 将所有流量分配到 A
    + 部署代码到 B，测试无误后，将流量切到 B
    + 交替进行
- Canary Deployment：灰度
    + 将服务分为两组：当前版（旧代码）和灰度版（新代码）
    + 将部分流量分配到灰度版
    + 可随时控制分流比例
    + 逐步开量，直到全量更新
