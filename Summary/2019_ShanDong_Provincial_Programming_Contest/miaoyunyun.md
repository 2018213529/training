# 5.11
说实话，中午饭不好吃。。。
## 热身赛
我们仨刚坐下没一会，有一个老师就问我们学校是不是要改成财富管理大学了。。。原来这么出名了吗。
前三个题都是做过的，不过还是WA了一发QAQ，还有最后一个题真的好毒。然后又试了试打印，看榜单，瞎测数据各种。
# 5.12
酒店早饭一水儿的素，难过。
## 正式赛 
开始我们就在草稿纸上写上：调试完一定要改回数据量，交C++，用scanf、printf等，放在显眼的地方，这些都是我们平时比赛中容易犯的错误。
1. 首先翻译的红色气球M题，首先看到k是1e9,不能暴力吧，总不能循环1e9次吧，然后赵晨阳说反正是除2,到1跳出就行，最多几十次循环，也对哦/笑哭/，这个题1发A。
2. 这个时候F题和A题都翻译出来了，我们先做的A题，这个题卡了好长时间，刚开始没有考虑给出的时间靠后，让求的时间靠前的情况，后来在加上相隔的天数还是减去相隔的天数那里WA了一发，然后我们三个从头捋了一遍，改了些细节才A。此时绝大部分队都已经出了这个题，我们一直在告诉队友别慌别慌。
3. F题差不多算是直接敲的吧，刚开始敲的时候，思路还是有点子绕的，敲着敲着恍然大悟1发A。
4. D题这个题真的WA的不该，思路简单，当时我问了一嘴，会不会循环删除，刘素芹题目上标注了那个mod k，说不会循环，我大致扫了一遍题面，没注意到mod k ，然后悲剧就发生了QAQ，然后仔细的看了一遍，才发现需要求余，加了个求余就A了，我俩那个后悔呀/哭/。
5. C题WA了一发，刚开始想的贼麻烦，并且赵晨阳出的样例也没过，一直卡了好久，后来赵说求一轮的最远的一个点，最后这个点就是最远的，还没开始敲就给否决了/笑哭/，感觉她的这个想法点醒了我，既然可以求一个最远点，那直接把一轮所有的点与原点的距离再根据总的趋势算出来最后的点的坐标就好了。其中有一发WA是忘记算最后一个点了，之后的WA都是因为没有把第一轮的点的距离算上，A的时候已经封榜了，A之前说着不慌，其实还是挺慌的，当时我们名次贼差呀，与牌无缘。
6. 这个时候还有半个小时？刘已经翻译好L、H了，我们转战L题，当时感觉挺简单的啊。。。刘说是弗洛伊德背包传递，直接让她敲，一直改一直交，最后五分钟的时候直接改完编译提交，59分的时候还交了一发，反正最后也没A/哭/ 。
回来看了个题解，用的也是弗洛伊德，两个地方不对，一个是小于等于但我们写的等于，一个在传递的时候少加了个判断条件QAQ。。。太菜了 
后来发现题意不大对，只要这个数有可能是中位数就输出1,我们理解的是，只有确定该数是中位数才输出1。
# 感想
5题铜。我们都尽力了，一直到最后一刻也没有放弃，但确实也菜的真实，出题慢，但是罚时比训练的时候好多了（算是一个进步吧QAQ），交流也多，没跟以前似的各想各的，发挥正常吧，爱我的队友/比心/ 还有下次女生赛加油。

