# Jouleverse治理系统的核心合约

已经完成的步骤：
1. 创世 - 宇宙大爆炸 - 1000亿Joule创世能量诞生，这就是Jouleverse(焦耳宇宙)全部能量，能量守恒定律
2. 所有Joule转入多签合约，归社区共同控制
3. J-WJ-VJ的治理思路形成，[WJ诞生](https://github.com/jouleverse/WJOULE)，核心(core)释放开始
4. [Tokenomics(代币经济学)定稿](https://github.com/Jouleverse/genesis-treasury)
5. [治理程序完成首次链下模拟运行，完成JIP-5预算1.44亿Joule的批准及首批7200万Joule的拨款](https://github.com/Jouleverse/jips)，检验了治理程序的设计和细节
6. Timelock(时间锁)部署，初步完成对于tokenomics的“永久固定化”

里程碑：
- 2025.4.15：全部Joule完成迁移，从GT迁移到两个时间锁合约中，严格按照Joulenomics（焦耳经济学）释放/解锁，彻底杜绝了多签人的干预可能性

## Timelock

Jouleverse Timelock模块分成两个部署实例：用于锁定200亿Joule能量的Timelock core，以及用于锁定800亿Joule能量的Timelock eco。它们的部署参数、步骤和结果如下：

Timelock core (核心预算时间锁):
- 合约地址：0x628beb88dD440A8c5e4cC89Ab33a041f521e4323
- 代码版本：commit ee05f3e
- 编译参数：0.5.16+commit.9c32226ce, optimization: 200 runs
- 部署参数：admin_ = 0x3B717119878E2db1AA7df46F5AdcF9766A01706F (GT多签合约), 172800 (2天 最低延时), 12000000000000000000000000 (每月1200万Joule)
- 部署后工作：1. 转入1 Joule进行测试，包括预算控制、延时控制等各项功能(GT#27,#29)；2. 调整已消耗预算数值至追平实际用量(GT#42,#43)；<del>3. 将合约控制权转交给创世多签合约，由社区共同控制；</del>4. 向创世多签合约申请3个月共3600万Joule作为第一批试运行资金转入该时间锁定合约(GT#30)；5. 试运行3个月；6. 择机申请将核心预算剩余部分Joule全部转入该时间锁定合约(GT#44)；7. 百年大计完成。
- 其他工作：加入合约工具

Timelock eco (生态发展预算时间锁):
- 合约地址：0xbb6b53Fadf85B73258cb6A54F1343Ac4D5F99773
- 代码版本：commit ee05f3e
- 编译参数：0.5.16+commit.9c32226ce, optimization: 200 runs
- 部署参数：admin_ = 0x3B717119878E2db1AA7df46F5AdcF9766A01706F (GT多签合约), 604800 (7天 最低延时), 48000000000000000000000000 (每月4800万Joule)
- 部署后工作：1. 转入1 Joule进行测试，包括预算控制、延时控制等各项功能(GT#28,#31)；2. 调整已消耗预算数值至追平实际用量(GT#49,#52)；<del>3. 将合约控制权转交给创世多签合约，由社区共同控制；</del>4. 待timelock核心试运行完毕，没有发现任何问题；5. 择机申请将生态发展预算剩余部分Joule全部转入该时间锁定合约(GT#53)；5. 百年大计完成。
- 其他工作：加入合约工具

---
2024.10.29 GT(创世金库)剩余全部Joule转入Timelock时间锁进行锁定释放(一旦完成，经济模型将永不可改）的执行方案（草）：

1. 请有能力的朋友再次审查一下Timelock的合约代码，确保万无一失：https://github.com/Jouleverse/governance/blob/master/contracts/Timelock.sol

2. 同步地，所有人，审计一下当前状态和关键数据计算，确保无误：

2.1 GT剩余Joule量：记账数值（https://github.com/Jouleverse/genesis-treasury/blob/main/README.md ）与链上数值（0x3B717119878E2db1AA7df46F5AdcF9766A01706F ）是否一致？

GT历史支出：3.36亿Joule 
- 核心：3 + 12 + 7 = 22个月 x 1200万 = 2.64亿Joule
- 生态：7200万 = 0.72亿Joule

GT应剩余：1000亿 - 3.36亿 = 996.64亿Joule
- 归属核心：200亿 - 2.64亿 = 197.36亿Joule
- 归属生态：800亿 - 0.72亿 = 799.28亿Joule

2.2 Timelock核心的释放（合约 released 方法）和已用数值（合约 used 方法）的链上真实数值，是否正确？

- released: 2.88亿Joule（2024.10月查询结果；每个月会增加0.12亿）
- used: 3600万 + 1 = 36000001 Joule（3600万是试运行3个月发放的预算；1是最初测试的1 Joule支出）

与实际支出对比，计算出应补差值：delta = 2.64亿 - (3600万 + 1) = 2.64亿 - 0.36亿 - 1 = 2.28亿 - 1 = 227999999 Joule

2.3 Timelock生态的释放（合约 released 方法）和已用数值（合约 used 方法）的链上真实数值，是否正确？

- released: 11.52亿Joule（2024.10月查询结果；每个月会增加0.48亿）
- used: 1 Joule（1是最初测试的1 Joule支出）

与实际支出对比，计算出应补差值：delta = 0.72亿 - 1 = 71999999 Joule

3. 链上执行

3.1 对于Timelock核心，应执行：

(1) 调用Timelock核心合约的incUsage方法，传入参数 delta * 10^18 = 227999999 * 10^18 （需要用GT调用其queueTransaction执行incUsage）

(2) 从GT合约转入归属核心的197.36亿Joule到Timelock核心合约

(3) 执行完成后，检查各项状态数值，确认正确无误：

- GT合约余额：799.28亿Joule
- Timelock核心合约余额：197.36亿Joule
- Timelock核心合约的used方法返回值：2.64亿Joule

3.2 对于Timelock生态，应执行：

(1) 调用Timelock生态合约的incUsage方法，传入参数 delta * 10^18 = 71999999 * 10^18 （需要用GT调用其queueTransaction执行incUsage）

(2) 从GT合约转入归属生态的799.28亿Joule到Timelock生态合约

(3) 执行完成后，检查各项状态数值，确认正确无误：

- GT合约余额：0 Joule
- Timelock生态合约余额：799.28亿Joule
- Timelock生态合约的used方法返回值：0.72亿Joule

3.3 更新github: https://github.com/Jouleverse/genesis-treasury/blob/main/README.md

