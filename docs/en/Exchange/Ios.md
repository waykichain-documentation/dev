# iOS Library
#### IOS Wallet Offline Signature Library Development Guide

## Introduction
When developers need to use IOS native client to `generate mnemonic`, `mnemonic To private key`, `create address`, `registered address`, `transfer`, `deployment contract`, `call contract`, `vote`, etc., WaykiChain provides IOS's offline signature library for its use.

## Sourcecode and library

IOS native wallet offline signature library compiled based on `Golang` language offline signature library. source code:

https://github.com/WaykiChain/wicc-wallet-utils-go

IOS native wallet offline signature library download url:

https://github.com/WaykiChain/wicc-wallet-utils-go/releases/download/v1.0/ios-wiccwallet.framework.zip

## Instructions
### Bridge.h 

```
//
//  Bridge.h
//  CommApp
//
//  Created by sorath on 2019/3/6.
//  Copyright © 2019 sorath. All rights reserved.
//

#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface Bridge : NSObject
//创建钱包、助记词
+ (NSString *)createNewWalletMnemonics;

//获取钱包地址和私钥
+ (NSArray *)getAddressAndPrivateKeyWithHelpString:(NSString *)helpStr;

//获取钱包哈希
+ (NSString *)getWalletHashFrom:(NSString *)codestring;

//检验钱包地址格式
+ (BOOL)addressIsAble:(NSString *)address;

//检查助记词列表

+(BOOL) checkMnemonicCode:(NSString*)words;

//打乱助记词数组词语顺序
+ (NSArray *)getRamdomArrayWithArray:(NSArray *)array;

//获取激活hex
+ (NSString *)getActrivateHexWithHelpStr:(NSString *)helpStr Fees:(double)fees validHeight:(double)validHeight ;

//获取转账wicc hex
+ (NSString *)getTransfetWICCHexWithHelpStr:(NSString *)helpStr Fees:(double)fees validHeight:(double)validHeight srcRegId:(NSString *)srcRegId destAddr:(NSString *)destAddr transferValue:(double)value ;


// 获取锁仓的hex
+ (NSString *)getLockHexWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appId:(NSString *)appId transferValue:(double)value;

// 获取解仓的hex
+ (NSString *)getUnlockHexWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appId:(NSString *)appId destAddr:(NSString *)destAddr transferValue:(double)value;

//获取 wicc兑换wusd 的 hex(string) 和 合约命令(str)
+ (NSArray *)getExchangeHexWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appid:(NSString *)appid exchangeValue:(double)value fee:(double)fee rate:(double)rate exchangeToken:(double)tokenNum;

//获取合约签名
+ (NSString *)getContractHexByContractStrWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appid:(NSString *)appid contractStr:(NSString *)contractStr handleValue:(double)value fee:(double)fee;

///发布合约
+ (NSString *)contractPub:(NSString *)helpStr blockHeight:(double)validHeight regessId:(NSString *)regessID contractStr:(NSString *)contractStr desc:(NSString *)desc fee:(double)fee;

///投票
+ (NSString *)nodeVote:(NSString *)helpStr blockHeight:(double)validHeight regessId:(NSString *)regessID beVotePubkey:(NSArray *)beVotePubkey beVoteAmounts:(NSArray *)beVoteAmounts fee:(double)fee;


@end

NS_ASSUME_NONNULL_END

```


##### 2. Bridge.m

```
//
//  Bridge.m
//  CommApp
//
//  Created by sorath on 2019/3/6.
//  Copyright © 2019 sorath. All rights reserved.
//

#import "Bridge.h"

#import "CommApp-Swift.h"
#import "Wiccwallet.framework/Headers/Wiccwallet.h"

@implementation Bridge
//NetType：   1测试网 2 主网
+ (long )getNetType{
    if ([OtherTools getWalletConfigure] == 1) {
        return 1;
    }
    return 2;
}


//根据助记词获取私钥和地址
+ (NSArray *)getAddressAndPrivateKeyWithHelpString:(NSString *)helpStr{
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    NSString *address = WiccwalletGetAddressFromMnemonic(helpStr,[self getNetType]);
    
    return  @[address,privateKey];
}

+ (NSString *)createNewWalletMnemonics{
    NSString *mnemonics = WiccwalletGenerateMnemonics();
    return mnemonics;
}

+ (NSArray *)getWalletHelpCodesFrom:(NSString *)codestring
{
    NSArray *arr = [codestring componentsSeparatedByString:@" "];
    NSMutableArray *arr1 = [NSMutableArray arrayWithArray:arr];
    [arr1 removeObject:@" "];
    [arr1 removeObject:@""];
    return arr1;
}

+ (NSString *)getWalletHashFrom:(NSString *)codestring
{
    NSArray * helpcodes = [self getWalletHelpCodesFrom:codestring];
    int size = (int)helpcodes.count;
    int i = 0;
    NSMutableString* strWords = [[NSMutableString alloc]init];
    for(id word in helpcodes) {
        [strWords appendString:word];
        if(i != size - 1) {
            [strWords appendString:@" "];
        }
        i++;
    }
    return  strWords.sha512String;
}

+ (NSString *)getWaletHelpStringWithCodes:(NSArray *)helpcodes{
    // 产生钱包字符串
    int size = (int)helpcodes.count;
    int i = 0;
    NSMutableString* strWords = [[NSMutableString alloc]init];
    for(id word in helpcodes) {
        [strWords appendString:word];
        if(i != size - 1) {
            [strWords appendString:@" "];
        }
        i++;
    }
    return  strWords;
}

+ (BOOL)addressIsAble:(NSString *)address{
    return YES;
}

//检验助记词
+(BOOL) checkMnemonicCode:(NSString*)words{
    BOOL isTrue = NO;
    NSString *address = WiccwalletGetAddressFromMnemonic(words, [self getNetType]);
    if (![address  isEqual: @""]){
        isTrue = YES;
    }
    return isTrue;
}

+ (NSArray *)getRamdomArrayWithArray:(NSArray *)array
{
    //随机数从这里边产生
    NSMutableArray *startArray=[NSMutableArray arrayWithArray:array];
    //随机数产生结果
    NSMutableArray *resultArray= [NSMutableArray array];
    //随机数个数
    for (int i=0; i<array.count; i++) {
        int t=arc4random()%startArray.count;
        resultArray[i]=startArray[t];
        startArray[t]=[startArray lastObject]; //为更好的乱序，故交换下位置
        [startArray removeLastObject];
    }
    return resultArray;
}


# pragma mark - 获取投注的签名



//获取激活hex
+ (NSString *)getActrivateHexWithHelpStr:(NSString *)helpStr Fees:(double)fees validHeight:(double)validHeight{
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    NSInteger fee = fees*100000000+(arc4random() % 100);
    WiccwalletRegisterAccountTxParam * para = [[WiccwalletRegisterAccountTxParam alloc] init];
    para.fees = fee;
    para.validHeight = validHeight;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignRegisterAccountTx(privateKey,para,&error);
    return hex;
}

//获取转账wicc hex
+ (NSString *)getTransfetWICCHexWithHelpStr:(NSString *)helpStr Fees:(double)fees validHeight:(double)validHeight srcRegId:(NSString *)srcRegId destAddr:(NSString *)destAddr transferValue:(double)value {
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    UInt64 fee = fees*100000000+(NSInteger)(arc4random() % 100);
    UInt64 transferValue = (NSInteger)(value*100000000);
    WiccwalletCommonTxParam * para = [[WiccwalletCommonTxParam alloc] init];
    para.fees = fee;
    para.validHeight = validHeight;
    para.values = transferValue;
    para.srcRegId = srcRegId;
    para.destAddr = destAddr;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignCommonTx(privateKey,para,&error);
    
    return hex;
}

//获取锁仓签名
+ (NSString *)getLockHexWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appId:(NSString *)appId transferValue:(double)value {
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    UInt64 fee = 0.01 * 100000000+(NSInteger)(arc4random() % 100);
    UInt64 lockValue = (NSInteger)(value*100000000);
    NSString *contractStr = [self getLockContractStrWithCount:value];
    WiccwalletCallContractTxParam* para = [[WiccwalletCallContractTxParam alloc] init];
    para.fees = fee;
    para.validHeight = validHeight;
    para.srcRegId = regessID;
    para.values = lockValue;
    para.appId = appId;
    para.contractHex = contractStr;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignCallContractTx(privateKey,para,&error);
    return hex;
    
}

+ (NSString *)getUnlockHexWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appId:(NSString *)appId destAddr:(NSString *)destAddr transferValue:(double)value {
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    UInt64 fee = 0.01 * 100000000+(NSInteger)(arc4random() % 100);
    UInt64 unlockValue = (NSInteger)(value*100000000);
    NSString *contractStr = [self getUnlockContractStrWithCount:value];
    WiccwalletCallContractTxParam* para = [[WiccwalletCallContractTxParam alloc] init];
    para.fees = fee;
    para.validHeight = validHeight;
    para.srcRegId = regessID;
    para.values = unlockValue;
    para.appId = appId;
    para.contractHex = contractStr;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignCallContractTx(privateKey,para,&error);
    return hex;
}



//wicc兑换wusd
+ (NSArray *)getExchangeHexWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appid:(NSString *)appid exchangeValue:(double)value fee:(double)fee rate:(double)rate exchangeToken:(double)tokenNum{
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    UInt64 exFee = fee    * 100000000 + (NSInteger)(arc4random() % 100);
    UInt64 exchangeValue = (NSInteger)(value*100000000);
    NSString *contractStr = [self getExchangeStrWithEXRate:rate andTokenNum:tokenNum];
    WiccwalletCallContractTxParam* para = [[WiccwalletCallContractTxParam alloc] init];
    para.fees = exFee;
    para.validHeight = validHeight;
    para.srcRegId = regessID;
    para.values = exchangeValue;
    para.appId = appid;
    para.contractHex = contractStr;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignCallContractTx(privateKey,para,&error);
    
    return @[hex,contractStr];
    
}

//获取合约签名
+ (NSString *)getContractHexByContractStrWithHelpStr:(NSString *)helpStr blockHeight:(double)validHeight regessID:(NSString *)regessID appid:(NSString *)appid contractStr:(NSString *)contractStr handleValue:(double)value fee:(double)fee{
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    UInt64 exFee = fee    * 100000000 + (NSInteger)(arc4random() % 100);
    UInt64 exchangeValue = (NSInteger)(value*100000000);
    WiccwalletCallContractTxParam* para = [[WiccwalletCallContractTxParam alloc] init];
    para.fees = exFee;
    para.validHeight = validHeight;
    para.srcRegId = regessID;
    para.values = exchangeValue;
    para.appId = appid;
    para.contractHex = contractStr;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignCallContractTx(privateKey,para,&error);
    NSDictionary * errorMSg = error.userInfo;
    NSString *msg = errorMSg[@"NSLocalizedDescription"];
    if (msg){
        return [NSString stringWithFormat:@"error-%@",msg];
    }
    return hex;
}

//发布合约
+ (NSString *)contractPub:(NSString *)helpStr blockHeight:(double)validHeight regessId:(NSString *)regessID contractStr:(NSString *)contractStr desc:(NSString *)desc fee:(double)fee{
    
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr, [self getNetType]);
    UInt64 exFee = fee    * 100000000 + (NSInteger)(arc4random() % 100);
    WiccwalletRegisterContractTxParam* para = [[WiccwalletRegisterContractTxParam alloc] init];
    para.fees = exFee;
    para.validHeight = validHeight;
    para.srcRegId = regessID;
    NSData * data = [contractStr dataUsingEncoding:NSUTF8StringEncoding];
    
    para.script = data;
    para.description = desc;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignRegisterContractTx(privateKey,para,&error);
    NSDictionary * errorMSg = error.userInfo;
    NSString *msg = errorMSg[@"NSLocalizedDescription"];
    if (msg){
        return [NSString stringWithFormat:@"error-%@",msg];
    }
    return hex;
}


//节点投票
+ (NSString *)nodeVote:(NSString *)helpStr blockHeight:(double)validHeight regessId:(NSString *)regessID beVotePubkey:(NSArray *)beVotePubkey beVoteAmounts:(NSArray *)beVoteAmounts fee:(double)fee{
    
    NSString *privateKey = WiccwalletGetPrivateKeyFromMnemonic(helpStr,[self getNetType]);
    UInt64 exFee = fee * 100000000 + (NSInteger)(arc4random() % 100);
    WiccwalletDelegateTxParam * para = [[WiccwalletDelegateTxParam alloc] init];
    para.validHeight = validHeight;
    para.srcRegId = regessID;
    para.fees = exFee;

    WiccwalletOperVoteFunds * voteFounds = [[WiccwalletOperVoteFunds alloc] init];
    for(int i = 0 ; i < beVotePubkey.count ; i++){
        WiccwalletOperVoteFund * found = [[WiccwalletOperVoteFund alloc] init];
        UInt64 amount = [beVoteAmounts[i] doubleValue];
        found.pubKey = [self convertHexStrToData:beVotePubkey[i]];
        found.voteValue = amount;
        [voteFounds add:found];
       
    }
    para.votes = voteFounds;
    NSError *error = [[NSError alloc] init];
    NSString *hex =  WiccwalletSignDelegateTx(privateKey, para , &error);
    NSDictionary * errorMSg = error.userInfo;
    NSString *msg = errorMSg[@"NSLocalizedDescription"];
    if (msg){
        return [NSString stringWithFormat:@"error-%@",msg];
    }
    return hex;
}




+ (NSString *)getLockContractStrWithCount:(double)value
{
    NSString * header = @"f202";
    NSString * hexs = [self int64ToHex:(NSInteger)(value*100000000)];
    NSString * countreverse = [self hexToReverseHex:hexs];
    NSString * hexReveseS = [self resultHexString:countreverse];
    NSString * all = [NSString stringWithFormat:@"%@%@",header,hexReveseS];
    
    return all;
}

+ (NSString *)getUnlockContractStrWithCount:(double)value{
    NSString * header = @"f203";
    //    NSString * hexs = [self int64ToHex:UInt64(value*100000000)];
    //    NSString * countreverse = [self hexToReverseHex:hexs];
    NSString * hexReveseS = @"";// [self resultHexString:countreverse];
    NSString * all = [NSString stringWithFormat:@"%@%@",header,hexReveseS];
    return all;
}


+(NSString *)getExchangeStrWithEXRate:(double)rate andTokenNum:(double)tokenNum{
    //魔法数 + 方法名 + 预留参数
    NSString * header = @"f0040000";
    //汇率，先*10000，再转化为16进制，再网络序
    NSString * hex1 = [self int64ToHex:(NSInteger)(rate*10000)];
    NSString * countreverse1 = [self hexToReverseHex:hex1];
    NSString * hexReveseS1 = [self resultHexString:countreverse1];
    
    //兑换的token数量，先*100000000，再转化为16进制，再网络序
    NSString * hex2 = [self int64ToHex:(NSInteger)(tokenNum*100000000)];
    NSString * countreverse2 = [self hexToReverseHex:hex2];
    NSString * hexReveseS2 = [self resultHexString:countreverse2];
    
    NSString * all = [NSString stringWithFormat:@"%@%@%@",header,hexReveseS1,hexReveseS2];
    return all;
}

//兑换组装合约参数
+ (NSMutableData *)getExchangeDataWithEXRate:(double)rate andTokenNum:(double)tokenNum{
    NSString *all = [self getExchangeStrWithEXRate:rate andTokenNum:tokenNum];
    return [NSMutableData dataWithData:[self convertHexStrToData:all]];
}

+ (NSString *)int64ToHex:(int64_t)tmpid
{
    NSString *nLetterValue;
    NSString *str =@"";
    int64_t ttmpig;
    for (int i = 0; i<19; i++) {
        ttmpig=tmpid%16;
        tmpid=tmpid/16;
        switch (ttmpig) {
            case 10:
                nLetterValue =@"a";break;
            case 11:
                nLetterValue =@"b";break;
            case 12:
                nLetterValue =@"c";break;
            case 13:
                nLetterValue =@"d";break;
            case 14:
                nLetterValue =@"e";break;
            case 15:
                nLetterValue =@"f";break;
            default:
                nLetterValue = [NSString stringWithFormat:@"%lld",ttmpig];
        }
        str = [nLetterValue stringByAppendingString:str];
        if (tmpid == 0) {
            break;
        }
    }
    return str;
}

+ (NSString *)resultHexString:(NSString *)str{
    NSString * result = [NSString new];
    if (str.length == 0){
        result = [NSString stringWithFormat:@"%@%@",str,@"0000000000000000"];
    }else if (str.length == 1){
        result = [NSString stringWithFormat:@"%@%@",str,@"000000000000000"];
    }else if (str.length == 2){
        result = [NSString stringWithFormat:@"%@%@",str,@"00000000000000"];
    }else if (str.length == 3){
        result = [NSString stringWithFormat:@"%@%@",str,@"0000000000000"];
    }else if (str.length == 4){
        result = [NSString stringWithFormat:@"%@%@",str,@"000000000000"];
    }else if (str.length == 5){
        result = [NSString stringWithFormat:@"%@%@",str,@"00000000000"];
    }else if (str.length == 6){
        result = [NSString stringWithFormat:@"%@%@",str,@"0000000000"];
    }else if (str.length == 7){
        result = [NSString stringWithFormat:@"%@%@",str,@"000000000"];
    }else if (str.length == 8){
        result = [NSString stringWithFormat:@"%@%@",str,@"00000000"];
    }else if (str.length == 9){
        result = [NSString stringWithFormat:@"%@%@",str,@"0000000"];
    }else if (str.length == 10){
        result = [NSString stringWithFormat:@"%@%@",str,@"000000"];
    }else if (str.length == 11){
        result = [NSString stringWithFormat:@"%@%@",str,@"00000"];
    }else if (str.length == 12){
        result = [NSString stringWithFormat:@"%@%@",str,@"0000"];
    }else if (str.length == 13){
        result = [NSString stringWithFormat:@"%@%@",str,@"000"];
    }else if (str.length == 14){
        result = [NSString stringWithFormat:@"%@%@",str,@"00"];
    }else if (str.length == 15){
        result = [NSString stringWithFormat:@"%@%@",str,@"0"];
    }else if (str.length == 16){
        return str;
    }
    return result;
}


+(NSString *)resultHexString:(NSString*)str maxLen:(int)maxLen{
    NSString *result = @"";
    
    return result;
}
//
//+(NSString *)addZero:(NSString *)str withLength:(int)length{
//    NSString *string = nil;
//    if (str.length==length) {
//        return str;
//    }
//
//    if (str.length<length) {
//        NSUInteger inter = length-str.length;
//        for (int i=0;i< inter; i++) {
//            string = [NSString stringWithFormat:@"0%@",str];
//            str = string;
//        }
//    }
//    return string;
//}

// 16进制字符串 -> 逆序16进制字符串
+ (NSString *)hexToReverseHex:(NSString *)hex{
    NSString * transferhex;
    if (hex.length % 2 == 1){
        transferhex = [NSString stringWithFormat:@"%@%@",@"0",hex];
    }else{
        transferhex = hex;
    }
    NSMutableArray * array = [NSMutableArray new];
    for(NSUInteger i=0;i<transferhex.length/2;i+=1){
        NSRange range = NSMakeRange(i*2, 2);
        NSString *sub = [transferhex substringWithRange:range];
        [array addObject:sub];
    }
    
    NSArray *nuarray = [[array reverseObjectEnumerator]allObjects];
    
    //    NSLog(@"%@",nuarray);
    
    NSMutableString * numbers = [[NSMutableString alloc]init];
    for(int i = 0;i<nuarray.count;i++){
        [numbers appendString:nuarray[i]];
    }
    return numbers;
}



// 16进制字符串 -> 2进制数据
+ (NSData *)convertHexStrToData:(NSString *)str {
    if (!str || [str length] == 0) {
        return nil;
    }
    
    NSMutableData *hexData = [[NSMutableData alloc] initWithCapacity:8];
    NSRange range;
    if ([str length] % 2 == 0) {
        range = NSMakeRange(0, 2);
    } else {
        range = NSMakeRange(0, 1);
    }
    for (NSInteger i = range.location; i < [str length]; i += 2) {
        unsigned int anInt;
        NSString *hexCharStr = [str substringWithRange:range];
        NSScanner *scanner = [[NSScanner alloc] initWithString:hexCharStr];
        
        [scanner scanHexInt:&anInt];
        NSData *entity = [[NSData alloc] initWithBytes:&anInt length:1];
        [hexData appendData:entity];
        
        range.location += range.length;
        range.length = 2;
    }
    return hexData;
}

@end

```
