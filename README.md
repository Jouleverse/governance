# Jouleverse治理系统的核心合约

已经完成的步骤：
1. 创世 - 宇宙大爆炸 - 1000亿Joule创世能量诞生，这就是Jouleverse(焦耳宇宙)全部能量，能量守恒定律
2. 所有Joule转入多签合约，归社区共同控制
3. J-WJ-VJ的治理思路形成，[WJ诞生](https://github.com/jouleverse/WJOULE)，核心(core)释放开始
4. [Tokenomics(代币经济学)定稿](https://github.com/Jouleverse/genesis-treasury)
5. [治理程序完成首次链下模拟运行，完成JIP-5预算1.44亿Joule的批准及首批7200万Joule的拨款](https://github.com/Jouleverse/jips)，检验了治理程序的设计和细节
6. Timelock(时间锁)部署，初步完成对于tokenomics的“永久固定化”

## Timelock

Jouleverse Timelock模块分成两个部署实例：用于锁定200亿Joule能量的Timelock core，以及用于锁定800亿Joule能量的Timelock eco。它们的部署参数、步骤和结果如下：

Timelock core (核心预算时间锁):
- 合约地址：0x628beb88dD440A8c5e4cC89Ab33a041f521e4323
- 代码版本：commit ee05f3e
- 编译参数：0.5.16+commit.9c32226ce, optimization: 200 runs
- 部署参数：admin_ = 0x3B717119878E2db1AA7df46F5AdcF9766A01706F (GT多签合约), 172800 (2天 最低延时), 12000000000000000000000000 (每月1200万Joule)
- 部署后工作：1. 转入1 Joule进行测试，包括预算控制、延时控制等各项功能；2. 调整已消耗预算数值至追平实际用量；<del>3. 将合约控制权转交给创世多签合约，由社区共同控制；</del>4. 向创世多签合约申请3个月共3600万Joule作为第一批试运行资金转入该时间锁定合约；5. 试运行3个月；6. 择机申请将核心预算剩余部分Joule全部转入该时间锁定合约；7. 百年大计完成。
- 其他工作：加入合约工具

Timelock eco (生态发展预算时间锁):
- 合约地址：0xbb6b53Fadf85B73258cb6A54F1343Ac4D5F99773
- 代码版本：commit ee05f3e
- 编译参数：0.5.16+commit.9c32226ce, optimization: 200 runs
- 部署参数：admin_ = 0x3B717119878E2db1AA7df46F5AdcF9766A01706F (GT多签合约), 604800 (7天 最低延时), 48000000000000000000000000 (每月4800万Joule)
- 部署后工作：1. 转入1 Joule进行测试，包括预算控制、延时控制等各项功能；2. 调整已消耗预算数值至追平实际用量；<del>3. 将合约控制权转交给创世多签合约，由社区共同控制；</del>4. 待timelock核心试运行完毕，没有发现任何问题；5. 择机申请将生态发展预算剩余部分Joule全部转入该时间锁定合约；5. 百年大计完成。
- 其他工作：加入合约工具

