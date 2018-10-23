# diceol
eosforce 上部署的一个骰子游戏，挺无聊的。支持scatter/麦子钱包，给大家作个参考吧。  

没有使用EOS提供的两次提交随机种子的方式，而是通过链外resolver定期结算。每隔12秒的block的hash值，被resolver作为随机种子灌入合约，结算。  

单次投注，用block hash拼接上投注id，然后计算sh256,然后按字节累加求和，对100求模+1,得到结果。  

多人投注，用block hash计算sh256,然后按字节累加求和，对10或者100求模后+1,得到结果。  

上述过程简单，不如全部在链上有公信力。但是因为上述流程和数据都是可以追查的，还是比较可信的。  

为了尽快结算，所以结算没有等到块不可逆。如果要等到不可逆块，预计需要90秒以上的延时，更难接受。  

由于会以接入节点提供的未到不可逆的块的hash作为随机种子，所以，可能出现个别结算使用的block hash，和最终链上不可逆块的hash值不一致。  

这点应该不是大问题，首先这个hash也是源于某个节点计算出来的，也具有足够的随机性和公信力；其次，使用resolve，可以在本地记录指定节点的历史block hash值以进行检查，也是可以事后验证的。  
