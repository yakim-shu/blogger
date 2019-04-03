---
title: "【交作業】CS50 - week 1"
layout: post
description: "第一週的作業，我認為對沒寫過程式的初學者，單單是上完 Week 1 的內容，還是有點勉強，但也沒有不好，有難關才有想解決的動力。（應該吧）"
category: CS50
image: https://i.imgur.com/WMl6MDn.png
tags: [CS50]
file_name: 2019-04-03-CS50_week1_problem
---

## 作業

第一週的作業，我認為對沒寫過程式的初學者，單單是上完 Week 1 的內容，還是有點勉強，但也沒有不好，有難關才有想解決的動力。（應該吧）

2017 年學到 Week 4 ，但因為種種因素就放棄了。

過了兩年決定重頭開始，加上之前寫的筆記，算是比當時更清楚了課程內容。而且看到兩年前交的作業，發現那時連 Week 1 的作業都寫的慘不忍睹（雖然現在也是，但當時更糟），看到這麼久沒寫程式，還能有進步的自己是蠻開心的！

來這裡寫 Week 1 作業： [Problem Set 1](https://docs.cs50.net/2019/x/psets/1/index.html)

---

### Mario (less comfort)

目的：使印出任意高度、由 # 組成的金字塔。

![Mario](https://i.imgur.com/8a1oj9U.png)

```c
#include <cs50.h>
#include <stdio.h>

void print(int n);
    
int main(void)
{
    int i;
    do 
    {
        i = get_int("height: ");
    } while (i < 1 || i > 8);
    print(i);
}

void print(int n)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            if ( j >= n - i )
            {
                printf("#");
            } else
            {
                printf(" ");
            }
        }
        printf("\n");                  
    }        
}
```

---

### Mario 加強版 (more comfort) 
目的：使印出任意高度、由 # 組成的雙邊金字塔。

![Mario 加強版](https://i.imgur.com/WMl6MDn.png)

```c
#include <cs50.h>
#include <stdio.h>

void print(int n);
    
int main(void)
{
    int i;
    do 
    {
        i = get_int("height: ");
    } while (i < 1 || i > 8);
    print(i);
}

void print(int n)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            // left
            if ( j >= n - i )
            {
                printf("#");
            } 
            else
            {
                printf(" ");
            } 
           
            // right
            if ( j == n )
            {
                printf("  ");
                int h = j;
                while ( h > 0 )
                {                 
                    if ( h < n - i )
                    {
                        printf(" ");
                    }
                    else 
                    {
                        printf("#");
                    }
                    h--;
                }
            }
        }
        printf("\n");                  
    }        
}
```

---

### Cash 
- 目的
    - 輸入一數值 (浮點數)，美元換成美分 (乘100)
    - 算出 25, 10, 5, 1 美分硬幣的最少數量
- 感想： 寫得很累贅，用陣列應該會更簡單，但還沒教到


```c
#include <cs50.h>
#include <math.h>
#include <stdio.h>

int change(int n);

int main(void)
{
    float n = get_float("Change owed: ");
    int cents = round(n * 100);
    printf("%i\n", change(cents));
}

int change(int total)
{
    int n = 0;
    while ( total > 0)
    {
        if (total >= 25)
        {
            n += round(total / 25);
            total = total % 25;
        } 
        else if (total >= 10)
        {
            n += round(total / 10);
            total = total % 10;
        } 
        else if (total >= 5)
        {
            n += round(total / 5);
            total = total % 5;
        } 
        else if (total >= 1)
        {
            n += round(total / 1);
            total = total % 1;
        }        
    }
    return n; 
}
```

---

### Credit 
- 目的：用公式驗算信用卡卡號，並查證出是哪種卡別
- 感想：不算很難，但過程有點麻煩
- 學到
    - 怎麼取數字的個數
    - 太長的數字要用 `long` or `long long`
    - 如何取前幾位數 or 後幾位數

    
```c
/*
American Express: 15 digit, start with 34, 37
MasterCard: 16 digit, start with 51, 52, 53, 54,55
Visa: 13 or 16 digit, start with 4
1. 先算出幾位數
2. 等於 16，看開頭是不是等於 51 - 55，或者為 4
3. 等於 15，看開頭是不是等於 34 or 37
3. 等於 13，看開頭是不是等於 4
-----
測試：
1. 基數位 * 2 ( 如果超過10 ，個位數跟十位數相加 )
2. 相加結果 n
3. 偶位數相加 再加上 n
4. n 如果為 10 的倍數，卡片號碼就成立
*/

# include <cs50.h>
# include <stdio.h>
# include <math.h>

int check(long total);
int count(long n);
string card_type(long n, int i);

string s_invalid = "INVALID";
int main(void)
{
    long n = get_long("card: ");
    int i = count(n);
    if ( check(n) % 10 == 0 )
    {
        printf("%s\n", card_type(n, i));
    }
    else
    {
        printf("%s\n", s_invalid);
    }
}

// 驗證卡號
int check(long total)
{
    int amount = 0;
    while( total >= 1 )
    {
        // 步驟 3. 偶位數相加   
        int even = total % 10;
        amount += even;
        total = round(total/10);
        
        // 步驟 1, 2. 基數位相加
        int odd = (total % 10) * 2;
        if (odd >= 10)
        {
            odd = (odd % 10) + round(odd/10);
        }
        amount += odd;
        total = round(total/10);
    }
//     printf("total: %i\n", amount);
    return amount;
}


// 先算有幾位數
int count(long n)
{
    int i = 0;
    while( n >= 1)
    {
        n = n / 10;
        i++;
    }
    return i;
}


// 判斷為哪種卡
string card_type(long n, int i)
{
    // 先算出前兩個數字
    while ( n >= 100 )
    {
        n = round(n / 10);
    }
    
    // 判斷
    switch (i)
    {
        case 16:
            if ( round(n / 10) == 4 )
            {
                return "VISA";
            } 
            else if ( (n >= 51) && (n <= 55) )
            {
                return "MASTERCARD";               
            }
            return s_invalid;
        case 15:
            if ( n == 34 || n == 37)
            {
                return "AMEX";
            }
            return s_invalid;
        case 13:
            if ( round(n / 10) == 4 )
            {
                return "VISA";
            } 
            return s_invalid;
        default :
            return s_invalid;
    }
}
```