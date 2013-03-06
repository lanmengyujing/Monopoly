#项目名称
##大富翁
#开发要求
##开发语言为Java
##编写Clean Code
##编写测试
##有构建脚本
##提示

在Java中实现命令行有颜色输出，可以寻找一些现有的库辅助实现。

需求方面，存在疑问，可以在邮件列表中提问。

在实现时有什么问题，可以再邮件列表中提问，也可以找buddy提问。
项目描述

##一、简介

大富翁是一款经典的策略游戏，相信不少人都玩过。

该游戏类似于飞行棋，多个玩家轮流掷骰子，按点数在地图上行走，可以买地、盖房子、使用道具等。途径其它玩家的地盘，需支付费用。最终破产者为输，坚持到最后的玩家为胜利者。

该项目开发一个完全基于命令行的大富翁游戏，实现传统大富翁游戏的一些
基本功能，如买卖地产、使用道具（路障、炸弹、机器娃娃等）。不涉及图形界面开发，因此，参与者不需要有图形库使用的经验。

##二 、游戏功能描述

###1.玩家

游戏支持2～4个玩家。不需支持电脑玩家。

###2.地图

  S0000000000000H0000000000000T
  $           地段一                地段二       0
  $                                                       0
  $                                     地段三       0
  $                                                       0
  $                                                       0
  $           地段四                地段五       0
  M0000000000000P0000000000000G

初始地图如上所示，是一个简单的矩形，玩家顺时针行走。
地形有如下几种：
S：起点。
0：空地（玩家可以买卖、建设），共有5个地段。地段3有6块空地，其它4个地段各是13块连续空
地。地段1、地段2的地皮价格为每块空地200元；地段3为黄金地段，每块空地500元；地段4、地段5每块空地300元。
T： 道具屋
G：礼品屋
M：魔法屋
H：医院：玩家被炸弹炸伤后，将被送到医院医治3天（即轮空3次），出院后，从医院出发。
P： 监狱：刚好走到该处，玩家被扣留两天。
$：矿地，可获取点数。从下之上依次为：20、80、100、40、80、60点。
每块地都有一编号，起点S编号为0，顺时针递增，最 后一块矿地$编号69。

###3.资产

玩家有四类资产：
资金：每位玩家默认初始资金10000。资金可用于购地，盖 房，支付过路费。
点数：初始点数为0，经过矿地时可获取点数。点数用于购买道具。
固定资产：玩家占有的土地、楼房。
道 具：如路障、炸弹等。

###4.道具

路障：玩家拥有路障后，可将路障放置到离当前位置前后10步的距离，任一玩家经过路障，都将被拦截。

炸弹：可将炸弹放置到离 当前位置前后10步的距离，任一玩家经过该位置，将被炸伤（路过就炸，而不是刚好走到该位置才炸），送往医院，住院三天。炸
弹和路障不可放置在有玩家的位置，一个位置只允许放置一个道具。

机器娃娃：使用该道具，可清扫前方路面上10步以内（包括10）的 其它道具，如炸弹、路障。以上道具一次有效。

###5.礼品

奖金：2000元
点数卡：200点
福神： 福神俯身，路过其它玩家地盘，均可免费。5轮内有效（在监狱、医院中也计轮次）。
玩 家进入礼品屋，可任选一件礼品。

###6.魔法屋
玩家进入魔法屋，可施展魔法。

##三、游戏规则和命令解释

###1.运行游戏：rich（rich为游戏名）

###2.设置玩家初始资金，范围1000～50000（默认10000）

###3.选择玩家

系统提示：“请选择2~4位不重复玩家，输入编号即可。(1.钱夫人; 2.阿土伯; 3.孙小美; 4.金贝贝):”：如输入12。

###4.行走

控制台显示当前玩家姓名和标识，如：
钱夫人>待输入命令

命令：Roll。掷骰子行走1~6步。步数由随即 算法产生。如果途中碰到路障，停留在路障处。 玩家显示的符号覆盖当前位置的地产符号。
轮到当前玩家时，在掷骰子之前，可以进 行其它诸如使用道具，卖房产，查询等操作。掷完骰子后，切换到下一玩家。

玩家在地图上显示为名字拼音的缩写，如阿土伯显示为A

###5.建设

玩家停留在一块未被购买的空地时，系统提示：“是否购买该处空地，xxx元（Y/N）?”玩家输入Y或者N。输入Y时，地图上的空地符号0变为当前玩家的颜色（ 比如用红色表示孙小美），表示被该玩家占有,系统扣除玩家相应的资金xxx元。

玩家停留在自己的空地或楼房处，系统提示：“是否升级该处地产，xxx元（Y/N）?”

输入Y时，地图上表示该位置的符号变为 相应级别房屋的符号，房屋总共分为三级，每升一级的费用同购买空地的费用，各级符号参考下表：

空地     0
茅屋     1
洋房     2
摩天楼  3

停留在其它玩家的空地或楼房处，需支付相应级别的费用。停留在其
它玩家空地处，支付该处空地购买价的1/2，地产每升一级，费用翻倍。如果福神俯身，可免费，系统提示：“福神附身，可免过路费”；
如果地产的主人在医院或监狱中，也免过路费。

###6.购买道具

玩家进入道具屋时，系统提示：“欢迎光临道具屋， 请选择您所需要的道具：”。
通过输入道具的编号选择道 具，每位玩家最多可以拥有10个道具。
道具        编号    价值（点数）    显示方式
路障          1         50                         ＃
机器娃娃      2         30
炸 弹         3         50                         @
如果玩家点数不足，系统提示：“您当前剩余的点数为xxx， 不足以购买yyy道具。”。如果玩家点数不足买点数最少的道具，自
动退出道具屋。按“F”可手工退出道具屋。

###7.获赠礼品

玩家进入礼品屋时，系统提示：“欢迎光临礼品屋，请选择一件您 喜欢的礼品：”
通过输入礼品编号选择礼品：
礼品    编号
奖金       1
点数卡   2
福神       3
只能选择一件礼品，选择后，自动退出礼品屋（输入错误，也退出，就当放弃此次机会），礼品即时生效。下个玩家 继续。

###8.使用魔法

玩家进入魔法屋，可施展魔法。具体施展什么样的魔法， 客户还没想好。

###9.使用路障

命令： block n 。n指定与当前位置的相对距离，范围 [-10~10]。

###10.使用炸弹

命令： bomb  n 。n指定与当前位置的相对距离，范围 [-10~10]。

###11.使用机器娃娃
命令： robot。清除前方10步内的任何障碍，如路障、炸弹。

###12.出售道具

命令： sellTool x。出售道具。x为道具编号。

###13.出售房产

当玩家资金紧张时，可出售自己的 房产。
命令：sell n 。 n 指定待售地产的位置编号。出售价格为该处房产总成本（包括购地，升级）的2倍。出售完后，该处地产
还原为未售空地，即在地图上使用0表示。

###14.查询资产
命令：query。
显示自家资产信息：
资 金：xxxxx元
点数：xxx点
地产：空地x处；茅屋x处；洋房y处；摩天楼z处。
道具：路障 x个；炸弹y个；机器娃娃z个

###15.帮助

命令：help
显 示各个命令的使用说明。

###16.退出游戏
命令：quit
执行该命令，强制结束游戏。

###17.游戏结束
玩 家资金低于0时，即宣告破产。该玩家占有的土地归还系统，初始化为空地，可供其它玩家重新购买。
只剩一个玩家时，游戏结束。该玩家获 胜。

###18.以上所涉及的所有命令忽略大小写。所涉及的系统提示信息， 可自由发挥，力求完备而不冗余。

四、附录
命令一览表
>roll          掷骰子命令，行走1~6步。步数由随即算法产生。

>block n       玩家拥有路障后，可将路障放置到离当前位置前后10步的距离，任一玩家经过路障，都将被拦截。该道具一次有效。n前后的相对距离，负数表示后方。

>bomb n        可将路障放置到离当前位置前后10步的距离，任一玩家j 经过在该位置，将被炸伤，送往医院，住院三天。n 前后的相对距离，负数表示后方。

>robot         使用该道具，可清扫前方路面上10步以内的其它道具，如炸弹、路障。

>sell x        出售自己的房产，x 地图上的绝对位置，即地产的编号。

>sellTool x    出售道具，x 道具编号

>query         显示自家资产信息

>help          查看命令帮助

>quit          强制退出
