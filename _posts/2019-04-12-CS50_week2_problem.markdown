---
title: "【交作業】CS50 - week 2"
layout: post
description: "第三題光是理解題目就花了一個上午，加上冒出很多不懂的名詞，為了查清楚 1 個又冒出 3 個不懂的，真正開始實作時也整個卡住，完全不知道自己在寫什麼，這樣卡關下去也不是辦法，決定先放棄，之後變強再回來解題。"
category: CS50
image: 
tags: [CS50]
file_name: 2019-04-12-CS50_week2_problem
---

課程連結： [Problem 2](https://docs.cs50.net/2019/x/psets/2/index.html)

作業測試：[submissions](https://cs50.me/submissions)（給自己看的）

---

### Caesar 加密訊息 (less comfort)

功能：將輸入的 plaintext 加上 key，輸出成 ciphertext

判斷大小寫並加密的那部分並不難實現，只是比較麻煩而已，但判斷輸入的 `key` 值是不是純數字反而讓我卡了一陣子，不確定回傳 `return 1` 是否恰當。


```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int check_key(string key);
void change_text(string text, int key);

int main(int argc, string argv[])
{
    if (argc == 2)
    {
        // string > int
        int key = check_key(argv[1]); 
        // key is a number
        if (key != 0)
        {
            string target_text = get_string("plaintext:  ");
            change_text(target_text, key);
        }
    }
    else
    {
        printf("Usage: ./caesar key\n");
        return 1;
    }
}

// Check key is a number && make sure key is less than 26
int check_key(string key)
{
    int key_num;
    for(int i = 0, n = strlen(key); i < n; i++)
    {
        // char > int
        key_num = (int) key[i];
        // key is not a number (0 ~ 9)
        if (!(key_num > 48 && key_num < 57))
        {
            printf("Usage: ./caesar key\n");
            return 0;
        }
    }   
    // string > int
    key_num = atoi(key);
    // change key_num to less than 26
    return key_num % 26;
}

// Change plaintext
void change_text(string text, int key)
{
    int UPPER_A = 65;
    int UPPER_Z = 90;
    int LOWER_A = 97;
    int LOWER_Z = 122;
    
    printf("ciphertext: ");
    for(int i = 0, n = strlen(text); i < n; i++)
    {
        // char > int
        int target = (int) text[i]; 
        // Lowercase
        if ( target >= LOWER_A && target <= LOWER_Z)
        {
            target += key;
            if (target > LOWER_Z)
            {
                target = (target % LOWER_Z) + (LOWER_A - 1); // start from 0
            }
        }
        // Uppercase
        else if ( target >= UPPER_A && target <= UPPER_Z)
        {
            target += key;
            if (target > UPPER_Z)
            {
                target = (target % UPPER_Z) + (UPPER_A - 1); // start from 0
            }  
        }
        printf("%c", target);
    }
    printf("\n");
}
```

---

## vigenere 用 keyword 加密訊息

又是個輕敵的狀況，以為只要把上題的 key 轉成 ASCII 就好，結果中間還是有些眉角要注意。

尤其是當 key 有數字的情況下，一開始做運算都會算錯，後來才想起數字還是要換成 ASCII 碼的數字，可惡啊！陷阱太多了。

不知道為什麼，提交測試最後一個沒過，一如往常的找不出原因就放棄的我。（哭）

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

void change_letter(string key, string text);
string check_letter(char letter);
int shift(char c);

int UPPER_A = 65;
int UPPER_Z = 90;
int LOWER_A = 97;
int LOWER_Z = 122;
int ASCII_ONE = 48;
int ASCII_NINE = 57;

int main(int argc, string argv[])
{
    if (argc == 2)
    {
        string plain_text = get_string("plaintext:  ");
        change_letter(argv[1], plain_text);
        return 0;
    }
    else
    {
        printf("Usage: ./vigenere keyword\n");
        return 1;
    }
}

// 加密 plaintext
void change_letter(string key, string text)
{
    printf("ciphertext: ");
    int k = 0;
    for(int i = 0, n = strlen(text); i < n; i++)
    {
        // 循環 key loop
        if (k >= strlen(key))
        {
            k = 0;  
        }
        int add = shift(key[k]);
        
        // plaintext 加上轉換過的 key
        int text_num = (int) text[i];
        
        // 字元超過範圍再做處理
        if ( strcmp(check_letter(text[i]), "upper") == 0)
        {
            k++;
            text_num += add;
            if ( text_num > UPPER_Z)
            {
                text_num = text_num - UPPER_Z + (UPPER_A-1);
            }
        }
        else if ( strcmp(check_letter(text[i]), "lower") == 0)
        {
            k++;
            text_num += add;
            if ( text_num > LOWER_Z)
            {
                text_num = text_num - LOWER_Z + (LOWER_A-1);
            }
        }
        printf("%c", text_num);        
    }   
    printf("\n");
}

// 轉換 key > int
int shift(char c)
{
    int key_num = (int) c;
    // strcmp(string1, string2), 2 字串相等會 return 0
    if ( strcmp(check_letter(c), "upper") == 0)
    {
        // A 轉成 0 開始
        key_num %= UPPER_A;
        return key_num;
    }
    else if ( strcmp(check_letter(c), "lower") == 0)
    {
        // a 轉成 0 開始
        key_num %= LOWER_A;
        return key_num;
    }
    // key_num is a number (0 ~ 9)
    // 陷阱：要把數字的 ASCII 轉回 int
    else if (key_num >= ASCII_ONE && key_num <= ASCII_NINE)
    {
        key_num %= ASCII_ONE;
        return key_num;
    }
    else
    {
        printf("Usage: ./vigenere keyword"); 
        return 1;
    }
    
}

// 檢查字元大小寫
string check_letter(char target)
{
    int letter = (int) target;
    if (letter >= UPPER_A && letter <= UPPER_Z)
    {
        return "upper";
    } 
    else if (letter >= LOWER_A && letter <= LOWER_Z)
    {
        return "lower";
    }
    else
    {
        return "non-alphbet";
    }
}
```

---

## Crack 破解金鑰

在任何登入帳號密碼系統時，通常系統儲存的不會是**明碼**（意指你看得到密碼是多少），他們會用某種函數將密碼轉成 hashes（一連串亂碼），當申請帳號成功後，會存著這組 hashes，之後要登入時，比對 hashes 是否跟申請帳號的那組一致。

所以當我們按下忘記密碼的連結時，通常是會寄給你一封信重置你的密碼，而不是直接告訴你密碼到底是多少，這是為了保護使用者安全，系統也沒有儲存著明碼，但可以重置密碼。

當然這種方法要破解也不難，還是可以靠猜測、或不斷地嘗試所有可能性來攻破，也稱作 brute force 暴力攻擊法，但密碼要是很長一串，電腦要花的時間也會非常長。

> brute force 暴力攻擊法，參考： [[資訊安全]brute-force attack(暴力攻擊法)](https://dotblogs.com.tw/jimmyyu/archive/2009/10/10/10997.aspx)


這題要考驗的也就是 **實作暴力攻擊法** 。不過因為每增加一個字元，運算的時間也會多非常多，題目算是很有人性的訂在 4 個字元。（才怪）

有興趣的暸解更多的可以看這影片 [Password Cracking - Computerphile](https://www.youtube.com/watch?v=7U-RbOKanYs) （主講人長得很像 Tobey Maguire，蜘蛛人在講解程式的感覺很幽默）

---

### 感到太難所以半途而廢的我

光是理解題目就花了一個上午，加上冒出很多不懂的名詞，為了 **查清楚 1 個又冒出 3 個不懂的** 一直循環下去，真正開始實作時也整個卡住，完全不知道自己在寫什麼，這樣卡關下去也不是辦法，決定先放棄，之後變強再回來解題。


### 還是留下亂七八糟的筆記給未來的自己看

題目提供的資訊：

- `man crypt`是一個課程提供的 function，會幫明碼轉成一連串隨機字元
    - 宣告： `char *crypt(const char *key, const char *salt);`
    - argument 之一的 `salt`
        - 參考維基：[鹽-密碼學](https://zh.wikipedia.org/wiki/盐_(密码学))
        - 作用：像是做菜要加鹽，加密後丟入前 2 碼的 `salt`，等於是說密碼加入隨機亂碼，增加複雜性
        - 疑問：
            - 給的範例幾乎都是 50，是說先從 50 開始驗證嗎？
- 條件說明 
    - CLI 只接受一個 argument: a hashed password
    - 如果沒有 argument 或超過一個以上，回傳錯誤訊息、立刻離開 `return 1`
    - 不斷得印出**明碼**`\n`，找到回傳 `return 0`
    - 確認每個明碼都呼叫 `crypt` 程式進行 hashed 作業
    - 假定每個密碼都不超過 4 個字元
    - 假定每個密碼都只有英文字母（大、小寫）

參考資料：[網站密碼 - hash + salt](https://sites.google.com/site/stevenattw/others/wang-zhan-mi-ma---hash-salt)

---

### 思考方向

所以我要做的可能是 ：

1. 列舉所有可能性的字串
2. 丟進 `crypt` 處理成 11 碼 hashes（加上 `salt` 後共 13 碼）
3. 再比對 input 跟 hashes 是否相同 
    - 有，回傳 0
    - 沒有，回到步驟 (3) ，繼續找直到全部列舉完

需要列舉的：

- 明碼的可能性有 52⁴ 種
    - 4 個字元，各有 52 種可能性
        - 大寫、小寫字母
    - 思考
        - **字典攻擊法可能更快**
- salt 的可能性有 62² 種
    - 每個字元有 [ 0-9 A-Z a-z ] 62種可能性
        - 大寫、小寫、數字
    - 思考
        - 很常出現 **50** ?

    