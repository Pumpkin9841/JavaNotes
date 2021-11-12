---
title: leetcode 1603. 设计停车系统
date: 2021-03-19 16:06:44.895
updated: 2021-03-20 16:06:46.699
url: https://pumpkn.xyz/archives/leetcode1603she-ji-ting-che-xi-tong
categories: 算法
tags: 算法
---

# 题目
## [原题链接](https://leetcode-cn.com/problems/design-parking-system/)
![image.png](https://pumpkn.xyz/upload/2021/03/image-8fbebaced9924e72984c02a01f865872.png)
# 代码
```java
class ParkingSystem {

    int big , medium , small ;

    public ParkingSystem(int big, int medium, int small) {
        this.big = big ;
        this.medium = medium ;
        this.small = small ;
    }
    
    public boolean addCar(int carType) {
        if(carType == 1){
            if( big > 0 ){
                big--;
                return true ;
            }
        }
        if( carType == 2 ){
            if( medium > 0 ){
                medium -- ;
                return true ;
            }
        }

        if( carType == 3  ){
            if( small > 0 ){
                small-- ;
                return true ;
            }
        }
        return false ;
    }
}

/**
 * Your ParkingSystem object will be instantiated and called as such:
 * ParkingSystem obj = new ParkingSystem(big, medium, small);
 * boolean param_1 = obj.addCar(carType);
 */
```