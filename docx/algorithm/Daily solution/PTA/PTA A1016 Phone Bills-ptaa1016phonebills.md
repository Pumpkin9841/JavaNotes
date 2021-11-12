---
title: PTA A1016 Phone Bills
date: 2021-06-05 18:00:36.938
updated: 2021-06-05 18:00:36.938
url: https://pumpkn.xyz/archives/ptaa1016phonebills
categories: 算法
tags: 算法 | 排序
---

# 题目
## [原题链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805493648703488)
![image.png](https://pumpkn.xyz/upload/2021/06/image-890d6b7856e348a0a203b92b362b9e6a.png)
# 题意
给出24h中每个小时区间内的资费（cents/minute），并给出N个通话记录点，每个通话记录点都记录了姓名、当前时刻（月：日：时：分）以及其属于通话开始（on-line）或是通话结束（off-line）。现在需要对每个人的有效通话记录进行资费计算，有效通话记录是指同一个用户能够配对的所有on-line和off-line，而这样的配对需要满足：在按时间顺序排列后，两条配对的on-line和off-ine对应时间内不允许出现其他on-line或者off-line的记录。</br>
输出要求：按姓名的字典序从小到大的顺序输出存在有效通话记录的用户。对单个用户来说，需要输出他的姓名、账单月份（题目保证单个用户的所有记录都在同一个月产生）以及有效通话记录的时长和花费，最后输出他的总资费。注意：资费的输出需要进行单位换算，即把cent换算为dollar，所以结果要除以100。
# 题解
整个算法分为三部分：对所有记录排序；对每个用户判断其是否存在有效通话记录；如果存在有效通话记录，则输出所有有效通话记录并计算资费。对应的步骤如下。</br>
步骤1：先以结构体类型Record存放单条记录的用户名、月、日、时、分以及通话状态（on-line还是off-line），然后根据题目需要对所有记录按下面的规则进行排序。</br>
1. 如果用户名不同，则按用户名字典序从小到大排序。
2. 否则，如果月份不同，则按月份从小到大排序。
3. 否则，如果日期不同，则按日期从小到大排序。
4. 否则，如果小时不同，则按小时从小到大排序。
5. 否则，如果分钟不同，则按分钟从小到大排序。


步骤2：由于均已将所有记录先按姓名字典序排序，因此同一个用户的记录在数组中是连续且按时间顺序排列的。考虑到有效通话记录必须保证on-line和off-line在记录中先后出现，因此可以采用如下的方法确定该用户是否存在有效通话记录：设置int型变量needPrint表示该用户是否存在有效通话记录，初值为0。遍历该用户的所有记录，如果在needPrint为0的情况下遇到on-line的记录，就把needPrint置为1；如果在needPrint为1的情况下遇到offline的记录，就把needPrint置为2，这样当遍历结束时，如果needPrint为2，则说明该用户存在有效通话记录。而为了步骤3的书写更方便，可以同时设置int型变量next，表示下一个用户的首记录在数组中的下标，并在遍历过程中对其进行累加，这样当遍历结束时，就得到next作为下一个用户的标识了。</br>

步骤3：输出所有有效通话记录。步骤2中已经分析过，有效通话记录必须保u on-ine和off-line在记录中先后出现，这在数组中的含义是"on-line的数组下标与off-line的数组下标只相差1时才能进行配对”。也就是说，只要反复寻找当前下标i与其下一个元素的下标i+
1满足前者为on-line而后者off-line的记录即可。这样，找到的下标i和i + 1就是配对的有效通话记录，将它们输出即可。</br>

步骤4：最后提一下时长的计算方法（计算资费的方法相同）。对已知的起始时间和终止时间，只需要不断将起始时间加1，判断其是否到达终止时间即可。

# 代码
```c++
#include <cstdio>
#include <cstring>
#include <string>
#include <set>
#include <map>
#include <vector>
#include <cmath>
#include <algorithm>
#include <iostream>
#define ll long long
#define eps 1e-8
#define INF 0x7FFFFFFF
 
using namespace std;
 
// 24小时话费
int cents_per_min[24] = {0};
 
// 时间结构体
struct CallTime {
    int month, day, hour, minute;
};
 
// 通话记录结构体
struct CallRecord {
    string name;
    CallTime call_time;
    bool online; // 记录类型 1上线 0下线
};
 
bool cmp(CallRecord r1, CallRecord r2) {
    if (r1.name == r2.name) {
        // return r1.call_time < r2.call_time
        if (r1.call_time.day < r2.call_time.day) return true;
        else if(r1.call_time.day > r2.call_time.day)return false;
        
        if (r1.call_time.hour < r2.call_time.hour) return true;
        else if(r1.call_time.hour > r2.call_time.hour) return false;
        
        if (r1.call_time.minute < r2.call_time.minute) return true;
        else if(r1.call_time.minute > r2.call_time.minute)return false;
    }
    return r1.name < r2.name;
}
 
// 求出两个时间的分钟差
int time_sub(CallTime t1, CallTime t2) {
    int min1, min2;
    min1 = t1.day*24*60 + t1.hour*60 + t1.minute;
    min2 = t2.day*24*60 + t2.hour*60 + t2.minute;
    return abs(min1-min2);
}
 
// 求出某个电话记录的话费
int phone_cost(CallTime t1, CallTime t2) {
    int ret = 0;
    while (!(t1.day == t2.day && t1.hour == t2.hour && t1.minute == t2.minute)) {
        if (t1.day == t2.day && t1.hour == t2.hour) {
            ret += cents_per_min[t1.hour]*(t2.minute-t1.minute);
            t1.minute = t2.minute;
        }
        else {
            ret += cents_per_min[t1.hour]*(60-t1.minute);
            t1.minute = 0;
            if (t1.hour == 23) {
                t1.hour = 0;
                t1.day ++;
            }
            else {
                t1.hour ++;
            }
        }
    }
    return ret;
}
 
int main() {
    // 输入cents per minute
    for (int i = 0; i < 24; i ++) {
        cin >> cents_per_min[i];
    }
    // 输入记录个数
    int n;
    cin >> n;
    // 对每条记录进行存储
    vector<CallRecord> records;
    for (int i = 0; i < n; i ++) {
        // 将输入信息先存放到record里
        CallRecord record;
        string name, call_time, type;
        cin >> name >> call_time >> type;
        record.name = name;
        const char *s = call_time.data();
        sscanf(s, "%d:%d:%d:%d", &record.call_time.month, &record.call_time.day, &record.call_time.hour, &record.call_time.minute);
        if (type == "on-line") record.online = true;
        else record.online = false;
        // 将记录存入vector
        records.push_back(record);
    }
    
    // 按照先字母表，后时间前后进行排序
    sort(records.begin(), records.end(), cmp);
    
    int sum = 0; // 记录总开销
    bool last_online = false; // 记录上一条记录是否是online
    CallTime begin_time, end_time; // 一条通话记录的开始和结束时间
    map <string, int> idx; // 建立顾客姓名和姓名编号的映射
    int name_cnt = 0; // 顾客姓名数量
    for (int i = 0; i < n; i ++) {
        CallRecord record = records[i];
        // 如果是上线记录，则记录开始电话的时间
        if (record.online) {
            begin_time = record.call_time;
            last_online = true;
        }
        // 如果是下线记录，且上条记录是上线记录
        else if (last_online && !record.online) {
            end_time = record.call_time;
            last_online = false;
            // 计算分钟数和花费数
            int minutes = time_sub(begin_time, end_time);
            int rec_cost = phone_cost(begin_time, end_time);
            sum += rec_cost;
            // 第一次时打印名字
            if(idx[record.name] == 0) {
                idx[record.name] = ++name_cnt;
                cout << record.name << " ";
                printf("%02d\n", record.call_time.month);
            }
            // 打印通话记录
            printf("%02d:%02d:%02d ", begin_time.day, begin_time.hour, begin_time.minute);
            printf("%02d:%02d:%02d ",end_time.day, end_time.hour, end_time.minute);
            printf("%d $%.2lf\n", minutes, 1.0*rec_cost/100);
        }
        
        bool customer_end = false;
        if (i == n-1) customer_end = true;
        else if (record.name != records[i+1].name) customer_end = true;
        if (customer_end) {
            if (sum != 0) printf("Total amount: $%.2lf\n", 1.0*sum/100);
            sum = 0;
            last_online = false;
        }
        
    }
    return 0;
}
```